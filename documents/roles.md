Here’s the **Roles Module** documentation in **Markdown format**, including all necessary API endpoints, database schema, relationships, and pivot tables:

---

# 🛠 Roles Module – Alchemist AI

The **Roles Module** defines user roles within the Alchemist AI SaaS boilerplate, allowing granular control over user access, permissions, and security. It works seamlessly with the **Permissions** module to manage user access levels for different areas of the platform.

---

## 📦 Features

- Role-based access control (RBAC)
- Create and manage user roles
- Assign permissions to roles
- Assign roles to users
- View role-based user statistics
- Multi-tenancy support for isolated roles per tenant
- API key access per role
- Flexible role management for admin and user tiers

---

## 🧾 Database Schema

### Table: `roles`

| Field        | Type           | Description                                       |
|--------------|----------------|---------------------------------------------------|
| id           | UUID / BIGINT  | Primary key                                       |
| name         | STRING         | Role name (e.g., "Admin", "User")                 |
| slug         | STRING (unique)| Unique role identifier                            |
| description  | TEXT           | Description of the role’s purpose                 |
| is_default   | BOOLEAN        | Flag for default roles like Admin (optional)     |
| tenant_id    | UUID (nullable)| For multi-tenancy support, each role can be tenant-specific |
| created_at   | TIMESTAMP      |                                                   |
| updated_at   | TIMESTAMP      |                                                   |

### Table: `role_permissions` (pivot)

| Field       | Type   | Description              |
|-------------|--------|--------------------------|
| role_id     | UUID   | Foreign key to roles      |
| permission_id| UUID  | Foreign key to permissions|

---

## 🔐 Relationships

- `Role` has many `Users` (many-to-many, through `user_roles`)
- `Role` has many `Permissions` through `role_permissions`
- `Role` belongs to a `tenant` (multi-tenancy)
- `Role` has many `audit_logs`

---

## 🧩 Related Models

- `users`
- `permissions`
- `role_permissions` (pivot)
- `audit_logs`
- `modules`

---

## 📡 API Endpoints

### 🔐 Role Management (Admin)

| Method | Endpoint                      | Description                          |
|--------|-------------------------------|--------------------------------------|
| GET    | /api/admin/roles               | List all roles                       |
| GET    | /api/admin/roles/{id}          | View a single role                   |
| POST   | /api/admin/roles               | Create a new role                    |
| PUT    | /api/admin/roles/{id}          | Update a role                        |
| DELETE | /api/admin/roles/{id}          | Delete a role                        |

### 🛠 Role Permissions (Admin)

| Method | Endpoint                          | Description                            |
|--------|-----------------------------------|----------------------------------------|
| POST   | /api/admin/roles/{id}/permissions | Assign permissions to a role           |
| DELETE | /api/admin/roles/{id}/permissions/{permission_id} | Remove permission from a role |

### 👤 User Roles (Admin/User)

| Method | Endpoint                           | Description                           |
|--------|------------------------------------|---------------------------------------|
| POST   | /api/admin/users/{user_id}/roles   | Assign a role to a user               |
| DELETE | /api/admin/users/{user_id}/roles/{role_id} | Remove a role from a user            |

---

## 📌 Notes

- **Roles** control user access levels by associating with permissions.
- **Permissions** are attached to **roles** through the `role_permissions` pivot table.
- Every role can be **tenant-specific** in multi-tenant environments.
- **Admin** has access to all roles and permissions, while other roles have limited access.

---

## ✅ Future Enhancements

- [ ] Multi-role support per user (e.g., Admin + Moderator)
- [ ] Role-based feature flags for advanced access control
- [ ] Dynamic role creation via frontend interface

---
