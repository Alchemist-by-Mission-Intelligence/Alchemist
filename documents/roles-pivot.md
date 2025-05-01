Here’s the **User-Roles Pivot Module** documentation in **Markdown format**, detailing the relationships between users and roles, along with the necessary API endpoints and database schema:

---

# 🛠 User-Roles Pivot Module – Alchemist AI

The **User-Roles Pivot Module** is used to manage the many-to-many relationship between users and roles. This pivot table establishes which roles are assigned to each user, granting them specific permissions and access levels based on their role(s).

---

## 📦 Features

- Many-to-many relationship between users and roles
- Support for assigning multiple roles to a user
- Role-based access control (RBAC) based on user-role associations
- Enables users to inherit the permissions of their assigned roles
- Multi-tenancy support for tenant-specific user-role relationships
- API integration with the **Roles** and **Permissions** modules

---

## 🧾 Database Schema

### Table: `user_roles` (pivot)

| Field     | Type    | Description                             |
|-----------|---------|-----------------------------------------|
| user_id   | UUID    | Foreign key to `users` table            |
| role_id   | UUID    | Foreign key to `roles` table            |
| tenant_id | UUID    | Foreign key to `tenants` table (optional, for multi-tenancy) |
| created_at| TIMESTAMP | Date and time the role was assigned  |
| updated_at| TIMESTAMP | Date and time the role was last updated |

---

## 🔐 Relationships

- A `User` has many `Roles` through the `user_roles` pivot table.
- A `Role` has many `Users` through the `user_roles` pivot table.
- Each `User` can have multiple roles, and each `Role` can be assigned to multiple users.
- The pivot table supports multi-tenancy, ensuring roles can be assigned on a per-tenant basis.

---

## 📡 API Endpoints

### 👤 User-Roles Management (Admin)

| Method | Endpoint                              | Description                                    |
|--------|---------------------------------------|------------------------------------------------|
| GET    | /api/admin/users/{id}/roles           | List all roles assigned to a user              |
| POST   | /api/admin/users/{id}/roles           | Assign one or more roles to a user             |
| DELETE | /api/admin/users/{id}/roles/{role_id} | Remove a role from a user                      |

### 🛠 Role-Users Management (Admin)

| Method | Endpoint                              | Description                                    |
|--------|---------------------------------------|------------------------------------------------|
| GET    | /api/admin/roles/{id}/users           | List all users with a specific role            |
| POST   | /api/admin/roles/{id}/users           | Assign a role to multiple users at once        |
| DELETE | /api/admin/roles/{id}/users/{user_id} | Remove a role from a user                      |

---

## 📌 Notes

- The **User-Roles Pivot** allows for flexible assignment of multiple roles to a single user. This enables users to have different levels of access across various parts of the platform based on the roles they are assigned.
- It plays a crucial role in **Role-Based Access Control (RBAC)**, where the permissions granted to a user are derived from the roles they hold.
- In multi-tenancy environments, this pivot table can also track roles for each tenant, ensuring that users have roles specific to their tenant context.

---

## ✅ Future Enhancements

- [ ] Support for role hierarchies (e.g., admin, moderator, user)
- [ ] Dynamic role management from the UI (for admins)
- [ ] Ability to assign permissions directly to users without roles

---

Would you like to proceed with the **Plan Module** documentation next?