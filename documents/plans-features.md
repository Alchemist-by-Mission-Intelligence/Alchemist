Here’s the **Plan Features Module** documentation in **Markdown format**, detailing the database schema, API endpoints, and relationships.

---

# 🛠 Plan Features Module – Alchemist AI

The **Plan Features Module** defines the features that are available to users depending on the subscription plan they have selected. These features can range from access to AI tools, API limits, premium features, and more. Plan features can be customized and associated with different plans to create flexible pricing models.

---

## 📦 Features

- Defines the specific features available with each plan.
- Allows admins to add/remove features for each plan.
- Supports dynamic feature activation/deactivation based on the plan the user subscribes to.
- Allows feature descriptions and categorization for easy management.
- Enables flexibility for custom features or promotional additions to plans.

---

## 🧾 Database Schema

### Table: `plan_features`

| Field          | Type        | Description                                                   |
|----------------|-------------|---------------------------------------------------------------|
| id             | UUID        | Primary key (UUID)                                            |
| plan_id        | UUID        | Foreign key to `plans` table (indicating which plan the feature belongs to) |
| feature_name   | String      | The name of the feature                                       |
| description    | Text        | A detailed description of the feature                         |
| created_at     | TIMESTAMP   | Date and time the feature was created                          |
| updated_at     | TIMESTAMP   | Date and time the feature was last updated                     |

### Relationships

- **Plan Features** are associated with a **Plan** through the `plan_id` field.
- One **Plan** can have multiple **Plan Features**, but each **Plan Feature** belongs to one specific plan.

---

## 📡 API Endpoints

### 👤 Plan Features Management (Admin)

| Method | Endpoint                           | Description                                            |
|--------|------------------------------------|--------------------------------------------------------|
| GET    | /api/admin/plans/{plan_id}/features | List all features of a specific plan                   |
| POST   | /api/admin/plans/{plan_id}/features | Add a new feature to the specified plan                |
| PUT    | /api/admin/plans/{plan_id}/features/{feature_id} | Update a specific feature of the plan             |
| DELETE | /api/admin/plans/{plan_id}/features/{feature_id} | Remove a feature from the specified plan             |

### 👤 Feature Access for Users (Public)

| Method | Endpoint                       | Description                                            |
|--------|--------------------------------|--------------------------------------------------------|
| GET    | /api/user/features             | Get the features available for the current user’s plan |

---

## 📌 Notes

- **Plan Features** define the specific functionalities or services available to a user based on their subscription plan. This can include tool access, API usage limits, storage quotas, and other exclusive features.
- Features are linked directly to a **Plan**, and when a user subscribes to a particular plan, they automatically gain access to its associated features.
- Administrators can manage features for any plan, enabling flexibility in pricing tiers and feature sets.
- This module plays a crucial role in supporting scalable SaaS platforms with custom features for different subscription levels.

---

## ✅ Future Enhancements

- [ ] Customizable feature categories (e.g., Basic, Premium, Exclusive)
- [ ] Ability to offer per-user/per-tenant customizable feature sets
- [ ] Add-ons for additional features outside of base plans
- [ ] Discounts or bonus features for special promotions or loyalty programs

---

Would you like to proceed with the **Subscription Module** next, or need any adjustments here?