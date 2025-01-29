To create a **Laravel API** project, follow these steps:

---

## **1. Install Laravel**
Run the following command to create a new Laravel project:
```bash
composer create-project laravel/laravel my-api
```
Or if you have Laravel Installer:
```bash
laravel new my-api
```

Move into the project directory:
```bash
cd my-api
```

---

## **2. Configure `.env` File**
Open the `.env` file and set up your database:
```ini
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=my_api_db
DB_USERNAME=root
DB_PASSWORD=your_password
```
Then, run:
```bash
php artisan config:clear
```

---

## **3. Setup API Authentication**
For API authentication, you can use **Laravel Sanctum** or **Laravel Passport**.

#### **Option 1: Sanctum (Recommended)**
```bash
composer require laravel/sanctum
```
Publish the Sanctum config:
```bash
php artisan vendor:publish --provider="Laravel\Sanctum\SanctumServiceProvider"
```
Run migrations:
```bash
php artisan migrate
```
Add Sanctum's middleware in `app/Http/Kernel.php`:
```php
'api' => [
    \Laravel\Sanctum\Http\Middleware\EnsureFrontendRequestsAreStateful::class,
    'throttle:api',
    \Illuminate\Routing\Middleware\SubstituteBindings::class,
],
```
Enable token authentication in `config/auth.php`:
```php
'guards' => [
    'api' => [
        'driver' => 'sanctum',
        'provider' => 'users',
    ],
],
```

#### **Option 2: Passport (For OAuth2)**
If you prefer **Laravel Passport**, install it:
```bash
composer require laravel/passport
```
Run migrations:
```bash
php artisan migrate
php artisan passport:install
```
Add this in `app/Models/User.php`:
```php
use Laravel\Passport\HasApiTokens;

class User extends Authenticatable
{
    use HasApiTokens, Notifiable;
}
```
In `config/auth.php`, update `api` guard:
```php
'guards' => [
    'api' => [
        'driver' => 'passport',
        'provider' => 'users',
    ],
],
```

---

## **4. Create API Routes**
In `routes/api.php`, add a simple API route:
```php
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Route;

Route::get('/hello', function () {
    return response()->json(['message' => 'Hello, World!']);
});
```
Test it with:
```bash
php artisan serve
```
Then open:  
[http://127.0.0.1:8000/api/hello](http://127.0.0.1:8000/api/hello)

---

## **5. Create API Controllers**
Generate a controller:
```bash
php artisan make:controller Api/UserController
```
Modify `app/Http/Controllers/Api/UserController.php`:
```php
namespace App\Http\Controllers\Api;

use Illuminate\Http\Request;
use App\Http\Controllers\Controller;

class UserController extends Controller
{
    public function index()
    {
        return response()->json(['users' => ['User1', 'User2', 'User3']]);
    }
}
```
Define the route in `routes/api.php`:
```php
Route::get('/users', [App\Http\Controllers\Api\UserController::class, 'index']);
```
Test with:
```bash
curl -X GET http://127.0.0.1:8000/api/users
```

---

## **6. Secure API Endpoints (Optional)**
To **protect routes** using **Sanctum** authentication:
```php
Route::middleware('auth:sanctum')->get('/profile', function (Request $request) {
    return response()->json($request->user());
});
```
For **Passport**, use:
```php
Route::middleware('auth:api')->get('/profile', function (Request $request) {
    return response()->json($request->user());
});
```

---

## **7. Generate API Documentation (Optional)**
Install **Laravel Swagger (L5-Swagger)**:
```bash
composer require "darkaonline/l5-swagger"
```
Publish the config:
```bash
php artisan vendor:publish --provider "L5Swagger\L5SwaggerServiceProvider"
```
Generate API documentation:
```bash
php artisan l5-swagger:generate
```
Visit:  
[http://127.0.0.1:8000/api/documentation](http://127.0.0.1:8000/api/documentation)

---

## **8. Test APIs with Postman**
1. Open **Postman**.
2. Make a `GET` request to:
   ```
   http://127.0.0.1:8000/api/hello
   ```
3. If authentication is required, send a Bearer Token.

---

## **9. Deploy the API**
- **Use Laravel Vapor** (for AWS Serverless API)
- **Use Laravel Forge** (for DigitalOcean, Linode, AWS)
- **Deploy to a VPS (NGINX & Supervisor)**
- **Use Docker Containers**

---

## **Final Steps**
```bash
php artisan config:clear
php artisan cache:clear
php artisan migrate --seed
php artisan serve
```

🚀 Your Laravel API is now live! Let me know if you need further customization. 🔥
