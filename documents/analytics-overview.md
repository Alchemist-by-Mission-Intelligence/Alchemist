### **Detailed Documentation for Analytics & Overview Module**

The **Analytics & Overview Module** provides a robust system to track and visualize user activity, system health, error logs, and traffic analytics. It also allows users to create custom analytics based on various entities and their data, empowering admins and users to generate tailored reports and insights. The module includes the following key features:

- **User Activity Tracking**: Track all actions users take within the system, including logins, page visits, and more.
- **System Health**: Monitor system performance, uptime, and key system metrics.
- **Error Logs**: Log errors in the system for troubleshooting.
- **Traffic Analytics**: Track overall traffic and engagement, such as page visits, new sign-ups, etc.
- **Custom Analytics Creation**: Allow users to create customized reports and analytics based on different entities and data sets.
  
---

### **1. Features Overview**

- **Activity Tracking**: Track user actions (e.g., page visits, actions, logins).
- **Custom Reports**: Users can define custom analytics based on entities like users, posts, subscriptions, etc.
- **Health & Error Logs**: Record and analyze errors and health metrics, such as uptime, system load, etc.
- **Traffic Monitoring**: Provide data on site traffic, active users, user demographics, and more.
- **Real-time Analytics**: Display real-time data on user activity and system status.
- **Exports**: Ability to export analytics reports in different formats (e.g., CSV, PDF).

---

### **2. Database Schema**

#### **Analytics Data Tables:**

1. **User Activity Logs Table**  
   This table records all user activities within the platform.

   | Field             | Type        | Description                                          |
   |-------------------|-------------|------------------------------------------------------|
   | id                | UUID        | Primary key                                          |
   | user_id           | UUID        | Foreign key referencing `users.id`                   |
   | activity_type     | String      | Type of activity (e.g., login, page visit, etc.)     |
   | activity_data     | JSON        | Additional data related to the activity              |
   | ip_address        | String      | User's IP address                                    |
   | user_agent        | String      | Browser/user-agent info                              |
   | created_at        | TIMESTAMP   | Timestamp when the activity occurred                 |

2. **Error Logs Table**  
   This table logs all system errors.

   | Field             | Type        | Description                                          |
   |-------------------|-------------|------------------------------------------------------|
   | id                | UUID        | Primary key                                          |
   | error_type        | String      | Type of error (e.g., 500, 404, etc.)                 |
   | message           | Text        | Detailed error message                               |
   | stack_trace       | Text        | Error stack trace (if available)                     |
   | created_at        | TIMESTAMP   | Timestamp of when the error occurred                 |

3. **Traffic Analytics Table**  
   This table tracks traffic data such as page views and active sessions.

   | Field             | Type        | Description                                          |
   |-------------------|-------------|------------------------------------------------------|
   | id                | UUID        | Primary key                                          |
   | page_url          | String      | The URL of the page that was visited                 |
   | views_count       | Integer     | The total number of views for the page               |
   | session_id        | String      | Unique session ID                                    |
   | user_id           | UUID        | Foreign key referencing `users.id` (nullable)        |
   | created_at        | TIMESTAMP   | Timestamp when the traffic occurred                  |

4. **Custom Analytics Table**  
   This table allows users to create and store custom analytics reports based on different entities.

   | Field             | Type        | Description                                          |
   |-------------------|-------------|------------------------------------------------------|
   | id                | UUID        | Primary key                                          |
   | user_id           | UUID        | Foreign key referencing `users.id`                   |
   | name              | String      | The name of the custom analytics report              |
   | description       | Text        | A description of the report                          |
   | entity_type       | String      | The type of entity being analyzed (e.g., "user", "post") |
   | filters           | JSON        | JSON object containing filters used for custom report |
   | created_at        | TIMESTAMP   | Timestamp when the custom analytics report was created |

5. **Custom Analytics Data Table**  
   Stores the generated data for custom reports.

   | Field             | Type        | Description                                          |
   |-------------------|-------------|------------------------------------------------------|
   | id                | UUID        | Primary key                                          |
   | custom_analytics_id | UUID      | Foreign key referencing `custom_analytics.id`        |
   | entity_id         | UUID        | The ID of the entity being tracked                   |
   | data              | JSON        | The data generated for this specific entity          |
   | created_at        | TIMESTAMP   | Timestamp when the data was generated                |

---

