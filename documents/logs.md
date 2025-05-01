Here is the **Audit Logs Module** documentation in **Markdown format**, including the database schema, API endpoints, and relationships.

---

# 📝 Audit Logs Module – Alchemist AI

The **Audit Logs Module** helps track and log important actions taken by users within the application. This includes actions like login attempts, data changes, administrative actions, and more. Audit logs are crucial for security, compliance, and monitoring user activity.

---

## 📦 Features

- Track user login, logout, and session management.
- Log data changes and updates (e.g., object creation, modifications, deletions).
- Record admin-level actions (e.g., user role assignments, billing changes).
- Provide audit trail to ensure compliance and data integrity.
- Searchable logs with filtering options (e.g., by user, action, date).
- Provide detailed logs for external integrations and system operations.

---

## 🧾 Database Schema

### Table: `audit_logs`

| Field              | Type        | Description                                                            |
|--------------------|-------------|------------------------------------------------------------------------|
| id                 | UUID        | Primary key (UUID)                                                     |
| user_id            | UUID        | Foreign key referencing `users.id` (user responsible for the action)  |
| action             | String      | Description of the action performed (e.g., "created", "updated")       |
| model              | String      | The model name on which the action occurred (e.g., "User", "Post")    |
| model_id           | UUID        | The ID of the model instance affected by the action                   |
| before_data        | JSON        | Data of the model before the action (for updates)                      |
| after_data         | JSON        | Data of the model after the action (for updates)                       |
| ip_address         | String      | IP address of the user performing the action                           |
| user_agent         | String      | User agent (browser/device) of the user performing the action         |
| created_at         | TIMESTAMP   | Timestamp when the action was performed                                |
| updated_at         | TIMESTAMP   | Timestamp when the log entry was last updated                          |

### Relationships

- **Audit Logs** belong to a **User**, referenced via the `user_id` foreign key.
- The **model** refers to the name of the model (e.g., `User`, `Post`, etc.) on which the action was performed.
- **before_data** and **after_data** are used to store the state of the model before and after an action (mainly for updates).
- **ip_address** and **user_agent** store information about the client making the request for further tracking and security.

---

## 📡 API Endpoints

### Admin API Endpoints

| Method | Endpoint                      | Description                                                   |
|--------|-------------------------------|---------------------------------------------------------------|
| GET    | /api/admin/audit-logs          | Retrieve all audit logs                                        |
| GET    | /api/admin/audit-logs/{id}     | Retrieve a specific audit log by ID                            |
| GET    | /api/admin/audit-logs/search   | Search and filter audit logs based on parameters (user, date) |

### User API Endpoints

| Method | Endpoint                      | Description                                                   |
|--------|-------------------------------|---------------------------------------------------------------|
| GET    | /api/user/audit-logs           | Retrieve all audit logs for the authenticated user            |
| GET    | /api/user/audit-logs/{id}      | Retrieve a specific audit log by ID for the authenticated user |
| GET    | /api/user/audit-logs/search    | Search and filter audit logs for the authenticated user       |

---

## 🔑 Notes

- **Tracking Actions**: This module is designed to track and record important actions performed by users. It can log actions such as creating, updating, and deleting records. It can also log admin activities like role assignments or subscription updates.
  
- **Search & Filters**: The `search` endpoint allows filtering logs by parameters such as date, user, action type, or model name. This is especially useful for debugging or compliance purposes.
  
- **Data Integrity**: By logging both the "before" and "after" data, this module ensures that any changes to data can be audited. This is particularly useful for debugging and verifying that changes were made correctly.
  
- **Security**: Logs contain metadata such as IP address and user agent, providing additional context and security information for each action. This can help in tracking suspicious activity.
  
- **Compliance**: The audit logs module helps maintain a compliant system by ensuring that all user actions are traceable, which is critical for certain industries like healthcare, finance, and education.

---

## ✅ Future Enhancements

- [ ] **User Activity Dashboard**: Create a UI for admins to view activity logs for individual users.
- [ ] **Log Retention Policy**: Implement log expiration and automatic deletion for compliance with data retention policies.
- [ ] **Automated Alerts**: Send notifications to admins for suspicious activities (e.g., repeated failed login attempts).
- [ ] **Export Logs**: Allow the export of audit logs in CSV or JSON format for reporting and compliance purposes.
- [ ] **Advanced Search**: Implement more granular filters, such as time ranges, action types, and model relationships.

---

This module is related to the **Users Module**, as it records actions taken by users. It also interacts with the **Roles and Permissions Module** to ensure that actions by admins or users with specific roles are properly logged.

---

Would you like to proceed with the **Support Tickets Module**, or do you need any adjustments to this module?