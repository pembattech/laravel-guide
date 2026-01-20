# 🔐 API Authentication (Sanctum)

### Method 1: Using php artisan api:install

This command **automatically configures API authentication** (Sanctum + API routes).

```bash
php artisan api:install
```

What this does:

* Installs **Laravel Sanctum**
* Publishes Sanctum config
* Creates `routes/api.php` (if missing)
* Sets up API middleware
* Prepares token-based authentication

After that, run migrations:

```bash
php artisan migrate
```

### Method 2: Manual Sanctum Installation (Older Laravel / Custom Setup)

Use this if you want full control:

```bash
composer require laravel/sanctum
php artisan vendor:publish --provider="Laravel\Sanctum\SanctumServiceProvider"
php artisan migrate
```

---

### 🔒 Protect API Routes with Sanctum

```php
Route::middleware('auth:sanctum')->get('/user', function (Request $request) {
    return $request->user();
});
```

---

### 🔑 Issue Token (Login Example)

```php
$token = $user->createToken('api-token')->plainTextToken;
```

Response example:

```json
{
  "token": "1|XyZabc123..."
}
```

---

### 📡 Send Token from Frontend (jQuery / AJAX)

```js
$.ajax({
    url: "/api/profile",
    method: "GET",
    headers: {
        "Authorization": "Bearer YOUR_TOKEN_HERE",
        "Accept": "application/json"
    },
    success: function (res) {
        console.log(res);
    }
});
```

Perfect section 👍
Below is a **drop-in continuation** you can add **after your AJAX example**, written as a **best-practice guide for frontend token storage + optimization**, especially relevant for **jQuery / AJAX + Sanctum** projects like *MeroTable*.

---

## 🔐 Secure Token Storage (Frontend Best Practices)

### ❌ What NOT to Do

Avoid these in production:

* ❌ Storing token in **plain global JS variables**
* ❌ Hard-coding token in JS files
* ❌ Sending token without `Accept: application/json`
* ❌ Exposing token in URLs (`?token=...`)

---

## ✅ Recommended Ways to Store Token

### 🥇 Option 1: In-Memory Storage (Most Secure – SPA)

Best for **single-page apps** or dashboards.

```js
let authToken = null;

// after login
authToken = response.token;
```

✔ Token disappears on refresh
✔ Safe from XSS persistence
❌ User must re-login on refresh

**Use when:** Admin panels, POS systems, kiosks

---

### 🥈 Option 2: localStorage (Common & Practical)

```js
// Store token
localStorage.setItem('auth_token', response.token);

// Retrieve token
const token = localStorage.getItem('auth_token');
```

✔ Survives page refresh
✔ Simple
❌ Vulnerable to XSS if site is compromised

🔐 **Mitigation Tips**

* Never use `innerHTML` with user input
* Always escape data
* Use strict CSP headers

---

### 🥉 Option 3: sessionStorage (Safer than localStorage)

```js
sessionStorage.setItem('auth_token', response.token);
```

✔ Cleared when tab closes
✔ Less persistent than localStorage
✔ Good balance for dashboards

---

### 🚫 Why Cookies Are Not Recommended (Token Mode)

Sanctum token mode **does NOT need cookies**.

Cookies:

* Require CSRF protection
* Can be sent automatically (risk)
* Better suited for **SPA cookie auth**, not API tokens

---

## 🔄 Centralize AJAX Authorization (Optimization)

### ✅ Set Global Authorization Header (Recommended)

```js
$.ajaxSetup({
    headers: {
        'Authorization': 'Bearer ' + localStorage.getItem('auth_token'),
        'Accept': 'application/json'
    }
});
```

✔ No repetition
✔ Cleaner code
✔ Easy token rotation

---

## 🔁 Handle Token Expiry Automatically

### Global 401 Handler

```js
$(document).ajaxError(function (event, jqxhr) {
    if (jqxhr.status === 401) {
        localStorage.removeItem('auth_token');
        window.location.href = '/login';
    }
});
```

✔ Prevents infinite errors
✔ Improves UX
✔ Auto-logout on expiry

---

## 🚀 Performance & Security Optimizations

### ✅ Use Token Abilities (Scopes)

```php
$token = $user->createToken('api-token', ['orders', 'billing'])->plainTextToken;
```

```php
Route::middleware(['auth:sanctum', 'abilities:orders'])->post('/order', ...);
```

✔ Limits damage if token leaks
✔ Role-based API access

---

### 🔁 Rotate Tokens on Login

