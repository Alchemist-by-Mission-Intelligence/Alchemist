Here's the **Blog Posts Module** documentation in **Markdown format**, detailing the database schema, API endpoints, and relationships.

---

# 📝 Blog Posts Module – Alchemist AI

The **Blog Posts Module** allows users (admin or authors) to create, manage, and display blog content on the platform. The module supports SEO optimization, categorization, and tagging. This module is essential for maintaining content that keeps users engaged, informs them, and improves site SEO.

---

## 📦 Features

- Centralized blog management for admins and authors.
- SEO optimization for each blog post.
- Categorization and tagging of blog posts.
- Support for rich text formatting and media embedding (images, videos).
- Ability for users to comment on blog posts (optional).
- Admin and author roles for controlling access to create/edit posts.
- Integration with the **Blog Categories** module for categorizing posts.

---

## 🧾 Database Schema

### Table: `blog_posts`

| Field          | Type       | Description                                                       |
|----------------|------------|-------------------------------------------------------------------|
| id             | UUID       | Primary key (UUID)                                                |
| title          | String     | Title of the blog post                                            |
| slug           | String     | SEO-friendly slug for the blog post                               |
| content        | Text       | Main content of the blog post (supports HTML or Markdown)         |
| summary        | String     | Short description or excerpt of the blog post                     |
| seo_title      | String     | SEO title for the blog post                                       |
| seo_description| String     | SEO description for the blog post                                 |
| image          | String     | URL or path to the blog post image (optional)                     |
| category_id    | UUID       | Foreign key referencing `blog_categories.id`                      |
| tags           | JSON       | JSON array of tags associated with the blog post                  |
| status         | String     | Status of the blog post (draft, published, archived)             |
| created_at     | TIMESTAMP  | Timestamp when the blog post was created                          |
| updated_at     | TIMESTAMP  | Timestamp when the blog post was last updated                     |

### Table: `blog_categories`

| Field          | Type       | Description                                                       |
|----------------|------------|-------------------------------------------------------------------|
| id             | UUID       | Primary key (UUID)                                                |
| name           | String     | Name of the category                                              |
| description    | String     | Description of the category                                       |
| created_at     | TIMESTAMP  | Timestamp when the category was created                           |
| updated_at     | TIMESTAMP  | Timestamp when the category was last updated                      |

### Table: `blog_post_logs`

| Field          | Type       | Description                                                       |
|----------------|------------|-------------------------------------------------------------------|
| id             | UUID       | Primary key (UUID)                                                |
| blog_post_id   | UUID       | Foreign key referencing `blog_posts.id`                           |
| user_id        | UUID       | Foreign key referencing `users.id`                                |
| action         | String     | Type of action performed (e.g., created, updated, deleted)        |
| created_at     | TIMESTAMP  | Timestamp when the log entry was created                          |

### Relationships

- **Blog Posts** belong to **Blog Categories** (many-to-one relationship).
- **Blog Posts** can have multiple **Tags** (stored as a JSON array in the `tags` field).
- **Blog Post Logs** track actions on blog posts (create, update, delete), with references to both the **Blog Post** and the **User** who performed the action.

---

## 📡 API Endpoints

### 👤 Admin & Author Blog Post Management

| Method | Endpoint                                | Description                                               |
|--------|-----------------------------------------|-----------------------------------------------------------|
| GET    | /api/admin/blog-posts                   | Retrieve all blog posts                                   |
| POST   | /api/admin/blog-posts                   | Create a new blog post                                    |
| GET    | /api/admin/blog-posts/{id}              | Retrieve a specific blog post by ID                       |
| PUT    | /api/admin/blog-posts/{id}              | Update an existing blog post                               |
| DELETE | /api/admin/blog-posts/{id}              | Delete a specific blog post                               |

### 👤 Blog Categories Management

| Method | Endpoint                                | Description                                               |
|--------|-----------------------------------------|-----------------------------------------------------------|
| GET    | /api/admin/blog-categories              | Retrieve all blog categories                               |
| POST   | /api/admin/blog-categories              | Create a new blog category                                |
| GET    | /api/admin/blog-categories/{id}         | Retrieve a specific blog category by ID                   |
| PUT    | /api/admin/blog-categories/{id}         | Update a specific blog category                            |
| DELETE | /api/admin/blog-categories/{id}         | Delete a specific blog category                            |

### 👤 User Blog Post Interaction (Optional)

| Method | Endpoint                                | Description                                               |
|--------|-----------------------------------------|-----------------------------------------------------------|
| GET    | /api/user/blog-posts                    | Retrieve all published blog posts                          |
| GET    | /api/user/blog-posts/{slug}             | Retrieve a specific blog post by slug                      |
| POST   | /api/user/comment/{blog_post_id}        | Post a comment on a blog post (optional)                   |

---

## 📌 Notes

- **Blog Categories** allow admin and authors to group related posts together. Posts can belong to only one category, but categories can have multiple posts.
- **Tags** are stored as JSON arrays in the **blog_posts** table, allowing for flexible tagging of posts.
- **SEO Optimization** is supported for each blog post with customizable **seo_title** and **seo_description** fields.
- The **status** field in the **blog_posts** table allows you to manage posts in different states (e.g., draft, published, archived).
- **Blog Post Logs** track administrative actions such as creation, updates, and deletions.
- The **comment** feature is optional and can be enabled or disabled based on project requirements.

---

## ✅ Future Enhancements

- [ ] Add **comment moderation** to allow admins to review and approve comments before they are published.
- [ ] Enable **multi-language support** for blog posts to target global audiences.
- [ ] Implement **A/B testing** for blog post titles and content to increase engagement.
- [ ] Integrate with **social media sharing** to allow users to easily share blog posts.

---

This module is closely integrated with the **Users** module for tracking authorship and interactions, the **Audit Logs** module for tracking changes, and the **Email Templates** module for notifying users about new blog posts (optional).

---
