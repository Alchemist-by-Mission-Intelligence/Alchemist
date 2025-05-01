Here’s the **Plan Module** documentation in **Markdown format**, detailing the database schema, API endpoints, and relationships.

---

# 🛠 Plan Module – Alchemist AI

The **Plan Module** manages the subscription plans available to users. These plans define what features and limits are available to users on different levels. The module supports different pricing tiers, billing cycles (monthly/yearly), and customizable features for each plan.

---

## 📦 Features

- Defines subscription plans with customizable features.
- Supports multiple pricing models (monthly, yearly, etc.).
- Integration with the **Subscription** module for billing and payments.
- Allows the creation of trial plans, discount plans, and promotional offers.
- Supports multi-tenancy for custom pricing and plans per tenant.
- API integration with the **User** and **Subscriptions** modules.

---

## 🧾 Database Schema

### Table: `plans`

| Field          | Type        | Description                                                   |
|----------------|-------------|---------------------------------------------------------------|
| id             | UUID        | Primary key (UUID)                                            |
| name           | String      | The name of the plan                                          |
| description    | Text        | A detailed description of the plan                            |
| price          | Decimal     | The price of the plan in the base currency                     |
| billing_cycle  | String      | Defines the billing cycle ("monthly", "yearly")               |
| trial_period   | Integer     | Number of days for the trial period (nullable)                |
| active         | Boolean     | Indicates if the plan is active                               |
| created_at     | TIMESTAMP   | Date and time the plan was created                            |
| updated_at     | TIMESTAMP   | Date and time the plan was last updated                       |

### Table: `plan_features`

| Field          | Type        | Description                                                   |
|----------------|-------------|---------------------------------------------------------------|
| id             | UUID        | Primary key (UUID)                                            |
| plan_id        | UUID        | Foreign key to `plans` table                                   |
| feature_name   | String      | Name of the feature included in the plan                      |
| description    | Text        | A brief description of the feature                             |
| created_at     | TIMESTAMP   | Date and time the feature was added to the plan                |
| updated_at     | TIMESTAMP   | Date and time the feature was last updated                     |

---

## 🔐 Relationships

- **Plans** have many **Plan Features** – A single plan can have multiple features associated with it.
- **Plans** are associated with **Users** through the **Subscriptions** module (A user subscribes to a plan).
- The **Plan Features** table defines the specific features available for each plan.
- Multi-tenancy support is provided, allowing each tenant to have their own set of plans.

---

## 📡 API Endpoints

### 👤 Plan Management (Admin)

| Method | Endpoint                  | Description                                         |
|--------|---------------------------|-----------------------------------------------------|
| GET    | /api/admin/plans           | List all available plans                           |
| POST   | /api/admin/plans           | Create a new plan                                  |
| PUT    | /api/admin/plans/{id}      | Update an existing plan                            |
| DELETE | /api/admin/plans/{id}      | Delete a plan                                      |

### 👤 Plan Features Management (Admin)

| Method | Endpoint                       | Description                                         |
|--------|--------------------------------|-----------------------------------------------------|
| GET    | /api/admin/plans/{id}/features | List features of a specific plan                    |
| POST   | /api/admin/plans/{id}/features | Add a feature to a specific plan                    |
| DELETE | /api/admin/plans/{id}/features/{feature_id} | Remove a feature from a plan        |

---

## 📌 Notes

- The **Plan Module** is essential for defining different subscription levels available to users. Each plan can have a set of features, such as access to specific tools or API usage limits.
- **Billing cycle** defines how frequently a user will be charged, and this can be either **monthly** or **yearly**.
- The **Trial period** feature allows users to try a plan before committing to a full subscription.
- Multi-tenancy support allows different organizations (or tenants) to have their own set of plans, ideal for SaaS platforms serving multiple clients.

---

## ✅ Future Enhancements

- [ ] Integration with dynamic pricing models (based on usage, user number, etc.)
- [ ] Support for discount codes and promo campaigns
- [ ] Dynamic updates to features based on user feedback
- [ ] Notifications for plan upgrades or downgrades

---

Would you like to proceed with the **Plan Features** next, or need any adjustments here?