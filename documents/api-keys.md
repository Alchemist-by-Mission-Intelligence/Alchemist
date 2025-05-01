Here's the **API Keys Module** documentation in **Markdown format**, detailing the database schema, API endpoints, and relationships.

---

# 🗝 API Keys Module – Alchemist AI

The **API Keys Module** is used to manage API key creation and access for users. This allows users to interact programmatically with the platform through secure, controlled access. API keys can be used for integrating external services, automation, or accessing platform data.

---

## 📦 Features

- Generate API keys for users.
- Secure API key management.
- Define scopes/permissions for each API key (e.g., read, write).
- Expire or revoke API keys as needed.
- Provide rate-limiting to control API usage.
- Support OAuth and JWT for API key security.
- Store API key metadata (creation time, expiration, etc.).

---

## 🧾 Database Schema

### Table: `api_keys`

| Field            | Type        | Description                                                        |
|------------------|-------------|--------------------------------------------------------------------|
| id               | UUID        | Primary key (UUID)                                                 |
| user_id          | UUID        | Foreign key referencing `users.id` (the user who owns the key)    |
| key              | String      | The generated API key (secure, encrypted)                          |
| name             | String      | Name for the API key (e.g., "User integration")                    |
| description      | String      | Description of the purpose of the API key                          |
| scopes           | JSON        | Permissions associated with the API key (e.g., `read`, `write`)   |
| created_at       | TIMESTAMP   | Timestamp when the API key was created                             |
| updated_at       | TIMESTAMP   | Timestamp when the API key was last updated                        |
| expires_at       | TIMESTAMP   | Expiry date for the API key (nullable)                             |
| revoked_at       | TIMESTAMP   | Timestamp when the API key was revoked (nullable)                  |

### Relationships

- **API Keys** belong to a **User**, referenced via the `user_id` foreign key.
- The **scopes** field contains a JSON object defining the permissions assigned to the API key (e.g., `read`, `write`).

---

## 📡 API Endpoints

### 👤 Admin API Key Management

| Method | Endpoint                                | Description                                                      |
|--------|-----------------------------------------|------------------------------------------------------------------|
| GET    | /api/admin/api-keys                     | Retrieve all API keys                                            |
| POST   | /api/admin/api-keys                     | Create a new API key                                             |
| GET    | /api/admin/api-keys/{id}                | Retrieve a specific API key by ID                                |
| PUT    | /api/admin/api-keys/{id}                | Update a specific API key                                        |
| DELETE | /api/admin/api-keys/{id}                | Revoke/Delete a specific API key                                 |

### 👤 User API Key Management

| Method | Endpoint                                | Description                                                      |
|--------|-----------------------------------------|------------------------------------------------------------------|
| GET    | /api/user/api-keys                      | Retrieve all API keys for the user                               |
| POST   | /api/user/api-keys                      | Create a new API key for the user                                |
| GET    | /api/user/api-keys/{id}                 | Retrieve a specific API key by ID                                |
| DELETE | /api/user/api-keys/{id}                 | Revoke/Delete a specific API key                                 |

---

## 🔑 Notes

- **Scoping**: Each API key can have multiple scopes defined in a JSON object. This allows granular control over what actions the API key can perform (e.g., `read` and `write` permissions).
- **Expiry**: API keys can be set with an expiration date (`expires_at`). Once the key expires, the user will need to generate a new key.
- **Revocation**: API keys can be revoked by the user or admin. A revocation timestamp (`revoked_at`) is stored for audit purposes.
- **Security**: The `key` field is encrypted, ensuring that API keys are securely stored and can only be used through proper authentication (OAuth or JWT).
- **Usage**: API keys can be used to authenticate requests to the platform, allowing users to interact with the platform programmatically. Rate limits may be applied based on user tier or API key scope.

---

## ✅ Future Enhancements

- [ ] **Rate Limiting**: Implement per-key rate limiting to prevent abuse.
- [ ] **API Key Analytics**: Provide usage statistics for each API key (e.g., number of requests made, last used time).
- [ ] **API Key Scopes Customization**: Allow admins to define custom scopes for more granular permissions.
- [ ] **Integration with OAuth Providers**: Enable OAuth-based authentication for external apps.

---

This module is related to the **Users Module**, as API keys are associated with individual users. It also interacts with the **Roles and Permissions Module** for defining access levels and scoping the permissions granted to each API key.

---
