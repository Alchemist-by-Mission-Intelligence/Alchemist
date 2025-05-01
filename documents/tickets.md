Here is the **Support Tickets Module** documentation in **Markdown format**, including the database schema, API endpoints, and relationships.

---

# 📝 Support Tickets Module – Alchemist AI

The **Support Tickets Module** allows users to submit and manage support requests. It provides a structured way to handle user issues, inquiries, and feature requests, enabling efficient customer support. This module is ideal for tracking tickets, their resolution status, and communication between users and the support team.

---

## 📦 Features

- **User Ticket Submission**: Allows users to create support tickets for issues or inquiries.
- **Admin Ticket Management**: Admins can view, assign, and manage tickets.
- **Ticket Categories**: Organize tickets into categories (e.g., Billing, Technical Support, Feature Requests).
- **Ticket Status**: Track the status of tickets (e.g., Open, Pending, Resolved, Closed).
- **Email Notifications**: Notify users and admins when a ticket is updated or resolved.
- **Ticket Priority**: Set priorities (Low, Medium, High) for tickets to indicate urgency.
- **Search and Filter**: Search and filter tickets by user, status, priority, or category.
- **Ticket Notes**: Add internal notes to tickets for communication between support agents.
- **Attachment Support**: Allow users to attach files or screenshots to tickets.

---

## 🧾 Database Schema

### Table: `support_tickets`

| Field             | Type        | Description                                                            |
|-------------------|-------------|------------------------------------------------------------------------|
| id                | UUID        | Primary key (UUID)                                                     |
| user_id           | UUID        | Foreign key referencing `users.id` (user who created the ticket)       |
| category_id       | UUID        | Foreign key referencing `ticket_categories.id` (ticket category)      |
| title             | String      | Title of the support ticket                                            |
| description       | Text        | Detailed description of the issue or inquiry                           |
| status            | String      | Ticket status (e.g., "Open", "Pending", "Resolved", "Closed")          |
| priority          | String      | Ticket priority (e.g., "Low", "Medium", "High")                        |
| created_at        | TIMESTAMP   | Timestamp when the ticket was created                                  |
| updated_at        | TIMESTAMP   | Timestamp when the ticket was last updated                              |
| resolved_at       | TIMESTAMP   | Timestamp when the ticket was resolved (if applicable)                  |

### Table: `ticket_categories`

| Field             | Type        | Description                                                            |
|-------------------|-------------|------------------------------------------------------------------------|
| id                | UUID        | Primary key (UUID)                                                     |
| name              | String      | Name of the ticket category (e.g., Billing, Technical Support)         |
| description       | Text        | Description of the category (optional)                                 |
| created_at        | TIMESTAMP   | Timestamp when the category was created                                |
| updated_at        | TIMESTAMP   | Timestamp when the category was last updated                           |

### Table: `ticket_notes`

| Field             | Type        | Description                                                            |
|-------------------|-------------|------------------------------------------------------------------------|
| id                | UUID        | Primary key (UUID)                                                     |
| ticket_id         | UUID        | Foreign key referencing `support_tickets.id` (the related ticket)     |
| user_id           | UUID        | Foreign key referencing `users.id` (support agent or user adding the note) |
| note              | Text        | Note content                                                          |
| created_at        | TIMESTAMP   | Timestamp when the note was added                                      |

### Relationships

- **Support Tickets** belong to a **User** (`user_id`).
- **Support Tickets** belong to a **Ticket Category** (`category_id`).
- **Ticket Notes** belong to a **Support Ticket** (`ticket_id`).
- **Ticket Notes** belong to a **User** (`user_id`), who either adds a note as a support agent or the user themselves.

---

## 📡 API Endpoints

### User API Endpoints

| Method | Endpoint                     | Description                                                   |
|--------|------------------------------|---------------------------------------------------------------|
| POST   | /api/user/tickets             | Submit a new support ticket                                    |
| GET    | /api/user/tickets             | Retrieve all support tickets for the authenticated user       |
| GET    | /api/user/tickets/{id}        | Retrieve a specific ticket by ID for the authenticated user    |
| PUT    | /api/user/tickets/{id}        | Update an existing ticket (e.g., add more details or close it) |
| GET    | /api/user/tickets/search      | Search and filter tickets (by status, priority, category)     |

### Admin API Endpoints

| Method | Endpoint                     | Description                                                   |
|--------|------------------------------|---------------------------------------------------------------|
| GET    | /api/admin/tickets            | Retrieve all support tickets                                   |
| GET    | /api/admin/tickets/{id}       | Retrieve a specific support ticket by ID                       |
| PUT    | /api/admin/tickets/{id}       | Update the ticket status, priority, or assign to an agent      |
| DELETE | /api/admin/tickets/{id}       | Delete a support ticket (soft delete or permanently)           |
| GET    | /api/admin/tickets/search     | Search and filter tickets (by status, user, priority, category)|
| POST   | /api/admin/ticket-categories  | Create a new ticket category                                   |
| GET    | /api/admin/ticket-categories  | List all available ticket categories                           |

---

## 🔑 Notes

- **Ticket Submission**: Users can submit tickets with a title and detailed description of the issue. They can also select a category and priority level. 
- **Ticket Categories**: Categories help organize tickets, ensuring they are routed to the right department or team. Admins can manage the categories.
- **Ticket Status**: The status of a ticket is updated by admins or support agents as the ticket moves through different stages (e.g., Open, Resolved).
- **Notes**: Support agents can add internal notes to tickets for internal communication. These notes are visible to other support agents but not to the end users.
- **Search and Filter**: Both admins and users can search and filter tickets based on various criteria like status, priority, category, and user.
- **Email Notifications**: Email notifications are sent to users and admins when a ticket is updated, resolved, or requires attention.

---

## ✅ Future Enhancements

- [ ] **Ticket Attachments**: Allow users to upload files/screenshots with their tickets.
- [ ] **Escalation Workflow**: Implement an escalation system for tickets that require higher-level attention.
- [ ] **SLA Integration**: Set Service Level Agreements (SLAs) for tickets based on priority.
- [ ] **Automated Responses**: Use AI-powered automated responses for common inquiries.
- [ ] **Ticket Dashboard**: A UI for users and support agents to view, manage, and track ticket progress.
- [ ] **Customizable Categories**: Allow admins to create custom ticket categories and set rules for routing tickets to the appropriate support team.
  
---

This module is directly related to the **Users Module**, as tickets are submitted by users. It integrates with the **Roles and Permissions Module** to control who can view, manage, and resolve tickets. The **Audit Logs Module** could also be connected to track changes in ticket statuses and actions taken by admins.

---

Would you like to proceed with the **Files Module**, or do you need any adjustments to this module?