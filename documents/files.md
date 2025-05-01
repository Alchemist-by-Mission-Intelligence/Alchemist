Here is the **Files Module** documentation in **Markdown format**, including the database schema, API endpoints, and relationships.

---

# 📝 Files Module – Alchemist AI

The **Files Module** enables the uploading, management, and storage of files within the platform. It allows users to upload files for various purposes, such as supporting documentation for support tickets, user-generated content, or any other relevant data. Files are securely stored and can be easily managed by both users and admins.

---

## 📦 Features

- **File Upload**: Users and admins can upload files.
- **File Management**: Allows for easy viewing, downloading, and managing of uploaded files.
- **File Types**: Support for various file types, such as images, documents, and PDFs.
- **Access Control**: Control file access based on user roles and permissions.
- **File Deletion**: Admins can delete files that are no longer needed.
- **File Versioning**: Support for versioning, allowing users to upload newer versions of the same file.

---

## 🧾 Database Schema

### Table: `files`

| Field             | Type        | Description                                                            |
|-------------------|-------------|------------------------------------------------------------------------|
| id                | UUID        | Primary key (UUID)                                                     |
| user_id           | UUID        | Foreign key referencing `users.id` (user who uploaded the file)       |
| fileable_id       | UUID        | Polymorphic relation to other entities (e.g., tickets, posts)          |
| fileable_type     | String      | Polymorphic relation type (e.g., 'SupportTicket', 'BlogPost')         |
| file_name         | String      | The original name of the file                                          |
| file_path         | String      | Path where the file is stored                                         |
| file_size         | Integer     | Size of the file in bytes                                             |
| file_type         | String      | Mime type of the file (e.g., "image/jpeg", "application/pdf")         |
| created_at        | TIMESTAMP   | Timestamp when the file was uploaded                                  |
| updated_at        | TIMESTAMP   | Timestamp when the file details were last updated                     |

### Relationships

- **Files** belong to a **User** (`user_id`), who uploaded the file.
- **Files** can belong to any **fileable model** via polymorphic relationships (e.g., `SupportTicket`, `BlogPost`).

### Polymorphic Relationships

The `fileable` polymorphic relation allows files to be associated with various entities, such as:
- **Support Tickets**
- **Blog Posts**
- Any other models that require file uploads

---

## 📡 API Endpoints

### User API Endpoints

| Method | Endpoint                     | Description                                                       |
|--------|------------------------------|-------------------------------------------------------------------|
| POST   | /api/user/files               | Upload a new file                                                  |
| GET    | /api/user/files               | Retrieve all files uploaded by the authenticated user             |
| GET    | /api/user/files/{id}          | Retrieve a specific file by ID for the authenticated user          |
| DELETE | /api/user/files/{id}          | Delete a file uploaded by the authenticated user                   |

### Admin API Endpoints

| Method | Endpoint                     | Description                                                       |
|--------|------------------------------|-------------------------------------------------------------------|
| GET    | /api/admin/files              | Retrieve all files uploaded on the platform                       |
| GET    | /api/admin/files/{id}         | Retrieve a specific file by ID                                     |
| DELETE | /api/admin/files/{id}         | Delete a file (admin only)                                         |
| GET    | /api/admin/files/search       | Search files by user, file type, or associated entity (e.g., support tickets, blog posts) |

---

## 🔑 Notes

- **File Upload**: Users can upload files related to their account or activities. The files can be linked to various entities (e.g., support tickets, blog posts) through the polymorphic relation.
- **File Types**: The system supports a wide range of file types, ensuring compatibility with common document formats and images.
- **File Management**: Users can view their uploaded files and admins can manage all files uploaded on the platform.
- **Access Control**: Files are tied to specific users and entities, and access can be controlled based on the file's relationship with other modules (e.g., Support Tickets, Blog Posts).
- **File Deletion**: Admins have the ability to delete files that are no longer needed, while users can only delete their own uploaded files.
- **File Versioning**: Versioning allows users to upload new versions of the same file if necessary (e.g., when updating a document or image).

---

## ✅ Future Enhancements

- [ ] **File Compression**: Automatically compress large files upon upload to optimize storage space.
- [ ] **File Preview**: Add preview capabilities for certain file types (e.g., images, PDFs).
- [ ] **Access Logs**: Track which users have accessed or downloaded files.
- [ ] **File Search**: Implement advanced search features to allow users and admins to search files by name, type, or associated entity.
- [ ] **File Sharing**: Allow users to share files with other users within the platform.

---

This module is directly related to the **Users Module**, as each file is associated with a user who uploads it. The **Roles and Permissions Module** governs who can upload, view, or delete files. Additionally, the **Support Tickets Module** can make use of this module to attach files to tickets, and the **Blog Posts Module** can use it to handle media files related to blog content.

---

Would you like to proceed with additional modules, or do you need further clarification on this one?