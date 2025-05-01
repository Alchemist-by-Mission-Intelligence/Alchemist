Here’s the **Permissions Module** documentation in **Markdown format**, including all necessary API endpoints, database schema, relationships, and pivot tables:

---

# 🛠 Permissions Module – Alchemist AI

The **Permissions Module** defines granular permissions within the Alchemist AI SaaS boilerplate. It is tightly integrated with the **Roles** module, enabling fine-grained access control for users across different parts of the platform. Permissions can be assigned to roles, and users are granted access based on their assigned roles and the permissions associated with those roles.

---

## 📦 Features

- Define and manage permissions within the application
- Assign permissions to roles
- Assign multiple permissions to a role (many-to-many relationship)
- Flexibility to create custom permissions for any feature of the platform
- Multi-tenancy support for isolated permissions per tenant
- API key access control based on permissions
- Role-based access to different modules

---

## 🧾 Database Schema

### Table: `permissions`

| Field        | Type           | Description                                        |
|--------------|----------------|----------------------------------------------------|
| id           | UUID / BIGINT  | Primary key                                        |
| name         | STRING         | Permission name (e.g., "view_dashboard")           |
| slug         | STRING (unique)| Unique permission identifier                       |
| description  | TEXT           | Description of the permission's purpose            |
| tenant_id    | UUID (nullable)| For multi-tenancy support, each permission can be tenant-specific |
| created_at   | TIMESTAMP      |                                                    |
| updated_at   | TIMESTAMP      |                                                    |

### Table: `role_permissions` (pivot)

| Field       | Type   | Description              |
|-------------|--------|--------------------------|
| role_id     | UUID   | Foreign key to `roles`    |
| permission_id | UUID | Foreign key to `permissions`|

---

## 🔐 Relationships

- `Permission` has many `Roles` through `role_permissions`
- `Role` has many `Permissions` through `role_permissions`
- `Permission` belongs to `tenant` (multi-tenancy)
- `Permission` has many `audit_logs`

---

## 🧩 Related Models

- `roles`
- `role_permissions` (pivot)
- `users` (through `user_roles`)
- `audit_logs`
- `modules`

---

## 📡 API Endpoints

### 🔐 Permission Management (Admin)

| Method | Endpoint                           | Description                              |
|--------|------------------------------------|------------------------------------------|
| GET    | /api/admin/permissions             | List all permissions                     |
| GET    | /api/admin/permissions/{id}        | View a single permission                 |
| POST   | /api/admin/permissions             | Create a new permission                  |
| PUT    | /api/admin/permissions/{id}        | Update a permission                      |
| DELETE | /api/admin/permissions/{id}        | Delete a permission                      |

### 🛠 Role Permissions (Admin)

| Method | Endpoint                           | Description                               |
|--------|------------------------------------|-------------------------------------------|
| POST   | /api/admin/permissions/{id}/roles  | Assign permission to a role               |
| DELETE | /api/admin/permissions/{id}/roles  | Remove permission from a role             |

### 👤 User Permissions (Admin/User)

| Method | Endpoint                           | Description                               |
|--------|------------------------------------|-------------------------------------------|
| POST   | /api/admin/users/{user_id}/permissions | Assign permission to a user              |
| DELETE | /api/admin/users/{user_id}/permissions/{permission_id} | Remove permission from a user           |

---

## 📌 Notes

- **Permissions** define what actions a user or role can perform on the platform (e.g., access to specific resources, view or modify data).
- Permissions are assigned to roles through the `role_permissions` pivot table.
- Users are granted permissions based on their assigned roles, which are stored in the `user_roles` pivot table.
- Each permission can be isolated per tenant in a multi-tenant architecture.

---

## ✅ Future Enhancements

- [ ] Granular permission levels (e.g., read-only, write, delete)
- [ ] UI support for dynamically creating and managing permissions
- [ ] Advanced permissions for third-party integrations

---