### **3. Relationships & Pivot Tables**

#### **User Activity Logs & Users**  
Each activity log is associated with a user. The relationship is one-to-many between `users` and `user_activity_logs`.

#### **Error Logs**  
The error logs are not directly associated with any user. They are system-wide logs.

#### **Traffic Analytics & Users**  
Each traffic log can optionally be associated with a user. The relationship is one-to-many, with `traffic_analytics` pointing to `users`.

#### **Custom Analytics & Users**  
Custom analytics reports are created by users. The relationship is one-to-many, where one user can create many custom analytics reports.

#### **Custom Analytics Data & Custom Analytics**  
The custom analytics data is tied to a specific custom analytics report. The relationship is one-to-many, with `custom_analytics_data` referencing `custom_analytics`.

---

### **4. API Endpoints**

#### **User Activity Logs**

- **GET /api/admin/analytics/activity**: Retrieve user activity logs.
  - Query Parameters: `?user_id=uuid&activity_type=type&date_range=start_date,end_date`
  - Response:
    ```json
    [
      {
        "id": "uuid",
        "user_id": "uuid",
        "activity_type": "login",
        "activity_data": {"browser": "Chrome"},
        "ip_address": "192.168.0.1",
        "user_agent": "Mozilla/5.0",
        "created_at": "2025-01-01T10:00:00Z"
      }
    ]
    ```

- **POST /api/analytics/activity**: Log a user activity.
  - Request Body: `{ "user_id": "uuid", "activity_type": "page_visit", "activity_data": {"page": "home"}, "ip_address": "192.168.0.1", "user_agent": "Mozilla/5.0" }`
  - Response: `{ "status": "success", "message": "Activity logged successfully" }`

#### **Error Logs**

- **GET /api/admin/analytics/errors**: Retrieve error logs.
  - Query Parameters: `?error_type=500&date_range=start_date,end_date`
  - Response:
    ```json
    [
      {
        "id": "uuid",
        "error_type": "500",
        "message": "Internal Server Error",
        "stack_trace": "Error at line 34",
        "created_at": "2025-01-01T11:00:00Z"
      }
    ]
    ```

#### **Traffic Analytics**

- **GET /api/admin/analytics/traffic**: Retrieve page traffic analytics.
  - Query Parameters: `?page_url=home&date_range=start_date,end_date`
  - Response:
    ```json
    [
      {
        "id": "uuid",
        "page_url": "home",
        "views_count": 1000,
        "session_id": "xyz123",
        "user_id": "uuid",
        "created_at": "2025-01-01T12:00:00Z"
      }
    ]
    ```

#### **Custom Analytics**

- **GET /api/admin/analytics/custom**: Get all custom analytics reports created by the user.
  - Query Parameters: `?user_id=uuid`
  - Response:
    ```json
    [
      {
        "id": "uuid",
        "user_id": "uuid",
        "name": "Custom User Report",
        "entity_type": "user",
        "filters": {"age": "25-30"},
        "created_at": "2025-01-01T13:00:00Z"
      }
    ]
    ```

- **POST /api/admin/analytics/custom**: Create a custom analytics report.
  - Request Body: 
    ```json
    {
      "user_id": "uuid",
      "name": "Custom User Report",
      "entity_type": "user",
      "filters": {"age": "25-30"}
    }
    ```
  - Response: `{ "status": "success", "message": "Custom report created successfully" }`

- **GET /api/admin/analytics/custom/{id}/data**: Retrieve the generated data for a specific custom analytics report.
  - Response:
    ```json
    [
      {
        "id": "uuid",
        "custom_analytics_id": "uuid",
        "entity_id": "uuid",
        "data": {"count": 100},
        "created_at": "2025-01-01T14:00:00Z"
      }
    ]
    ```

---

### **5. Summary of Key Features:**

- **Custom Analytics Creation**: Users and admins can create custom reports based on different entities and their data.
- **Real-Time Insights**: Users can track their activity and traffic in real time.
- **Health & Error Monitoring**: Real-time error and health logs provide valuable insights for troubleshooting.
- **Search & Filters**: Users can filter logs and analytics by date ranges, activity types, and other parameters.
- **Export Options**: Admins can export logs and reports in various formats (e.g., CSV, PDF).

This concludes the documentation for the **Analytics & Overview Module**. If you need additional information or clarifications, feel free to ask!