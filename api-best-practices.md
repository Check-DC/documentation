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
| Remove(Soft Delete)       | `POST`    | `/{resource}/delete`       | `/users/delete`                   |             |

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
GET /users?page=1&limit=20&status=active&sortBy=createdAt&order=desc
```


### Remove (Soft Delete) Requests

For batch operations or soft deletes:

```
// POST /users/delete
{
  "ids": [1, 2, 3],
  "reason": "User requested account deletion",
  "notify": true
}
```


## 4. Response Structure
Single Resource Response

```
{
  "data": {
    "id": 123,
    "name": "John Doe",
    "email": "john@example.com",
    "createdAt": "2023-12-01T10:30:00Z"
  }
}
```
