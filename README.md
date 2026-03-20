# 🔐 Secure Django LDAP

<div align="center">

[![Python](https://img.shields.io/badge/Python-3.x-blue?style=for-the-badge&logo=python)](https://www.python.org/)
[![Django](https://img.shields.io/badge/Django-5.0-success?style=for-the-badge&logo=django)](https://www.djangoproject.com/)
[![Django REST Framework](https://img.shields.io/badge/DRF-3.14-red?style=for-the-badge&logo=django)](https://www.django-rest-framework.org/)
[![LDAP](https://img.shields.io/badge/Integration-LDAP-purple?style=for-the-badge)](#)
[![Database](https://img.shields.io/badge/Database-SQL-blue?style=for-the-badge)](#)

**A robust, secure, and hybrid authentication API combining the centralized user management of LDAP with the stateless security of JSON Web Tokens (JWT).**

</div>

## 📖 Overview

This project provides a foundational backend solution for user authentication. Developed using Python and the Django REST Framework, it bridges legacy directory services with modern front-end architectures. By integrating LDAP for automated account provisioning and synchronization.

### 🙏 Acknowledgments
This project was successfully completed as a dedicated task at **IUCAA (Inter-University Centre for Astronomy and Astrophysics)**. Special thanks to **Mr. Harshad Savant** and **Mr. Dipak Joshi** for their invaluable guidance, as well as for providing the necessary LDAP server infrastructure and resources to bring this system to life.

## ✨ Core Features

- **Hybrid Authentication**: Validates user credentials against an LDAP directory and issues secure JWT access and refresh tokens for API authorization.
- **Automated LDAP Provisioning**: New users created via the API registration workflow are automatically provisioned in the LDAP directory.
- **Bi-Directional Profile Sync**: Updates to the user's name, email, or phone number in the Django database automatically cascade to the LDAP server.
- **Custom User Architecture**: Extends Django's `AbstractUser` to include a `phone_number` field and enforces automated name capitalization.
- **Automated Cleanup**: A pre-delete signal guarantees that LDAP entries are immediately removed when a user is deleted from the local database.
- **OTP Verification**: Includes an integrated verification system for password setup and recovery (uses static OTPs for testing: **Phone: 111111**, **Email: 222222**).
- 
## 🛠️ Tech Stack

* **Backend Framework:** Python 3.11, Django 5.2.6, Django REST Framework
* **Authentication:** `python-ldap` 3.4.4, `django-auth-ldap` 5.2.0, PyJWT
* **Database:** Compatible with MySQL (`accounts_db`) and PostgreSQL via configuration.

## 🚀 Quick Start

### 1. System Prerequisites
Before installing Python packages, you must install the following OS-level dependencies required to build the `python-ldap` library:

```bash
sudo apt-get update
sudo apt-get install libldap2-dev libsasl2-dev python3.11-dev
````

### 2\. Installation and Setup

**Clone the repository and set up the environment:**

```bash
git clone [https://github.com/infin8-innov8/Proj8_authentication_system_task.git](https://github.com/infin8-innov8/Proj8_authentication_system_task.git)
cd Proj8_authentication_system_task
python -m venv venv
source venv/bin/activate  # On Windows use: .\venv\Scripts\activate
pip install -r requirements.txt
```

### 3\. Environment Variables

Create a `.env` file in the project root to manage sensitive configurations and LDAP bindings:

```env
# Django & Database
SECRET_KEY=your_super_secret_django_key_here
DEBUG=True
ALLOWED_HOSTS=localhost,127.0.0.1
DATABASE_URL=mysql://accountant:Activ8*o@localhost:3306/accounts_db  # Or postgres://...

# LDAP Server Configuration
AUTH_LDAP_SERVER_URI=ldap://your-ldap-server
AUTH_LDAP_BIND_DN=cn=admin,dc=example,dc=com
AUTH_LDAP_BIND_PASSWORD=your-ldap-password
AUTH_LDAP_SEARCH_BASE_DN=ou=users,dc=example,dc=com
LDAP_USERS_BASE_DN=ou=users,dc=example,dc=com
```

### 4\. Database Initialization

Apply migrations to build the schemas for the custom user model and authentication tables:

```bash
python manage.py makemigrations
python manage.py migrate
python manage.py runserver
```

## 📚 API Reference

All endpoints are available under the configured API path (e.g., `/api/` or `/accounts/`) and communicate strictly via JSON.

### Public Endpoints

  * `POST /register/`: Creates a new user in both the Django database and LDAP directory.
  * `POST /login/`: Authenticates credentials (username/email) against LDAP/Django and returns JWT `access` and `refresh` tokens.
  * `POST /token/refresh/`: Issues a new access token using a valid refresh token.
  * `POST /verification/`: OTP verification workflow.
  * `POST /forgot_password/`: Initiates the password recovery protocol.
  * `POST /reset_password/`: Finalizes password reset using a verified OTP.

### Protected Endpoints (Requires `Authorization: Bearer <token>`)

  * `GET /profile/`: Retrieves the authenticated user's profile data.
  * `PUT /profile/`: Updates user information (automatically syncs changes to LDAP).
  * `GET /home/`: Accesses the protected user dashboard.
  * `POST /logout/`: Terminates the session (client-side token deletion/blacklisting).

## 🧰 Management Commands

The project includes custom Django management commands to simplify administration. You can create an administrative user in both the local database and the LDAP directory simultaneously:

```bash
python manage.py create_ldap_superuser --username <user> --email <email> --first_name <first> --last_name <last> --phone_number <phone>
```

## 📄 License

This project is licensed under the [MIT License](https://www.google.com/search?q=LICENSE) - see the https://www.google.com/search?q=LICENSE file for details.

-----

\<div align="center"\>

**⭐ Star this repo if you find it helpful\!**

Made with ❤️ by Pranav Vasankar [infin8-innov8]

\</div\>

```

Would you like me to review any specific views or serializer logic to ensure the LDAP synchronization functions smoothly with your JWT setup?
```
