

---

# 🧾 Django Authentication Models – Study Notes

**By: Syed Mufassir Shah**

---

## 🧠 Overview

Django has a built-in authentication system that manages **users, passwords, login, and permissions**.
There are **3 main base classes** we can use when creating or extending user models:

---

## 🔹 1. `User` (Default Built-in Model)

* Comes ready with fields like:

  ```python
  username, email, password, first_name, last_name, is_staff, is_active, date_joined
  ```
* Provides login, registration, and permission features automatically.
* Use it if you don’t need to change the basic user structure.
* You can extend it using a **Profile model** (One-to-One relationship).

### Example:

```python
from django.db import models
from django.contrib.auth.models import User

class Profile(models.Model):
    user = models.OneToOneField(User, on_delete=models.CASCADE)
    phone_number = models.CharField(max_length=15, blank=True)
    bio = models.TextField(blank=True)
    profile_picture = models.ImageField(upload_to='profiles/', blank=True, null=True)

    def __str__(self):
        return self.user.username
```

✅ **Use case:** You only want to add extra fields (like bio, phone number) but keep Django’s default authentication system.

---

## 🔹 2. `AbstractUser`

* It is a **customizable version of Django’s default user**.
* Inherits everything from the built-in `User` model.
* You can **add or remove fields** directly in the user table itself.
* Login, password hashing, and permissions still work the same.

### Example:

```python
from django.contrib.auth.models import AbstractUser
from django.db import models

class CustomUser(AbstractUser):
    phone_number = models.CharField(max_length=15, blank=True)
    bio = models.TextField(blank=True)
```

✅ **Use case:** You want to customize the user model but still keep Django’s full authentication logic.

---

## 🔹 3. `AbstractBaseUser`

* The **lowest-level** user model class.
* Gives you **only**:

  * `password`
  * `last_login`
  * `set_password()` and `check_password()` methods
* You must define:

  * Your own fields (like email or username)
  * Your own manager (`BaseUserManager`)
  * Your own login field (`USERNAME_FIELD`)
  * Logic for creating users and superusers

### Example:

```python
from django.db import models
from django.contrib.auth.models import AbstractBaseUser, BaseUserManager

class MyUserManager(BaseUserManager):
    def create_user(self, email, password=None, **extra_fields):
        if not email:
            raise ValueError("Email is required")
        email = self.normalize_email(email)
        user = self.model(email=email, **extra_fields)
        user.set_password(password)
        user.save(using=self._db)
        return user

    def create_superuser(self, email, password=None, **extra_fields):
        extra_fields.setdefault('is_staff', True)
        extra_fields.setdefault('is_superuser', True)
        return self.create_user(email, password, **extra_fields)

class MyUser(AbstractBaseUser):
    email = models.EmailField(unique=True)
    name = models.CharField(max_length=100)
    is_active = models.BooleanField(default=True)
    is_staff = models.BooleanField(default=False)
    is_superuser = models.BooleanField(default=False)

    USERNAME_FIELD = 'email'
    REQUIRED_FIELDS = ['name']

    objects = MyUserManager()
```

✅ **Use case:** You want **full control** over your user model — e.g., using email instead of username for login.

---

## 🔐 Password Hashing and Authentication Flow

* Django **never stores passwords in plain text**.
* It uses secure algorithms like **PBKDF2** with a random salt.

### Key Methods

| Method                         | Purpose                                    |
| ------------------------------ | ------------------------------------------ |
| `set_password(raw_password)`   | Hashes the password before saving          |
| `check_password(raw_password)` | Compares entered password with stored hash |
| `authenticate()`               | Finds user + checks password internally    |

### Example:

```python
user = MyUser.objects.create_user(email="syed@example.com", password="abc123")
user.check_password("abc123")  # ✅ True
user.check_password("wrongpass")  # ❌ False
```

Django uses these automatically when logging in with:

```python
from django.contrib.auth import authenticate, login
```

---

##⚙️ Important Setting

If you’re using a custom user model:

```python
# settings.py
AUTH_USER_MODEL = 'yourapp.MyUser'
```

Do this **before** running migrations for the first time.

---

## 🧾 Summary Table

| Model Type           | Features                   | Authentication Handling            | When to Use                                     |
| -------------------- | -------------------------- | ---------------------------------- | ----------------------------------------------- |
| **User**             | Default Django user        | Built-in                           | When you need basic authentication              |
| **AbstractUser**     | Customizable built-in user | Handled automatically              | When you want to add/remove fields              |
| **AbstractBaseUser** | Fully custom user          | Must define with `BaseUserManager` | When you want total control (e.g., email login) |

---

## 🔑 One-to-One vs ForeignKey

* `OneToOneField` → one user → one profile
* `ForeignKey` → one user → many profiles

So they are **not the same**.
`OneToOneField` is basically a special kind of `ForeignKey` that ensures **only one related object per user**.

---

## ✅ Quick Recap

* `AbstractUser` → Extend Django’s user easily
* `AbstractBaseUser` → Build your own user system
* `BaseUserManager` → Handles user and superuser creation
* `set_password()` and `check_password()` → Handle password security
* `authenticate()` → Handles user login checking internally

