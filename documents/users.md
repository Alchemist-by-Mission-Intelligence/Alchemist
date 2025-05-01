Here's the **Users Module** documentation in production-ready **Markdown format**, including full support for OAuth, social logins, and necessary relationships:

---

# 🧑‍💼 Users Module – Alchemist AI

This is the foundational module for user management within the Alchemist AI SaaS boilerplate. It includes registration, login, OAuth integration, social logins, multi-tenant support, and ties into the roles and permissions system.

---

## 📦 Features

- User registration/login/logout
- Email verification & password reset
- OAuth (JWT/Personal Access Tokens)
- Social login (Google, GitHub, Twitter, etc.)
- Role and permission assignment
- Multi-tenant compatibility
- User status (active/banned/suspended)
- API key generation
- 2FA (Two-Factor Authentication)
- Email preferences
- Profile management

---

## 🧾 Database Schema

### Table: `users`

| Field              | Type             | Description                            |
|-------------------|------------------|----------------------------------------|
| id                | UUID / BIGINT    | Primary key                            |
| name              | STRING           | Full name                              |
| email             | STRING (unique)  | User email                             |
| password          | STRING (nullable)| Hashed password (null if social login) |
| avatar            | STRING (nullable)| Profile picture URL                    |
| provider          | STRING (nullable)| Social login provider (e.g. google)    |
| provider_id       | STRING (nullable)| Social ID from provider                |
| email_verified_at | TIMESTAMP        | Email verification timestamp           |
| status            | ENUM             | active / banned / suspended            |
| is_admin          | BOOLEAN          | Global system admin flag               |
| last_login_at     | TIMESTAMP        | Last login timestamp                   |
| created_at        | TIMESTAMP        |                                        |
| updated_at        | TIMESTAMP        |                                        |

### Table: `user_roles` (pivot)

| Field     | Type   | Description              |
|---------- |--------|--------------------------|
| user_id   | UUID   | Foreign key to users      |
| role_id   | UUID   | Foreign key to roles      |

### Table: `api_keys`

| Field     | Type    | Description                        |
|-----------|---------|------------------------------------|
| id        | UUID    | Primary key                        |
| user_id   | UUID    | Linked user                        |
| key       | STRING  | Encrypted token                    |
| name      | STRING  | Developer label for API key        |
| scopes    | JSON    | Permissions for the key            |
| last_used_at | TIMESTAMP | Last access time               |
| created_at| TIMESTAMP |                                  |

---

## 🔐 OAuth & Social Login Support

- OAuth 2.0 token-based API (Laravel Passport / Sanctum)
- Socialite integration for:
  - Google
  - GitHub
  - Twitter
  - LinkedIn
- Token issuance:
  - Login via frontend (OAuth grant)
  - Use of `Bearer Token` in API requests
- JWT-ready (if using Laravel Passport or Firebase Auth)

---

## 🔁 Relationships

- `User` has many `Roles` (many-to-many)
- `User` has many `Permissions` through roles
- `User` has many `api_keys`
- `User` has many `audit_logs`
- `User` can be tied to a `tenant` (multi-tenancy)

---

## 🧩 Related Models

- `roles`
- `permissions`
- `user_roles` (pivot)
- `api_keys`
- `audit_logs`
- `subscriptions`
- `invoices`

---

## 📡 API Endpoints

### 🔐 Auth

| Method | Endpoint                  | Description               |
|--------|---------------------------|---------------------------|
| POST   | /api/register             | Register a new user       |
| POST   | /api/login                | Email/password login      |
| POST   | /api/logout               | Logout                    |
| GET    | /api/user                 | Authenticated user info   |
| POST   | /api/social/{provider}   | Social login              |
| GET    | /api/email/verify/{id}   | Email verification        |
| POST   | /api/password/forgot     | Send reset link           |
| POST   | /api/password/reset      | Reset password            |

### 👤 Profile

| Method | Endpoint               | Description              |
|--------|------------------------|--------------------------|
| GET    | /api/user/profile      | Get profile              |
| PUT    | /api/user/profile      | Update profile           |
| PUT    | /api/user/password     | Change password          |
| GET    | /api/user/api-keys     | List API keys            |
| POST   | /api/user/api-keys     | Create API key           |
| DELETE | /api/user/api-keys/{id}| Revoke API key           |

### 🔐 Admin/User Management

| Method | Endpoint               | Description              |
|--------|------------------------|--------------------------|
| GET    | /api/admin/users       | List all users           |
| GET    | /api/admin/users/{id}  | View user details        |
| PUT    | /api/admin/users/{id}  | Update user              |
| DELETE | /api/admin/users/{id} | Delete or suspend user   |
| POST   | /api/admin/users/{id}/roles | Assign roles         |

---

## 📌 Notes

- Users, Roles, and Permissions are **core modules** and **cannot be disabled**.
- Social logins fallback to standard registration if email exists.
- All sensitive actions require auth token (via `Bearer <token>`).
- 2FA can be enabled via additional table and middleware.

---

## ✅ Future Enhancements

- [ ] Two-Factor Auth via SMS/Email or TOTP
- [ ] Login history and device tracking
- [ ] User invitation system
- [ ] SSO (Single Sign-On) with OAuth2 clients

---

