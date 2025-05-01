Here’s the **Subscription Module** documentation in **Markdown format**, detailing the database schema, API endpoints, and relationships.

---

# 🛠 Subscription Module – Alchemist AI

The **Subscription Module** handles the process of managing user subscriptions, including plans, billing cycles, payments, and renewal management. It is integral for supporting SaaS businesses with recurring revenue models, allowing users to subscribe to different plans and manage their payment preferences.

---

## 📦 Features

- Manages user subscriptions to various pricing plans.
- Supports different billing cycles (e.g., monthly, yearly).
- Tracks active, canceled, and expired subscriptions.
- Integrates with payment gateways (Stripe, PayPal, Razorpay) for payments.
- Allows users to update their subscription, upgrade, or downgrade.
- Sends email notifications for upcoming renewals, failed payments, etc.

---

## 🧾 Database Schema

### Table: `subscriptions`

| Field           | Type        | Description                                                       |
|-----------------|-------------|-------------------------------------------------------------------|
| id              | UUID        | Primary key (UUID)                                                |
| user_id         | UUID        | Foreign key to `users` table, identifies the user                 |
| plan_id         | UUID        | Foreign key to `plans` table, identifies the plan chosen by the user |
| start_date      | TIMESTAMP   | The date when the subscription started                            |
| end_date        | TIMESTAMP   | The date when the subscription will end (for recurring plans)     |
| status          | Enum        | Status of the subscription (active, canceled, expired)            |
| trial_end_date  | TIMESTAMP   | Date when the trial period ends (if applicable)                   |
| next_billing_date | TIMESTAMP | The date for the next billing cycle                              |
| created_at      | TIMESTAMP   | Date and time when the subscription was created                   |
| updated_at      | TIMESTAMP   | Date and time when the subscription was last updated              |

### Table: `subscription_payments`

| Field           | Type        | Description                                                       |
|-----------------|-------------|-------------------------------------------------------------------|
| id              | UUID        | Primary key (UUID)                                                |
| subscription_id | UUID        | Foreign key to `subscriptions` table, identifies the subscription |
| payment_date    | TIMESTAMP   | Date when the payment was made                                    |
| amount          | Decimal     | The amount paid                                                   |
| payment_status  | Enum        | Payment status (completed, failed, pending)                       |
| transaction_id  | String      | Transaction reference ID from payment provider                    |
| created_at      | TIMESTAMP   | Date and time when the payment record was created                 |

### Relationships

- A **Subscription** belongs to a **User** and a **Plan**.
- A **Subscription** can have many **Payments** recorded in `subscription_payments`.

---

## 📡 API Endpoints

### 👤 Subscription Management (Admin)

| Method | Endpoint                                | Description                                              |
|--------|-----------------------------------------|----------------------------------------------------------|
| GET    | /api/admin/subscriptions                | List all subscriptions                                  |
| GET    | /api/admin/subscriptions/{subscription_id} | Get details of a specific subscription                    |
| PUT    | /api/admin/subscriptions/{subscription_id} | Update subscription details (status, next billing, etc.) |
| DELETE | /api/admin/subscriptions/{subscription_id} | Cancel a subscription                                    |

### 👤 User Subscription Management (User)

| Method | Endpoint                                | Description                                              |
|--------|-----------------------------------------|----------------------------------------------------------|
| GET    | /api/user/subscription                  | Get the current subscription for the authenticated user  |
| POST   | /api/user/subscription/upgrade          | Upgrade the user's subscription plan                     |
| POST   | /api/user/subscription/cancel           | Cancel the user's subscription                           |
| GET    | /api/user/subscription/history          | Get the payment and subscription history for the user    |

### 👤 Payment Management

| Method | Endpoint                                | Description                                              |
|--------|-----------------------------------------|----------------------------------------------------------|
| POST   | /api/user/subscription/payment          | Make a payment for a subscription                         |
| GET    | /api/user/subscription/payment/status   | Get the status of a payment                               |

---

## 📌 Notes

- **Subscriptions** are tied to **Plans** and represent the user's current active billing cycle.
- A user can only have one active subscription at a time, but they may switch plans or pause/cancel their subscription.
- Payments are tracked separately, and can be linked to subscriptions to handle billing history.
- The system allows flexibility in managing subscription renewals, upgrades, and cancellations.
- Subscription status can be updated automatically when payments are successful or failed.

---

## ✅ Future Enhancements

- [ ] Add **coupon codes** for discounts or promotional pricing.
- [ ] Integrate with **webhooks** from payment providers for automatic payment notifications.
- [ ] Support **multi-currency** payments for international customers.
- [ ] Provide **subscription history** and detailed billing history to users.
- [ ] Implement **grace periods** for failed payments before cancellation.
- [ ] Support **custom billing cycles** (e.g., quarterly, biennial).

---

This module integrates deeply with the **Payments** module, **Plans** module, and user **Authentication**.

Would you like to proceed with the **Invoice Module** next, or need any adjustments here?