Below is a **clean, well-organized list of essential Laravel commands**, written as **practical notes** you can directly add to your Markdown file.
These cover **project creation → daily development → database → auth → API → deployment prep**.

You can save this as a section like:
👉 `Laravel_Commands_CheatSheet.md`
or append it to your **Phase 4–9 notes**.

---

# 🧾 Laravel Essential Commands (Complete Guide)

## 🔧 Prerequisites (Run Once)

Make sure these are installed:

```bash
php -v
composer -V
node -v
npm -v
```

---

## 🚀 1. Creating a New Laravel Project

### Method 1: Using Composer (Recommended)

```bash
composer create-project laravel/laravel project-name
```

### Method 2: Using Laravel Installer

```bash
composer global require laravel/installer
laravel new project-name
```

### Move into project folder

```bash
cd project-name
```

---

## ▶ 2. Running the Laravel Project

### Start development server

```bash
php artisan serve
```

Default URL:

```
http://127.0.0.1:8000
```

---

## ⚙ 3. Environment Configuration

### Copy environment file

```bash
cp .env.example .env
```

### Generate application key

```bash
php artisan key:generate
```

---

## 🗄 4. Database Commands

### Create migration

```bash
php artisan make:migration create_users_table
```

### Run migrations

```bash
php artisan migrate
```

### Rollback migration

```bash
php artisan migrate:rollback
```

### Refresh database (drop & migrate)

```bash
php artisan migrate:refresh
```

### Seed database

```bash
php artisan db:seed
```

---

## 📦 5. Model, Controller & Resource Commands

### Create model

```bash
php artisan make:model User
```

### Model + Migration

```bash
php artisan make:model User -m
```

### Create controller

```bash
php artisan make:controller UserController
```

### Resource controller (CRUD)

```bash
php artisan make:controller UserController --resource
```

### Model + Controller + Migration

```bash
php artisan make:model Post -mcr
```

---

## 🧩 6. Blade & View Structure

(No command needed)

Views location:

```
resources/views/
```

Create files manually:

```
users/index.blade.php
```

---

## 🔐 7. Authentication Commands

### Install Laravel Breeze

```bash
composer require laravel/breeze --dev
php artisan breeze:install
npm install
npm run dev
php artisan migrate
```

### Install Jetstream

```bash
composer require laravel/jetstream
php artisan jetstream:install livewire
npm install
npm run dev
php artisan migrate
```

---

## 🔑 8. Middleware & Authorization

### Create middleware

```bash
php artisan make:middleware AdminMiddleware
```

### Register middleware

```php
// app/Http/Kernel.php
```

---

## 🌐 9. API Development Commands

### Create API controller

```bash
php artisan make:controller Api/UserController
```

### Create resource for API response

```bash
php artisan make:resource UserResource
```

---

## 🔐 10. API Authentication (Sanctum)

### Install Sanctum

```bash
composer require laravel/sanctum
php artisan vendor:publish --provider="Laravel\Sanctum\SanctumServiceProvider"
php artisan migrate
```

---

## 📁 11. File Storage Commands

### Create storage symlink

```bash
php artisan storage:link
```

---

## ✉ 12. Mail Commands

### Create mail class

```bash
php artisan make:mail NotifyDonorMail
```

---

## ⏳ 13. Queues & Jobs

### Create job

```bash
php artisan make:job SendEmailJob
```

### Run queue worker

```bash
php artisan queue:work
```

---

## 📢 14. Events & Listeners

### Create event

```bash
php artisan make:event BloodRequested
```

### Create listener

```bash
php artisan make:listener NotifyDonors
```

---

## 🧪 15. Testing Commands

### Create test

```bash
php artisan make:test UserTest
```

### Run tests

```bash
php artisan test
```

---

## ⚡ 16. Optimization & Cache

### Clear caches

```bash
php artisan optimize:clear
```

### Cache config

```bash
php artisan config:cache
```

### Cache routes

```bash
php artisan route:cache
```

---

## 🚀 17. Deployment Preparation Commands

### Optimize for production

```bash
php artisan optimize
```

### Set app environment

```env
APP_ENV=production
APP_DEBUG=false
```

---

## 🧠 Bonus: Helpful Artisan Commands

### List all commands

```bash
php artisan list
```

### Route list

```bash
php artisan route:list
```

### Tinker (Test code)

```bash
php artisan tinker
```

---

## 📌 Best Practice Tips

✔ Run migrations before seeding
✔ Use `-mcr` flags to save time
✔ Always commit after major steps
✔ Never push `.env` to GitHub

---
