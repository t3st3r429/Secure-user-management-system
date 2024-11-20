This is a simple  Secure User Management System in Linux 

### 1. Set Up Project Structure
Your project should be well-organized, and you can structure it in a way that separates scripts, configuration files, and documentation. Here’s a sample directory structure:

```
secure-user-management-linux/
├── README.md
├── LICENSE
├── user-management-scripts/
│   ├── create_user.sh
│   ├── delete_user.sh
│   ├── modify_user.sh
│   └── manage_permissions.sh
├── configs/
│   ├── sudoers_config
│   └── passwd_config
├── docs/
│   ├── setup_guide.md
│   └── security_best_practices.md
└── tests/
    └── test_user_management.sh
```

### 2. Features

```markdown

## Overview
This project implements a Secure User Management System for Linux-based systems, focusing on the secure creation, modification, and deletion of users, along with tight control over user permissions and access. The system follows best practices for password management, auditing, and permission control to ensure a secure environment.

## Features
- User creation and deletion with permission control.
- Modification of user roles and sudo access.
- Secure password management and storage.
- Logging user activities and security events.
- Integration with system security tools such as PAM and SELinux.

```

###  Navigate to the project directory:
```bash
cd secure-user-management-linux
```

### 3. Ensure the necessary utilities are installed:
```bash
sudo apt install sudo useradd passwd
```

### 4. Run scripts for user management:
```bash
./user-management-scripts/create_user.sh username
```

## Usage
To create a new user with secure permissions:
```bash
./user-management-scripts/create_user.sh username
```

For a detailed setup and security best practices, see [setup guide](docs/setup_guide.md)

###5.Create User Management Scripts

The core of your project will be bash scripts that handle user management tasks like creating, modifying, and deleting users. Below are some examples of what these scripts might look like:

#### `create_user.sh`

```bash
#!/bin/bash

# Check if a username is provided
if [ -z "$1" ]; then
  echo "Usage: $0 <username>"
  exit 1
fi

USER=$1

# Check if the user already exists
if id "$USER" &>/dev/null; then
  echo "User '$USER' already exists!"
  exit 1
fi

# Create the user
useradd -m -s /bin/bash "$USER"
passwd "$USER"  # Set password for the user

# Secure sudo permissions for the user
echo "$USER ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers.d/$USER

# Log the user creation event
echo "$(date) - Created user: $USER" >> /var/log/user_management.log

echo "User '$USER' created successfully!"
```

#### `delete_user.sh`

```bash
#!/bin/bash

# Check if a username is provided
if [ -z "$1" ]; then
  echo "Usage: $0 <username>"
  exit 1
fi

USER=$1

# Check if the user exists
if ! id "$USER" &>/dev/null; then
  echo "User '$USER' does not exist!"
  exit 1
fi

# Delete the user and their home directory
userdel -r "$USER"

# Log the user deletion event
echo "$(date) - Deleted user: $USER" >> /var/log/user_management.log

echo "User '$USER' deleted successfully!"
```

### 6. Configuration Files

You may need configuration files to specify user permissions or password policies. For example:

#### `sudoers_config`

Ensure proper access control for specific users or groups. It could include configurations like:

```
# Allow the user to execute commands as root
user1    ALL=(ALL) NOPASSWD: ALL
```

#### `passwd_config`

Specify password policy rules (e.g., minimum password length or password expiration). Example:

```
# Enforce strong passwords
PASS_MIN_LEN 8
PASS_WARN_AGE 7
```

### 7. Testing Your Scripts

In the `tests/` folder, you can write bash scripts to automate the testing of your user management scripts. For example:

#### `test_user_management.sh`

```bash
#!/bin/bash

# Test creating a new user
./user-management-scripts/create_user.sh testuser
if id "testuser" &>/dev/null; then
  echo "User creation test passed!"
else
  echo "User creation test failed."
fi

# Test deleting the user
./user-management-scripts/delete_user.sh testuser
if ! id "testuser" &>/dev/null; then
  echo "User deletion test passed!"
else
  echo "User deletion test failed."
fi
```

### 8. Security Best Practices Documentation

Add a `docs/` folder with files that explain security best practices for user management, password policies, logging, and auditing. For example:

- `security_best_practices.md`: Discuss how to securely manage users, enforce strong password policies, and use sudo privileges responsibly.
  
- `setup_guide.md`: Step-by-step instructions for deploying the system, configuring user roles, and setting up logs.



### 9. Security Considerations
- Secure Password Management: Use strong password policies and consider integrating PAM (Pluggable Authentication Modules) for better password management.
- Role-Based Access Control (RBAC): Implement RBAC to ensure users only have access to what they need.
- Logging: Log all user creation, deletion, and modification actions for auditing purposes.
- Least Privilege Principle: Ensure users have the minimum necessary permissions to perform their tasks.

---

