# Test Rumahweb - Laravel REST API dengan JWT

REST API sederhana menggunakan Laravel 12 dan JWT Authentication.

## Requirements

- PHP >= 8.2
- Composer
- MySQL / MariaDB
- Node.js & NPM

## Instalasi

### 1. Clone Repository

```bash
git clone <repository-url>
cd test_rumahweb
```

### 2. Install Dependencies

```bash
composer install
npm install
```

### 3. Setup Environment

```bash
cp .env.example .env
php artisan key:generate
php artisan jwt:secret
```

### 4. Konfigurasi Database

Edit file `.env` dan sesuaikan konfigurasi database:

```env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=rumahweb
DB_USERNAME=root
DB_PASSWORD=
```

### 5. Jalankan Migrasi

```bash
php artisan migrate
```

### 6. (Opsional) Buat User Test

```bash
php artisan tinker
```

```php
User::create(['name'=>'Test User', 'email'=>'test@example.com', 'password'=>Hash::make('password123')]);
```

## Menjalankan Project

```bash
php artisan serve
```

Server akan berjalan di `http://localhost:8000`

## API Endpoints

| Method | Endpoint          | Deskripsi                  | Auth |
| ------ | ----------------- | -------------------------- | ---- |
| POST   | `/api/login`      | Login & dapatkan JWT token | ❌   |
| GET    | `/api/users`      | List semua users           | ✅   |
| GET    | `/api/users/{id}` | Get user by ID             | ✅   |
| POST   | `/api/users`      | Create user baru           | ✅   |
| PUT    | `/api/users/{id}` | Update user                | ✅   |
| DELETE | `/api/users/{id}` | Delete user                | ✅   |

## Contoh Penggunaan

### 1. Login (Dapatkan Token)

```bash
curl -X POST http://localhost:8000/api/login \
  -H "Content-Type: application/json" \
  -d '{"email":"test@example.com","password":"password123"}'
```

**Response:**

```json
{
    "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9...",
    "token_type": "bearer",
    "expires_in": 3600
}
```

---

### 2. GET /api/users (List Semua Users)

```bash
curl http://localhost:8000/api/users \
  -H "Authorization: Bearer <token>" \
  -H "Accept: application/json"
```

**Response:**

```json
[
    {
        "id": 1,
        "name": "Test User",
        "email": "test@example.com",
        "created_at": "2026-01-29T10:00:00.000000Z"
    }
]
```

---

### 3. GET /api/users/{id} (Get User by ID)

```bash
curl http://localhost:8000/api/users/1 \
  -H "Authorization: Bearer <token>" \
  -H "Accept: application/json"
```

**Response:**

```json
{
    "id": 1,
    "name": "Test User",
    "email": "test@example.com",
    "created_at": "2026-01-29T10:00:00.000000Z"
}
```

---

### 4. POST /api/users (Create User Baru)

```bash
curl -X POST http://localhost:8000/api/users \
  -H "Authorization: Bearer <token>" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -d '{"name":"User Baru","email":"baru@example.com","password":"password123"}'
```

**Response (201 Created):**

```json
{
    "id": 2,
    "name": "User Baru",
    "email": "baru@example.com",
    "created_at": "2026-01-29T10:30:00.000000Z"
}
```

---

### 5. PUT /api/users/{id} (Update User)

```bash
curl -X PUT http://localhost:8000/api/users/1 \
  -H "Authorization: Bearer <token>" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -d '{"name":"Nama Updated"}'
```

**Response (200 OK):**

```json
{
    "id": 1,
    "name": "Nama Updated",
    "email": "test@example.com",
    "created_at": "2026-01-29T10:00:00.000000Z"
}
```

---

### 6. DELETE /api/users/{id} (Delete User)

```bash
curl -X DELETE http://localhost:8000/api/users/1 \
  -H "Authorization: Bearer <token>" \
  -H "Accept: application/json"
```

**Response (200 OK):**

```json
{
    "message": "User deleted successfully"
}
```

**Response jika user tidak ditemukan (404):**

```json
{
    "message": "No query results for model [App\\Models\\User] 1"
}
```

## Postman Collection

Import file `Test Rumahweb.postman_collection.json` ke Postman untuk testing API.

## Tech Stack

- Laravel 12
- JWT Auth (tymon/jwt-auth)
- MySQL
