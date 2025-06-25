# 📘 Pop.site API Documentation – MemberSpace Integration

** Test URL:** `https://dev-api.pop.site`

** Base URL:** `https://api.pop.site`  

> All requests must be authenticated using a Bearer Token, which will be provided by the Pop.site team.

---

## 🧪 1. Validate Username

### `GET /validate-username`

Verifies if a username (used as a subdomain) is valid and available.

### Query Parameters

| Name     | Type   | Required | Description                                                                 |
|----------|--------|----------|-----------------------------------------------------------------------------|
| username | string | Yes      | Desired subdomain. Must be lowercase, 3–63 chars, alphanumeric or hyphens. |

### Headers

| Header        | Required | Example                     |
|---------------|----------|-----------------------------|
| Authorization | Yes      | Bearer `your_api_token_here`|

### Response

#### ✅ 200 OK

```
{
  "valid": true,
  "available": true,
  "message": "Username is available"
}
```

#### ❌ 400 Bad Request (Invalid format)

```
{
  "valid": false,
  "available": false,
  "message": "Invalid username. Must be 3–63 characters, lowercase letters, numbers or hyphens, and must not start or end with a hyphen."
}
```

#### ❌ 409 Conflict (Already taken)

```
{
  "valid": true,
  "available": false,
  "message": "Username is already taken"
}
```

---

## 🏗 2. Create MemberSpace Website

### `POST /create-ms-website`

Creates a new Firebase Auth user (if necessary) and deploys a website with the provided subdomain.

### Request Body

```json
{
  "username": "testing123",
  "email": "user@example.com"
}
```

| Field     | Type   | Required | Description                                                       |
|-----------|--------|----------|-------------------------------------------------------------------|
| username  | string | Yes      | Must be valid per validation rules used in `/validate-username`.  |
| email     | string | Yes      | Valid email of the user to associate with the website.            |
| cdn_domain| string | No       | Cdn domain to use in the script.                                  |

### Headers

| Header        | Required | Example                     |
|---------------|----------|-----------------------------|
| Authorization | Yes      | Bearer `your_api_token_here`|
| Content-Type  | Yes      | `application/json`          |

### Response

#### ✅ 200 OK

```
{
  "success": true,
  "userId": "UqQ23voRS9PJDTstehv9xYbHQlX2",
  "username": "testing123",
  "siteUrl": "https://0000.pop.site"
}
```
(IMPORTANT! ❌, your siteUrl will be something like "https://dev.0000.pop.site" for test environment.)

#### ❌ 400 Bad Request

```
{
  "success": false,
  "message": "Invalid email format"
}
```

#### ❌ 409 Conflict

```
{
  "success": false,
  "message": "Username is already taken"
}
```

#### ❌ 500 Internal Server Error

```
{
  "success": false,
  "message": "An unexpected error occurred"
}
```

---

## 🔐 Authentication

All requests must include the following header:

```
Authorization: Bearer <API_TOKEN>
```

If omitted or invalid, the server will respond with `401 Unauthorized`.

---

## 📞 Support

For assistance or integration help, contact: **daniel@pop.site**
