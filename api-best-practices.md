# API Best Practices

This document defines the **request and response standards** between backend services and the frontend.  
Following these conventions ensures consistency, predictable behavior, and seamless integration across all applications.

---

## 1. Resource Naming

- Use **plural nouns** for resource paths.  
  - ✅ `/users`  
  - ✅ `/products`  
  - ❌ `/user`, `/product`

---

## 2. Standard Endpoints

Every REST resource should expose a consistent set of endpoints so the frontend can interact with it:

| Operation                  | HTTP Method | Path                     | Example                        |
|----------------------------|-------------|--------------------------|--------------------------------|
| Get All (with pagination)  | `GET`       | `/{resource}`            | `/users?page=1&limit=10`       |
| Get One                    | `GET`       | `/{resource}/{id}`       | `/users/123`                   |
| Create                     | `POST`      | `/{resource}/create`     | `/users/create`                |
| Update (PUT)               | `PUT`       | `/{resource}/{id}/update` | `/users/123/update`           |
| Partial Update (PATCH)     | `PATCH`     | `/{resource}/{id}/update` | `/users/123/update`           |
| Delete       | `DELETE`    | `/{resource}/{id}`       | `/users/123`                   |             |

---

## 3. Request Structure

### Create & Update Requests

- Body must be JSON.
- For **create**, accept required resource fields.
- For **update/edit**, accept partial or full fields.

```
// POST /users/create
{
  "name": "John Doe",
  "email": "john@example.com"
}

// PUT /users/123/update
{
  "name": "Jane Doe"
}
```

### Query Parameters for Get All

Support filtering, pagination, and sorting parameters:

```
GET /users?page=1&limit=20&status=active&sort_by=created_at&order=desc
```

## 4. Response Structure

All API responses must be wrapped in a consistent structure with a `data` field containing the actual response payload.

### Single Resource Response

For individual resources (GET by ID, POST create, PUT/PATCH update):

```json
{
    "data": {
        "id": "01jzp4xrspbxrtqn0n691ngvzg",
        "first_name": "John",
        "last_name": "Doe",
        "email": "johndoe@gmail.com",
        "phone": "2347064463916",
        "business_name": "Doe's ventures",
        "business_type": "Cosmetics",
        "business_address": "No 4, unilag road, yaba"
    }
}
```

### Status/Action Response

For operations that return status messages or confirmation:

```json
{
    "data": {
        "status": true,
        "message": "Item added/updated in cart successfully"
    }
}
```

### Collection Response (with Pagination)

For list endpoints that return multiple resources:

```json
{
    "data": [
        {
            "id": "01k0egx1yprf00vywd14dms57a",
            "manufacturer": "Joseph",
            "sku_id": "sku_236",
            "sku_code": "sku_code_1",
            "name": "Schneider-Armstrong",
            "description": "Vitae fugit id iusto commodi qui ipsa.",
            "category": "Dairy Products",
            "unit_price": 73.44,
            "unit_of_measure": "kg",
            "case_size": 189,
            "image_url": "https://via.placeholder.com/640x480.png/0077cc?text=sed",
            "is_active": true
        }
    ],
    "links": {
        "first": "http://city-distro-core.test/api/products?page=1",
        "last": "http://city-distro-core.test/api/products?page=1",
        "prev": null,
        "next": null
    },
    "meta": {
        "current_page": 1,
        "from": 1,
        "last_page": 1,
        "links": [
            {
                "url": null,
                "label": "&laquo; Previous",
                "active": false
            },
            {
                "url": "http://city-distro-core.test/api/products?page=1",
                "label": "1",
                "active": true
            },
            {
                "url": null,
                "label": "Next &raquo;",
                "active": false
            }
        ],
        "path": "http://city-distro-core.test/api/products",
        "per_page": 10,
        "to": 1,
        "total": 1
    }
}
```

---

## 5. Validation

Use array syntax for validation rules instead of pipe-separated strings for better readability and maintainability.

### ✅ Preferred Array Syntax

```php
$validator = Validator::make($request->all(), [
    'first_name' => ['sometimes', 'string', 'max:255'],
    'last_name' => ['sometimes', 'string', 'max:255'],
    
    // Profile fields
    'bio' => ['sometimes', 'string', 'max:1000'],
    'job_title' => ['sometimes', 'string', 'max:255'],
    'company' => ['sometimes', 'string', 'max:255'],
    'experience_level' => ['sometimes', 'string', 'max:255'],
    'skills' => ['sometimes', 'array'],
    'skills.*' => ['string', 'max:100'],
]);
```

### ❌ Avoid Pipe-Separated Syntax

```php
$validator = Validator::make($request->all(), [
    'first_name' => 'sometimes|string|max:255',
    'last_name' => 'sometimes|string|max:255',
    'bio' => 'sometimes|string|max:1000',
    // ...
]);
```

---

## 6. Data Resources

Always return data through Laravel Resource classes for consistent formatting and transformation.

### Controller Implementation

```php
$user = $request->user()->load('profile');

return new UserResource($user);
```

### Resource Class Example

```php
<?php

namespace App\Http\Resources;

use Illuminate\Http\Request;
use Illuminate\Http\Resources\Json\JsonResource;

class UserResource extends JsonResource
{
    /**
     * Transform the resource into an array.
     *
     * @return array<string, mixed>
     */
    public function toArray(Request $request): array
    {
        return [
            'id' => $this->id,
            'name' => $this->name,
            'first_name' => $this->first_name,
            'last_name' => $this->last_name,
            'email' => $this->email,
            'profile_photo_url' => $this->profile_photo_url,
            'profile' => ProfileResource::make($this->whenLoaded('profile')),
        ];
    }
}
```