```php
$user->tokens()->delete();
$token = $user->createToken('api-token')->plainTextToken;
```

✔ One active session
✔ Prevents token reuse

---

### 🧹 Logout (Revoke Token)

```php
$request->user()->currentAccessToken()->delete();
```

Frontend:

```js
localStorage.removeItem('auth_token');
```

---

## 🛡 Additional Security Hardening

✔ Use HTTPS **only**
✔ Set short token lifetime (custom logic)
✔ Rate-limit login routes
✔ Validate `Accept: application/json` in API
✔ Never expose token in error messages
✔ Log suspicious token usage

---

## 🧠 Summary (Best Practice Stack)

| Area          | Recommendation                |
| ------------- | ----------------------------- |
| Token storage | sessionStorage / localStorage |
| AJAX          | Global header setup           |
| Security      | Token abilities + rotation    |
| UX            | Auto 401 handling             |
| Backend       | Sanctum + rate limit          |

---

# Security Hardening

Here’s a **clean, production-ready security checklist** you can directly add to your Laravel + Sanctum documentation.
This is written with **API-first projects**, **jQuery/AJAX frontends**, and systems like **MeroTable** in mind.

---

# 🛡 Production Security Checklist (Laravel + Sanctum)

## 🔐 Authentication & Tokens

✔ Use **Laravel Sanctum** for API authentication
✔ Always protect routes with `auth:sanctum`
✔ Use **token abilities (scopes)**
✔ Delete old tokens on login (token rotation)
✔ Revoke token on logout
✔ Never expose tokens in URLs or logs

```php
$user->tokens()->delete();
$token = $user->createToken('api-token', ['orders'])->plainTextToken;
```

---

## 🌐 API Request Hardening

✔ Always require:

```http
Accept: application/json
Authorization: Bearer TOKEN
```

✔ Return JSON-only error responses
✔ Handle `401 Unauthorized` globally in frontend
✔ Do not leak stack traces in API responses

```env
APP_ENV=production
APP_DEBUG=false
```

---

## 🧾 Frontend Token Safety

✔ Store token in:

* `sessionStorage` (recommended)
* `localStorage` (acceptable with XSS protection)

❌ Do NOT:

* Hardcode token
* Store in JS files
* Inject via HTML
* Send in query strings

✔ Clear token on:

* Logout
* 401 response
* Session timeout

---

## 🔥 XSS & CSRF Protection

✔ Escape all dynamic HTML
✔ Avoid `innerHTML` with user input
✔ Validate & sanitize all API input
✔ Use strict Content Security Policy (CSP)

Sanctum **token-based APIs do NOT need CSRF**.

---

## 🚦 Rate Limiting (VERY IMPORTANT)

✔ Protect login endpoints:

```php
Route::post('/login', ...)->middleware('throttle:5,1');
```

✔ Apply rate limits to:

* Login
* OTP
* Order creation
* QR scans

✔ Prevent brute-force attacks

---

## 🔒 HTTPS & Headers

✔ Force HTTPS in production
✔ Redirect HTTP → HTTPS
✔ Set security headers:

* `X-Frame-Options`
* `X-Content-Type-Options`
* `Referrer-Policy`
* `Strict-Transport-Security`

---

## 🗄 Database & Backend Safety

✔ Use **fillable / guarded** properly
✔ Never trust frontend values
✔ Validate all requests with Form Requests
✔ Avoid mass assignment vulnerabilities

```php
protected $fillable = ['name', 'email'];
```

---

## 📂 File Upload Security

✔ Validate file types & sizes
✔ Rename uploaded files
✔ Store outside public directory when possible
✔ Never allow executable uploads

```php
'mimes:jpg,png,pdf',
'max:2048'
```

---

## 🧠 Token Scope & Role Control

✔ Use token abilities instead of role checks in frontend
✔ Enforce permissions in backend only

```php
Route::middleware(['auth:sanctum','abilities:billing'])->post('/checkout');
```

---

## 📊 Logging & Monitoring

✔ Log:

* Failed logins
* Token misuse
* Unauthorized access
* Suspicious API activity

❌ Never log:

* Tokens
* Passwords
* Sensitive payloads

---

## 🧹 Cleanup & Maintenance

✔ Clear unused tokens periodically
✔ Expire old sessions
✔ Rotate secrets regularly
✔ Audit API access logs

---

## 🚀 Deployment Safety

✔ `.env` NOT in Git
✔ Correct file permissions
✔ Disable directory listing
✔ Optimize app:

```bash
php artisan optimize
php artisan config:cache
php artisan route:cache
```

---
