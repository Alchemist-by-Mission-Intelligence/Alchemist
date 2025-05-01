Here's the **Blog Categories Module** documentation in **Markdown format**, detailing the database schema, API endpoints, and relationships.

---

# 🗂 Blog Categories Module – Alchemist AI

The **Blog Categories Module** is used to organize blog posts into specific categories, making it easier for users to navigate the content. It helps to group related blog posts and enhance the SEO of the platform by categorizing content in a meaningful way.

---

## 📦 Features

- Create and manage categories for blog posts.
- Categories are hierarchical (optional), allowing for sub-categories.
- Each blog post can belong to only one category.
- SEO optimization for categories.
- Admin users can manage categories.
- API endpoints to interact with categories programmatically.

---

## 🧾 Database Schema

### Table: `blog_categories`

| Field          | Type       | Description                                                       |
|----------------|------------|-------------------------------------------------------------------|
| id             | UUID       | Primary key (UUID)                                                |
| name           | String     | Name of the category                                              |
| description    | String     | Description of the category                                       |
| parent_id      | UUID       | Foreign key referencing `blog_categories.id` for sub-categories (nullable) |
| seo_title      | String     | SEO title for the category                                         |
| seo_description| String     | SEO description for the category                                   |
| created_at     | TIMESTAMP  | Timestamp when the category was created                           |
| updated_at     | TIMESTAMP  | Timestamp when the category was last updated                      |

### Table: `blog_post_categories`

| Field          | Type       | Description                                                       |
|----------------|------------|-------------------------------------------------------------------|
| blog_post_id   | UUID       | Foreign key referencing `blog_posts.id`                           |
| category_id    | UUID       | Foreign key referencing `blog_categories.id`                      |

### Relationships

- **Blog Categories** can have **sub-categories** (self-referencing one-to-many relationship via `parent_id`).
- **Blog Categories** are linked to **Blog Posts** via the `blog_post_categories` pivot table. This allows many-to-many relationships, so a blog post can belong to multiple categories (if needed, based on requirements).

---

## 📡 API Endpoints

### 👤 Admin Blog Category Management

| Method | Endpoint                                | Description                                               |
|--------|-----------------------------------------|-----------------------------------------------------------|
| GET    | /api/admin/blog-categories              | Retrieve all blog categories                               |
| POST   | /api/admin/blog-categories              | Create a new blog category                                 |
| GET    | /api/admin/blog-categories/{id}         | Retrieve a specific blog category by ID                    |
| PUT    | /api/admin/blog-categories/{id}         | Update a specific blog category                             |
| DELETE | /api/admin/blog-categories/{id}         | Delete a specific blog category                             |

### 👤 User Blog Category Interaction

| Method | Endpoint                                | Description                                               |
|--------|-----------------------------------------|-----------------------------------------------------------|
| GET    | /api/user/blog-categories               | Retrieve all blog categories                               |
| GET    | /api/user/blog-categories/{id}          | Retrieve a specific blog category by ID                    |

---

## 📌 Notes

- **Parent-Child Categories**: The `parent_id` field allows for creating hierarchical categories (sub-categories). If `parent_id` is null, the category is a top-level category.
- **Pivot Table (`blog_post_categories`)**: This table establishes a many-to-many relationship between **Blog Posts** and **Categories**. It links each blog post to its category.
- **SEO Optimization**: Each category has **seo_title** and **seo_description** fields to improve search engine visibility.
- The **description** field helps provide more context for each category and can be used in the frontend for displaying category details.

---

## ✅ Future Enhancements

- [ ] Add **nested categories** for deep hierarchical structures.
- [ ] Implement **category-specific permissions**, allowing admins to restrict access based on category.
- [ ] Enable **category sorting** by drag-and-drop interface in the admin panel.
- [ ] Introduce **category-based recommendations** for blog posts.

---

This module is closely related to the **Blog Posts Module** as it organizes posts by category. It also integrates with the **Users Module** for managing who creates categories and the **SEO Module** for enhancing category visibility.

---