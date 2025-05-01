### **Detailed Documentation for Notifications System**

The **Notifications System** is a robust system that allows the platform to send alerts and messages to users regarding various events and actions. It supports multiple channels for notification delivery, including in-app notifications, email, and push notifications. The system allows for both system-triggered notifications and user-triggered notifications, enabling personalized communication with the users.

---

### **1. Features Overview**

- **Notification Channels**: In-app notifications, emails, and push notifications.
- **Notification Types**: System alerts, user-generated notifications (e.g., messages, likes, comments).
- **Notification Preferences**: Users can manage their preferences for receiving notifications through different channels.
- **Real-Time Updates**: Notifications can be sent in real-time when an event or action occurs.
- **Notification Scheduling**: Notifications can be scheduled for future delivery.
- **Admin Control**: Admins can send broadcast notifications to all users.
- **Notification Templates**: Admins can create reusable notification templates for common events.

---

### **2. Database Schema**

#### **Notifications Table**
This table stores information about all notifications sent to users.

| Field             | Type        | Description                                                   |
|-------------------|-------------|---------------------------------------------------------------|
| id                | UUID        | Primary key                                                   |
| user_id           | UUID        | Foreign key referencing `users.id`                            |
| notification_type | String      | Type of notification (e.g., "message", "system", "alert")     |
| message           | Text        | Content of the notification                                   |
| status            | Enum        | Status of the notification (`sent`, `pending`, `read`)        |
| delivery_channel  | Enum        | Channel used for delivery (`email`, `in-app`, `push`)        |
| created_at        | TIMESTAMP   | Timestamp when the notification was created                   |
| updated_at        | TIMESTAMP   | Timestamp when the notification was last updated              |

#### **Notification Preferences Table**
This table stores user-specific notification preferences, including which channels and types of notifications the user wants to receive.

| Field             | Type        | Description                                                   |
|-------------------|-------------|---------------------------------------------------------------|
| id                | UUID        | Primary key                                                   |
| user_id           | UUID        | Foreign key referencing `users.id`                            |
| notification_type | String      | Type of notification the user can subscribe to (e.g., "message", "system") |
| email_enabled     | Boolean     | Whether email notifications are enabled for this type        |
| in_app_enabled    | Boolean     | Whether in-app notifications are enabled for this type       |
| push_enabled      | Boolean     | Whether push notifications are enabled for this type         |
| created_at        | TIMESTAMP   | Timestamp when the preference was created                     |
| updated_at        | TIMESTAMP   | Timestamp when the preference was last updated                |

#### **Notification Templates Table**
This table stores reusable templates for notifications that can be triggered by the system or the user.

| Field             | Type        | Description                                                   |
|-------------------|-------------|---------------------------------------------------------------|
| id                | UUID        | Primary key                                                   |
| template_name     | String      | Name of the template (e.g., "Welcome Email", "Password Reset")|
| subject           | String      | Subject line of the notification                              |
| body              | Text        | Body content of the notification template                     |
| created_at        | TIMESTAMP   | Timestamp when the template was created                       |
| updated_at        | TIMESTAMP   | Timestamp when the template was last updated                  |

#### **Push Notification Queue Table**
This table stores queued notifications that are scheduled for push notification delivery.

| Field             | Type        | Description                                                   |
|-------------------|-------------|---------------------------------------------------------------|
| id                | UUID        | Primary key                                                   |
| notification_id   | UUID        | Foreign key referencing `notifications.id`                    |
| scheduled_time    | TIMESTAMP   | Timestamp when the notification is scheduled to be sent       |
| delivery_status   | Enum        | Status of the push notification (`pending`, `sent`, `failed`) |
| created_at        | TIMESTAMP   | Timestamp when the push notification was queued               |
| updated_at        | TIMESTAMP   | Timestamp when the status was last updated                    |

---

### **3. Relationships & Pivot Tables**

- **Users & Notifications**  
  One-to-many relationship: Each user can have many notifications, but each notification belongs to one user.

- **Users & Notification Preferences**  
  One-to-many relationship: Each user can have many preferences, but each preference belongs to one user.

- **Notifications & Notification Templates**  
  Many-to-one relationship: Each notification can be created based on a template, but each template can be associated with multiple notifications.

- **Users & Push Notification Queue**  
  One-to-many relationship: Each user can have multiple push notifications queued, but each notification belongs to one user.

---

### **4. API Endpoints**

#### **Notifications Management**

- **GET /api/notifications**: Retrieve a list of all notifications for the authenticated user.
  - Query Parameters: `status` (optional), `delivery_channel` (optional)
  - Response:
    ```json
    [
      {
        "id": "uuid",
        "notification_type": "message",
        "message": "You have a new message from John.",
        "status": "sent",
        "delivery_channel": "email",
        "created_at": "2025-04-01T10:00:00Z"
      }
    ]
    ```

- **POST /api/notifications**: Create a new notification and send it to a user.
  - Request Body:
    ```json
    {
      "user_id": "uuid",
      "notification_type": "system",
      "message": "System maintenance scheduled for tonight.",
      "status": "pending",
      "delivery_channel": "in-app"
    }
    ```
  - Response:
    ```json
    {
      "status": "success",
      "message": "Notification created successfully"
    }
    ```

#### **Notification Preferences**

- **GET /api/notifications/preferences**: Retrieve notification preferences for the authenticated user.
  - Response:
    ```json
    [
      {
        "id": "uuid",
        "notification_type": "message",
        "email_enabled": true,
        "in_app_enabled": true,
        "push_enabled": false
      }
    ]
    ```

- **POST /api/notifications/preferences**: Update notification preferences for the authenticated user.
  - Request Body:
    ```json
    {
      "notification_type": "message",
      "email_enabled": true,
      "in_app_enabled": true,
      "push_enabled": false
    }
    ```
  - Response:
    ```json
    {
      "status": "success",
      "message": "Notification preferences updated successfully"
    }
    ```

#### **Push Notification Queue**

- **POST /api/notifications/push-queue**: Queue a push notification to be delivered later.
  - Request Body:
    ```json
    {
      "notification_id": "uuid",
      "scheduled_time": "2025-04-01T14:00:00Z"
    }
    ```
  - Response:
    ```json
    {
      "status": "success",
      "message": "Push notification queued successfully"
    }
    ```

---

### **5. Notification Channels Overview**

- **In-app Notifications**: Displayed within the application interface, can be accessed by users anytime within the app.
- **Email Notifications**: Sent to the user’s registered email address for important events, reminders, or alerts.
- **Push Notifications**: Sent as real-time alerts directly to the user’s device.

---

### **6. Notification Types**
- **Message Notifications**: Notifications about new messages, comments, likes, or interactions.
- **System Notifications**: Notifications triggered by system events such as maintenance or updates.
- **Alert Notifications**: Time-sensitive or critical notifications such as security warnings, account actions, or transaction alerts.

---

### **7. Summary of Key Features:**

- **Multi-channel Delivery**: In-app, email, and push notifications.
- **User Preferences**: Users can manage how and when they receive notifications.
- **Real-time Notifications**: Users are alerted immediately when key events occur.
- **Admin Broadcasts**: Admins can send broadcast messages to users.
- **Template System**: Predefined notification templates for system-wide events.
- **Queue System**: Notifications can be queued and scheduled for future delivery.

---
