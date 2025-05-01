Here’s the **Invoice Module** documentation in **Markdown format**, detailing the database schema, API endpoints, and relationships.

---

# 🛠 Invoice Module – Alchemist AI

The **Invoice Module** is responsible for generating invoices for user subscriptions, managing the billing cycle, and tracking the status of payments. It is designed to integrate seamlessly with the **Subscription Module**, providing users with detailed invoice records and payment tracking.

---

## 📦 Features

- Automatically generate invoices for subscription payments.
- Track invoice status (e.g., paid, pending, overdue).
- Integrates with payment gateways (Stripe, PayPal, Razorpay) to retrieve payment information.
- Allows users to download invoices in PDF format.
- Sends invoice-related notifications to users.

---

## 🧾 Database Schema

### Table: `invoices`

| Field           | Type        | Description                                                       |
|-----------------|-------------|-------------------------------------------------------------------|
| id              | UUID        | Primary key (UUID)                                                |
| user_id         | UUID        | Foreign key to `users` table, identifies the user                 |
| subscription_id | UUID        | Foreign key to `subscriptions` table, identifies the associated subscription |
| amount          | Decimal     | Total amount of the invoice                                       |
| status          | Enum        | Status of the invoice (paid, pending, overdue, refunded)         |
| payment_date    | TIMESTAMP   | Date when the payment was made                                    |
| due_date        | TIMESTAMP   | Date by which the payment is due                                  |
| invoice_number  | String      | Unique invoice number                                             |
| created_at      | TIMESTAMP   | Date and time when the invoice was created                        |
| updated_at      | TIMESTAMP   | Date and time when the invoice was last updated                   |

### Table: `invoice_payments`

| Field           | Type        | Description                                                       |
|-----------------|-------------|-------------------------------------------------------------------|
| id              | UUID        | Primary key (UUID)                                                |
| invoice_id      | UUID        | Foreign key to `invoices` table, identifies the related invoice   |
| payment_date    | TIMESTAMP   | Date when the payment was made                                    |
| amount          | Decimal     | Amount paid                                                       |
| payment_method  | Enum        | Payment method used (Stripe, PayPal, Razorpay, etc.)              |
| transaction_id  | String      | Payment transaction ID                                             |
| status          | Enum        | Payment status (successful, failed, pending)                      |
| created_at      | TIMESTAMP   | Date and time when the payment record was created                 |
| updated_at      | TIMESTAMP   | Date and time when the payment record was last updated            |

### Relationships

- An **Invoice** belongs to a **User** and a **Subscription**.
- An **Invoice** can have many **Invoice Payments**.
- An **Invoice Payment** belongs to an **Invoice**.

---

## 📡 API Endpoints

### 👤 Admin Invoice Management

| Method | Endpoint                                | Description                                               |
|--------|-----------------------------------------|-----------------------------------------------------------|
| GET    | /api/admin/invoices                     | List all invoices                                         |
| GET    | /api/admin/invoices/{invoice_id}        | Get details of a specific invoice                          |
| PUT    | /api/admin/invoices/{invoice_id}        | Update the status or other details of an invoice           |
| DELETE | /api/admin/invoices/{invoice_id}        | Delete an invoice (usually not recommended for production) |

### 👤 User Invoice Management

| Method | Endpoint                                | Description                                               |
|--------|-----------------------------------------|-----------------------------------------------------------|
| GET    | /api/user/invoices                      | List all invoices for the authenticated user               |
| GET    | /api/user/invoices/{invoice_id}         | Get details of a specific invoice                          |
| GET    | /api/user/invoices/download/{invoice_id} | Download a PDF version of a specific invoice               |
| POST   | /api/user/invoices/mark-paid            | Mark an invoice as paid (if manually handled)              |

---

## 📌 Notes

- **Invoices** are automatically generated when a user makes a payment for their subscription. They are linked to both the user and the subscription.
- The **Invoice Payments** table tracks individual payments made towards invoices, which can be handled through various payment methods (Stripe, PayPal, Razorpay).
- Users can download their invoices in PDF format. This can be implemented using a package like `dompdf` or `mpdf` in the Laravel backend.
- The **Invoice Status** field helps in tracking whether the invoice has been paid, is pending, or is overdue.
- A user’s subscription will remain active as long as the associated invoice is paid.

---

## ✅ Future Enhancements

- [ ] Add support for **recurring invoices** for users on subscription plans.
- [ ] Provide a **detailed invoice history** and payment tracking for users.
- [ ] Implement **invoice reminders** for users on pending payments.
- [ ] Integrate with other **payment systems** to support more gateways.
- [ ] Allow **multiple payment methods** for invoices (credit card, PayPal, etc.).
- [ ] Provide **partial payments** for invoices, useful for larger payment plans.

---

This module integrates with the **Subscription Module** for payment tracking and invoicing, and with the **Payments Module** for handling actual transactions.