### Benefits

- **Consistent formatting** across all endpoints
- **Eager loading control** with `whenLoaded()`
- **Data transformation** in a centralized location
- **Easy maintenance** when response structure changes

---

## 7. Query Handling & Collections

Use query builders with proper filtering, pagination, and eager loading for collection endpoints.

```php
$query = Order::query()
    ->where('retailer_id', auth()->id())
    ->with(['orderItems.product']);
$perPage = $request->per_page ?? 10;

// Filter by status
if ($request->has('status')) {
    $statuses = $request->input('status');
    $query->whereIn('status', $statuses);
}

// Filter by date range
if ($request->has('date_from')) {
    $query->whereDate('created_at', '>=', $request->date_from);
}

if ($request->has('date_to')) {
    $query->whereDate('created_at', '<=', $request->date_to);
}

// Filter by month
if ($request->has('month')) {
    $month = $request->month;
    $query->whereYear('created_at', substr($month, 0, 4))
        ->whereMonth('created_at', substr($month, 5, 2));
}

$orders = $query->latest()->paginate($perPage);

return OrderResource::collection($orders)->response();
```

### Key Principles

- **Start with query builder** using `Model::query()`
- **Apply authorization filters** early (e.g., `retailer_id`)
- **Eager load relationships** with `with()` to prevent N+1 queries
- **Default pagination** with configurable `per_page`
- **Conditional filtering** using `$request->has()`
- **Use resource collections** for consistent formatting
- **Call `->response()`** on collections for proper pagination metadata

---

## 8. Transaction Locking (Preventing Race Conditions)

When handling operations that modify shared resources (e.g., inventory, wallet balances, order processing), use proper locking mechanisms to prevent race conditions and double spending issues.

### Pessimistic Locking

Pessimistic locking uses database row locks (`FOR UPDATE`) to prevent concurrent access during a transaction. This is ideal for critical operations where conflicts are likely.

```php
use Illuminate\Support\Facades\DB;

DB::transaction(function () use ($userId, $amount) {
    // Lock the user's wallet row until transaction completes
    $wallet = Wallet::where('user_id', $userId)
        ->lockForUpdate()
        ->first();

    if ($wallet->balance < $amount) {
        throw new \Exception('Insufficient balance');
    }

    // Deduct amount - no other transaction can modify this row
    $wallet->balance -= $amount;
    $wallet->save();

    // Create transaction record
    Transaction::create([
        'wallet_id' => $wallet->id,
        'amount' => $amount,
        'type' => 'debit',
    ]);
});
```

### Atomic Operations

For simple increment/decrement operations, use atomic updates that execute in a single database query, eliminating race conditions without explicit locks.

```php
// ❌ Unsafe - prone to race conditions
$product = Product::find($productId);
$product->stock -= $quantity;
$product->save();

// ✅ Safe - atomic operation
Product::where('id', $productId)
    ->where('stock', '>=', $quantity)
    ->decrement('stock', $quantity);

// Verify the update succeeded
$affected = Product::where('id', $productId)
    ->where('stock', '>=', $quantity)
    ->decrement('stock', $quantity);

if ($affected === 0) {
    throw new \Exception('Insufficient stock or product not found');
}
```

### Combining Both Approaches

For complex operations involving multiple models, combine pessimistic locking with atomic operations:

```php
DB::transaction(function () use ($orderId, $items) {
    // Lock the order to prevent duplicate processing
    $order = Order::where('id', $orderId)
        ->where('status', 'pending')
        ->lockForUpdate()
        ->firstOrFail();

    foreach ($items as $item) {
        // Atomically decrement stock
        $affected = Product::where('id', $item['product_id'])
            ->where('stock', '>=', $item['quantity'])
            ->decrement('stock', $item['quantity']);

        if ($affected === 0) {
            throw new \Exception("Insufficient stock for product {$item['product_id']}");
        }
    }

    // Update order status
    $order->status = 'confirmed';
    $order->save();
});
```

### When to Use Each Approach

| Scenario | Approach | Reason |
|----------|----------|--------|
| Single numeric field update | Atomic Operations | Fastest, no locks needed |
| Multiple related updates | Pessimistic Locking | Ensures consistency across operations |
| High contention operations | Pessimistic Locking | Prevents conflicts explicitly |
| Read-then-write logic | Pessimistic Locking | Prevents stale reads |
| Simple counters | Atomic Operations | Most efficient for increments/decrements |

### Best Practices

- **Always use transactions** when multiple operations must succeed together
- **Lock only what you need** - be specific with WHERE clauses
- **Keep transactions short** - acquire locks, do work, release quickly
- **Handle deadlocks** - wrap in try-catch and consider retry logic
- **Test under load** - race conditions only appear under concurrent access
- **Use atomic operations** when possible - they're faster and simpler than locks

```php
// Handling potential deadlocks with retry logic
$maxRetries = 3;
$attempt = 0;

while ($attempt < $maxRetries) {
    try {
        DB::transaction(function () {
            // Your locked operations here
        });
        break; // Success, exit loop
    } catch (\Illuminate\Database\QueryException $e) {
        if ($e->getCode() === '40001') { // Deadlock error code
            $attempt++;
            if ($attempt >= $maxRetries) {
                throw $e;
            }
            usleep(100000); // Wait 100ms before retry
        } else {
            throw $e;
        }
    }
}
```
