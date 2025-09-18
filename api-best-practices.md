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
| Update (PUT)               | `PUT`       | `/{resource}/{id}/update` | `/users/123/update`            |
| Partial Update (PATCH)     | `PATCH`     | `/{resource}/{id}/update` | `/users/123/update`            |
| Delete       | `DELETE`    | `/{resource}/{id}`       | `/users/123`                   |               |

---