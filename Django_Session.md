
# 🧠 Django Session — Complete Notes (Simple & Clear)

## 🔹 What Is a Session?

A **session** is a temporary way for Django to **remember information** about a user **between requests**.

> Every time a user visits a new page, Django uses the session to know who they are and what data belongs to them.

---

## 🔐 Relationship Between Authentication and Sessions

* **Authentication** confirms **who the user is**.
* **Session** remembers **which user is which** across different pages.

### 🧩 Example:

When a user logs in:

1. Django verifies their credentials (authentication).
2. Django creates a **session** for that user.
3. Django stores a **session ID** in the browser as a **cookie**.
4. On every next request, Django reads that cookie to identify the user.

Without sessions, users would have to log in again on **every page**.

---

## 🍪 How Django Sessions Work (Step by Step)

1️⃣ **User logs in** — Django authenticates them.
2️⃣ **Django creates a session** and stores info like:

```python
{'_auth_user_id': 5, '_auth_user_backend': 'django.contrib.auth.backends.ModelBackend'}
```

3️⃣ **Browser gets a cookie** called `sessionid` that contains a unique ID.
4️⃣ **Next requests** — the browser sends back that cookie.
5️⃣ Django uses that cookie to find the user’s session data in the database or cache.
6️⃣ **Logout** — session data is deleted.

---

## 💡 Session vs Cookie

| Feature  | **Session**                        | **Cookie**                |
| -------- | ---------------------------------- | ------------------------- |
| Stored   | On the **server**                  | On the **client browser** |
| Contains | Data like user ID, cart info, etc. | Session ID or small text  |
| Security | More secure                        | Less secure               |
| Use Case | Remember users between requests    | Store small public info   |

---

## ⚙️ Types of Sessions (Session Backends)

Django provides several ways to **store** session data.
You can change this in `settings.py` using the `SESSION_ENGINE` variable.

### 1️⃣ Database-backed Sessions (Default)

* Stores session data in the `django_session` table.

```python
SESSION_ENGINE = 'django.contrib.sessions.backends.db'
```

✅ Pros: Secure, easy to use
⚠️ Cons: Slightly slower than cache-based sessions

---

### 2️⃣ File-based Sessions

* Stores session data in files on your server.

```python
SESSION_ENGINE = 'django.contrib.sessions.backends.file'
SESSION_FILE_PATH = '/tmp/django_sessions'  # optional
```

✅ Pros: Simple setup
⚠️ Cons: Slower, cleanup required

---

### 3️⃣ Cache-based Sessions

* Stores session data in memory (Redis or Memcached).

```python
SESSION_ENGINE = 'django.contrib.sessions.backends.cache'
```

✅ Pros: Super fast, great for large-scale apps
⚠️ Cons: Data lost if cache cleared

---

### 4️⃣ Cached Database Sessions

* Uses both cache and database.

```python
SESSION_ENGINE = 'django.contrib.sessions.backends.cached_db'
```

✅ Pros: Best of both worlds — fast and reliable

---

### 5️⃣ Cookie-based Sessions

* Stores all data directly inside browser cookies.

```python
SESSION_ENGINE = 'django.contrib.sessions.backends.signed_cookies'
```

✅ Pros: No database required
⚠️ Cons: Limited size, less secure if misused

---

## 🧾 Summary Table

| Type          | Stored In         | Speed           | Security | Recommended For       |
| ------------- | ----------------- | --------------- | -------- | --------------------- |
| **Database**  | Database table    | Medium          | High     | Default, safe choice  |
| **File**      | File system       | Medium          | Medium   | Small/simple projects |
| **Cache**     | Redis / Memcached | Fast            | High     | Large-scale systems   |
| **Cached DB** | Cache + Database  | Fast + Reliable | High     | Production            |
| **Cookie**    | Browser           | Fast            | Medium   | Demos / small apps    |

---

## 🧰 Working with Sessions in Code

### ▶️ Add data to session:

```python
request.session['username'] = 'syed'
```

### 📥 Get data from session:

```python
name = request.session.get('username')
```

### ❌ Delete data from session:

```python
del request.session['username']
```

### 🔄 Clear all session data (logout):

```python
request.session.flush()
```

---

## ⏳ Session Expiry Settings

You can control how long a session lasts in `settings.py`.

```python
# Expire in 2 weeks (in seconds)
SESSION_COOKIE_AGE = 1209600  

# End session when browser closes
SESSION_EXPIRE_AT_BROWSER_CLOSE = True
```

---

## 🔄 Session Lifecycle

| Step        | Description                                   |
| ----------- | --------------------------------------------- |
| **Created** | When data is first stored (e.g., after login) |
| **Active**  | While user browses and cookie exists          |
| **Expired** | After time defined in `SESSION_COOKIE_AGE`    |
| **Deleted** | When user logs out or session is flushed      |

---

## 👥 Anonymous Sessions

Even users who are **not logged in** can have sessions.

> For example, an anonymous user adds items to a cart — Django creates a session for them to remember the cart items until they log in or close the browser.

---

## 🧠 Quick Recap

| Concept               | Meaning                      |
| --------------------- | ---------------------------- |
| **Authentication**    | Confirms who the user is     |
| **Session**           | Remembers user between pages |
| **Session ID**        | Unique key stored in cookie  |
| **Backend**           | Where session data is stored |
| **Anonymous Session** | Session created before login |

---

## 🧩 Real-Life Example (Analogy)

Think of a **restaurant** 🍽️

* When you enter, the waiter gives you a **token (cookie)**.
* Your order details are written in a **notebook (session storage)** using your token number.
* When you call the waiter again, they check your token and find your order.

That’s exactly how Django sessions work behind the scenes!

---

## ✅ In Short

* Authentication = “Who are you?”
* Session = “I’ll remember who you are for a while.”
* Cookie = “Here’s your ID card so I know you when you come back.”

