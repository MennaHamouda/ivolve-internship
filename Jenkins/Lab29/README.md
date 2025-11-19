# Lab 29: Role-based Authorization

This lab demonstrates how to implement basic role-based access control
(RBAC) by creating two IAM users with different permission levels.

## 🎯 Objectives

-   Create **user1** and **user2**
-   Assign:
    -   **Admin role** → user1
    -   **Read-only role** → user2

## 🛠 Steps

### 1. Create IAM Users

1.  Go to **IAM Console** → **Users**
2.  Click **Create User**
3.  Create:
    -   `user1`
    -   `user2`
4.  Enable **Programmatic access** & **Console password** if required.

### 2. Attach Roles / Permissions

#### user1 (Admin User)

1.  Select **user1**
2.  Go to **Permissions → Add Permissions**
3.  Choose **Attach policies directly**
4.  Select:
    -   `AdministratorAccess`
5.  Click **Add Permissions**

#### user2 (Read-Only User)

1.  Select **user2**
2.  Go to **Permissions → Add Permissions**
3.  Choose **Attach policies directly**
4.  Select:
    -   `ReadOnlyAccess`
5.  Click **Add Permissions**

## 📌 Verification

-   Log in as **user1** → create resources (should work)
-   Log in as **user2** → create resources (should fail with
    "AccessDenied")
