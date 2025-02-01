### **Essential Requirements for Web Development**
For professional Web development, you need the right tools and configurations. Below is a comprehensive guide covering all necessary requirements:

---

## **1️⃣ Install PHP**
PHP is the core requirement for PHP development.

### **Mac (Using Homebrew)**
```bash
brew install php
```

### **Linux (Ubuntu/Debian)**
```bash
sudo apt update
sudo apt install php php-cli php-common php-mbstring php-xml php-bcmath php-tokenizer php-zip php-curl php-xmlrpc
```

### **Windows (Using WAMP/XAMPP)**
- Download **XAMPP** (which includes PHP, Apache, MySQL) from [https://www.apachefriends.org](https://www.apachefriends.org).
- Or use **WAMP** (Windows Apache MySQL PHP) from [https://www.wampserver.com](https://www.wampserver.com).

#### **Check PHP Version**
After installation, verify PHP is installed correctly:

```bash
php -v
```

---

## **2️⃣ Install a Web Server (Apache or Nginx)**
PHP requires a web server to run efficiently.

**Check if Apache is Already Installed**:

```bash
apachectl -v
```

### **Apache Installation**

- **Mac (Homebrew)**:
  ```bash
  brew install httpd
  ```
- **Linux (Ubuntu/Debian)**:
  ```bash
  sudo apt install apache2
  ```

### **Nginx Installation (Alternative Web Server)**
- **Mac (Homebrew)**:
  ```bash
  brew install nginx
  ```
- **Linux (Ubuntu/Debian)**:
  ```bash
  sudo apt install nginx
  ```

To start and enable Apache:

```bash
sudo systemctl start apache2
sudo systemctl enable apache2
```

For Nginx:

```bash
sudo systemctl start nginx
sudo systemctl enable nginx
```

---

## **Enable SSL (HTTPS)**
If you want to enable SSL on Apache, follow these steps:

### **1️⃣ Install OpenSSL**
```bash
sudo apt install openssl
```

### **2️⃣ Generate a Self-Signed SSL Certificate**
```bash
sudo openssl req -new -x509 -days 365 -nodes -out /etc/ssl/certs/apache-selfsigned.crt -keyout /etc/ssl/private/apache-selfsigned.key
```

### **3️⃣ Enable SSL Module in Apache**
```bash
sudo a2enmod ssl
```

### **4️⃣ Configure Apache to Use SSL**
Edit the SSL configuration file:
```bash
sudo nano /etc/apache2/sites-available/default-ssl.conf
```
Modify the following lines:
```
SSLCertificateFile /etc/ssl/certs/apache-selfsigned.crt
SSLCertificateKeyFile /etc/ssl/private/apache-selfsigned.key
```

### **5️⃣ Enable the SSL Site and Restart Apache**
```bash
sudo a2ensite default-ssl
sudo systemctl restart apache2
```

Now, your site should be accessible via **HTTPS** at `https://localhost`.

---

## **Configure a Custom Domain (project_name.test) in Apache**
To set up a virtual host for `http://project_name.test`, follow these steps:

### **1️⃣ Open the Apache Sites Directory**
```bash
cd /etc/apache2/sites-available/
```

### **2️⃣ Create a Virtual Host Configuration File**
```bash
sudo vim /etc/apache2/sites-available/project_name.test.conf
```

### **3️⃣ Add the Following Configuration**
```apache
<VirtualHost *:80>
    ServerAdmin webmaster@project_name.test
    ServerName project_name.test
    DocumentRoot /var/www/project_name
    <Directory /var/www/project_name>
        AllowOverride All
        Require all granted
    </Directory>
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

### **4️⃣ Enable the Site and Restart Apache**
```bash
sudo a2ensite project_name.test.conf
sudo systemctl restart apache2
```

### **5️⃣ Edit Your Hosts File**
To map `project_name.test` to your local machine, edit the hosts file:
```bash
sudo vim /etc/hosts
```
Add the following line:
```plaintext
127.0.0.1 project_name.test
```
Save and exit in Vim: Press `ESC`, then type `:wq` and press `Enter`.

### **6️⃣ Test Your Setup**
Now, open your browser and go to:
```
http://project_name.test
```
You should see your custom site loaded.

---


## **3️⃣ Install a Database Management System**
Most PHP applications require a database. You can choose between:

### **MySQL (Most Common)**

#### Install MySQL Server
- **Mac (Homebrew)**:
  ```bash
  brew install mysql
  ```
- **Linux (Ubuntu/Debian)**:
  ```bash
  sudo apt install mysql-server
  ```
- **Start MySQL**:
  ```bash
  sudo systemctl start mysql
  ```
  ```bash
  brew install mysql
  ```

#### Install MySQL Server
Install MySQL using the package manager:
```bash
sudo apt install mysql-server
```
This installs MySQL and its necessary dependencies.

#### Secure MySQL Installation
Run the security script to enhance MySQL security:
```bash
sudo mysql_secure_installation
```
Follow the prompts to set a root password, remove anonymous users, disable remote root login, and reload privileges.

#### Verify MySQL Status
Check if MySQL is running:
```bash
sudo systemctl status mysql
```
If inactive, start MySQL manually:
```bash
sudo systemctl start mysql
```

#### Log into MySQL
To access MySQL as the root user:
```bash
sudo mysql -u root -p
```
Enter the password you set earlier.

#### Create a New MySQL User (Optional)
To avoid using the root account for database management:
```sql
CREATE USER 'newuser'@'localhost' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON *.* TO 'newuser'@'localhost' WITH GRANT OPTION;
FLUSH PRIVILEGES;
EXIT;
```
Now, log in as the new user:
```bash
mysql -u newuser -p
```

#### Enable Remote Access (Optional)
To allow MySQL connections from other devices:

1️⃣ Edit MySQL Configuration:
```bash
sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf
```
Modify:
```ini
bind-address = 0.0.0.0
```

2️⃣ Allow MySQL through the firewall:
```bash
sudo ufw allow 3306
```

3️⃣ Restart MySQL to apply changes:
```bash
sudo systemctl restart mysql
```

4️⃣ Grant remote access:
```sql
GRANT ALL PRIVILEGES ON *.* TO 'newuser'@'%' IDENTIFIED BY 'password' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```
Now, MySQL can be accessed remotely.

### **PostgreSQL (Alternative)**

PostgreSQL is a powerful, open-source database system known for its performance, scalability, and strong ACID compliance. It is widely used for enterprise applications, data analytics, and high-traffic web applications.
Install PostgreSQL:
```bash
sudo apt install postgresql postgresql-contrib
```
Start PostgreSQL:
```bash
sudo systemctl start postgresql
```
To access PostgreSQL:
```bash
sudo -u postgres psql
```

#### Configure PostgreSQL
1️⃣ **Create a new PostgreSQL user** (optional, recommended for better security):
```sql
CREATE USER myuser WITH PASSWORD 'mypassword';
```
2️⃣ **Create a new database and assign the user:**
```sql
CREATE DATABASE mydatabase;
GRANT ALL PRIVILEGES ON DATABASE mydatabase TO myuser;
```
3️⃣ **Enable remote connections** (optional for external access):
- Edit the configuration file:
```bash
sudo vim /etc/postgresql/*/main/postgresql.conf
```
- Locate and update the `listen_addresses` setting:
```ini
listen_addresses = '*'
```
- Edit the authentication file:
```bash
sudo vim /etc/postgresql/*/main/pg_hba.conf
```
- Add the following line to allow remote access:
```ini
host    all             all             0.0.0.0/0               md5
```
4️⃣ **Restart PostgreSQL to apply changes:**
```bash
sudo systemctl restart postgresql
```
PostgreSQL is now configured and ready for use!

---

## **4️⃣ Install PHP Extensions (Common Requirements)**
Many PHP applications (like Laravel) require additional extensions.

Install essential PHP extensions:

```bash
sudo apt install php-mbstring php-xml php-bcmath php-tokenizer php-zip php-curl php-xmlrpc
```

Check installed extensions:

```bash
php -m
```

---

## **5️⃣ Install Composer (PHP Dependency Manager)**
Composer is essential for managing PHP packages.

### **Mac/Linux**
```bash
curl -sS https://getcomposer.org/installer | php
sudo mv composer.phar /usr/local/bin/composer
```

### **Windows**
Download from [https://getcomposer.org](https://getcomposer.org) and install.

Verify installation:

```bash
composer -v
```

---

## **6️⃣ Install Laravel (Popular PHP Framework)**
Once Composer is installed, install Laravel globally:

```bash
composer global require laravel/installer
```

Create a new Laravel project:

```bash
laravel new myproject
```

Alternatively:

```bash
composer create-project --prefer-dist laravel/laravel myproject
```

Run the project:

```bash
cd myproject
php artisan serve
```

---

## **7️⃣ Install Node.js & npm (For Frontend Development in PHP Projects)**
Some PHP projects require Node.js for asset compilation.

Install Node.js:

- **Mac (Homebrew)**:
  ```bash
  brew install node
  ```
- **Linux (Ubuntu/Debian)**:
  ```bash
  sudo apt install nodejs npm
  ```

Verify installation:

```bash
node -v
npm -v
```

---

## **8️⃣ Install PHP Code Editor / IDE**
### **Best Code Editors for PHP**
- **VS Code** – [https://code.visualstudio.com](https://code.visualstudio.com)
- **PHPStorm (Recommended for Laravel)** – [https://www.jetbrains.com/phpstorm/](https://www.jetbrains.com/phpstorm/)
- **Sublime Text** – [https://www.sublimetext.com](https://www.sublimetext.com)
- **Atom** – [https://atom.io](https://atom.io)

For **VS Code**, install PHP-related extensions:
```bash
code --install-extension bmewburn.vscode-intelephense-client
```

---

## **9️⃣ Install Redis (For Caching and Queues)**
If you're working with **Laravel or PHP applications** that need caching, install **Redis**.

### **Mac (Homebrew)**
```bash
brew install redis
```

### **Linux (Ubuntu/Debian)**
```bash
sudo apt install redis-server
```

Enable and start Redis:

```bash
sudo systemctl enable redis
sudo systemctl start redis
```

Verify Redis is running:

```bash
redis-cli ping
```
Expected output: `PONG`

---

## **🔟 Optional: Install Xdebug (For Debugging)**
For PHP debugging, install Xdebug.

- **Mac (Homebrew)**:
  ```bash
  pecl install xdebug
  ```
- **Linux (Ubuntu/Debian)**:
  ```bash
  sudo apt install php-xdebug
  ```

Then, enable it in `php.ini`:

```ini
zend_extension="xdebug.so"
xdebug.mode=debug
```

Restart Apache:

```bash
sudo systemctl restart apache2
```

---

## **4️⃣ Set Up a Laravel Project**

Setting up a Laravel project involves creating a new Laravel application, installing dependencies, configuring the environment, and ensuring necessary permissions are set for smooth operation.
```bash
Would you like to install a starter kit? ────────────────────┐
 │ › ● No starter kit                                        │
 │   ○ Laravel Breeze                                        │
 │   ○ Laravel Jetstream                                     │
 └───────────────────────────────────────────────────────────┘
 or 
 composer require laravel/breeze --dev
 php artisan breeze:install
```

### **Choosing the Right Starter Kit**

#### **No Starter Kit**
Selecting **No Starter Kit** means Laravel will be installed with only the core framework, without any pre-built authentication, UI scaffolding, or frontend presets.

- ✅ **Best for:** Developers who prefer full control over their project’s structure and frontend setup.
- 🔥 **No pre-installed authentication or UI components**—you build everything from scratch.
- 📌 **Ideal for:** Custom web applications, APIs, headless CMS setups, or projects where you want to use a different frontend framework (e.g., Next.js, Angular, or custom Blade components).
- ⚠️ **Requires manual setup** for authentication, user management, and UI if needed.

#### **Laravel Breeze** (Lightweight Authentication Starter Kit)
- ✅ **Best for:** Developers who need simple authentication with minimal setup.
- 🔥 **Includes authentication features** like login, registration, password reset, and email verification.
- 🎨 **Supports multiple frontend stacks** (Blade with Alpine.js, Livewire, React, Vue).
- 📌 **Ideal for:** Small to medium-sized projects that require authentication but don’t need advanced user roles.

#### **Laravel Jetstream** (Advanced Authentication & Features)
- ✅ **Best for:** Developers who need a feature-rich authentication system with team management.
- 🔥 **Includes:**
  - Multi-factor authentication (MFA).
  - API token authentication via Laravel Sanctum.
  - Optional **Livewire** or **Inertia.js** stack for frontend.
  - Team management (users can create/manage teams).
- 📌 **Ideal for:** Large applications needing robust authentication and user/team management features.
- ⚠️ **More complex than Breeze**—requires a deeper understanding of Laravel features.

### **Which One Should You Choose?**
| **Option** | **Best For** | **Includes** | **Frontend Support** |
|------------|-------------|-------------|----------------------|
| **No Starter Kit** | Full control, manual setup | Core Laravel only | Any frontend framework |
| **Laravel Breeze** | Simple authentication | Login, registration, email verification | Blade, Livewire, Vue, React |
| **Laravel Jetstream** | Advanced authentication, teams | MFA, API tokens, team management | Livewire or Inertia.js |

If you're building a **custom web app or API**, choose **No Starter Kit**.
If you need **simple authentication**, go with **Laravel Breeze**.
For **advanced authentication with team features**, **Laravel Jetstream** is the best choice. 

```bash
 ┌ Which Breeze stack would you like to install? ────────────┐
 │ › ● Blade with Alpine                                     │
 │   ○ Livewire (Volt Class API) with Alpine                 │
 │   ○ Livewire (Volt Functional API) with Alpine            │
 │   ○ React with Inertia                                    │
 │   ○ Vue with Inertia                                      │
```

### **Choosing the Right Breeze Stack**
- **Blade with Alpine.js** (Recommended for most Laravel apps)
  - ✅ Simple, server-side rendering with minimal JavaScript.
  - 🔥 Uses **Alpine.js** for lightweight interactivity.
  - 📌 **Ideal for:** Admin dashboards, traditional Laravel apps, content-based sites.

- **Livewire (Volt Class API) with Alpine.js**
  - ✅ Great for server-side interactivity **without JavaScript frameworks**.
  - 🔥 Livewire handles dynamic interactions with ease.
  - 📌 **Ideal for:** Admin panels, real-time dashboards, CRUD apps.

- **Livewire (Volt Functional API) with Alpine.js**
  - ✅ Provides a functional approach to Livewire components.
  - 🔥 Similar to the Class API but with a more flexible structure.
  - 📌 **Ideal for:** Large Laravel applications with modular Livewire components.

- **React with Inertia.js**
  - ✅ Full **React frontend** without needing a separate API.
  - 🔥 **Inertia.js** acts as a bridge between Laravel and React.
  - 📌 **Ideal for:** Single Page Applications (SPAs), interactive dashboards.

- **Vue with Inertia.js**
  - ✅ **Vue.js-powered frontend** with Laravel as the backend.
  - 🔥 Uses **Inertia.js** to remove the need for a REST API.
  - 📌 **Ideal for:** Vue-based apps, interactive UIs, real-time applications.

### **What Should You Choose?**
| **Option** | **Best For** | **Frontend Framework** | **Use Case** |
|------------|-------------|------------------------|--------------|
| **Blade with Alpine.js** | Simple, minimal JavaScript | Blade + Alpine.js | Traditional Laravel apps, dashboards |
| **Livewire (Volt Class API)** | Server-side interactivity | Blade + Livewire | Admin dashboards, CRUD apps |
| **Livewire (Volt Functional API)** | Functional approach to Livewire | Blade + Livewire | Large Livewire projects |
| **React with Inertia.js** | Full frontend reactivity | React | Single Page Applications (SPAs) |
| **Vue with Inertia.js** | Full frontend reactivity | Vue.js | Interactive dashboards |

```bash
 ┌ Would you like dark mode support? ───────────────────────────┐
 │ ○ Yes / ● No                                                 │
 └──────────────────────────────────────────────────────────────┘
```

Dark mode support enhances user experience by reducing eye strain and improving accessibility, especially in low-light environments. Choose **Yes** if your application should include a dark mode option, or **No** if you prefer a standard light mode only.

```bash
 ┌ Which testing framework do you prefer? ──────────────────────┐
 │ › ● Pest                                                     │
 │   ○ PHPUnit                                                  │
 └──────────────────────────────────────────────────────────────┘
```

### **Choosing the Right Testing Framework**
- **Pest** (Recommended for most Laravel applications)
  - ✅ Simpler syntax with a focus on developer experience.
  - 🔥 Uses a clean, expressive API.
  - 📌 **Ideal for:** Writing modern, concise tests quickly.

- **PHPUnit**
  - ✅ The default testing framework included with Laravel.
  - 🔥 More traditional, structured test cases.
  - 📌 **Ideal for:** Large projects that require structured unit and feature testing.

```bash
 ┌ Which database will your application use? ───────────────────┐
 │ › ● SQLite                                                   │
 │   ○ MySQL                                                    │
 │   ○ MariaDB                                                  │
 │   ○ PostgreSQL                                               │
 │   ○ SQL Server (Missing PDO extension)                       │
 └──────────────────────────────────────────────────────────────┘
```

### **Choosing the Right Database**
- **SQLite** (Recommended for local development and small projects)
  - ✅ Lightweight and serverless database.
  - 🔥 Simple file-based storage with zero configuration.
  - 📌 **Ideal for:** Prototyping, testing, small applications.

- **MySQL** (Most common for production applications)
  - ✅ High-performance, widely used database.
  - 🔥 Supports replication, clustering, and high availability.
  - 📌 **Ideal for:** Web applications, e-commerce platforms, and enterprise solutions.

- **MariaDB** (A MySQL fork with enhancements)
  - ✅ Drop-in replacement for MySQL with better performance.
  - 🔥 Open-source and supports additional features like JSON functions.
  - 📌 **Ideal for:** High-traffic websites, modern applications.

- **PostgreSQL** (Best for data integrity and complex queries)
  - ✅ Advanced database with strong ACID compliance.
  - 🔥 Offers full-text search, JSON support, and powerful indexing.
  - 📌 **Ideal for:** Data-heavy applications, analytics, and business intelligence.

- **SQL Server** (For Microsoft environments)
  - ✅ Best choice for applications running on Microsoft ecosystems.
  - 🔥 Supports stored procedures, triggers, and advanced security.
  - 📌 **Ideal for:** Enterprise-level applications and Windows-based solutions.

```bash
 ┌ Default database updated. Would you like to run the default database migrations? ────────────┐
 │ ● Yes / ○ No                                                                                 │
 └──────────────────────────────────────────────────────────────────────────────────────────────┘
```

Choosing **Yes** will execute Laravel's built-in migrations, setting up the structure required for user authentication, roles, and other features. 
If you prefer to manually manage database schema changes, select **No** and run migrations later using:

```bash
php artisan migrate
```

#### Install Laravel and Create a New Project
```bash
laravel new project_name
cd project_name
```

#### Install Frontend Dependencies and Build Assets
```bash
npm install && npm run build
```

#### Open the Project in VS Code
```bash
code .
```

#### Configure Environment File
Update `.env` file with database details:
```bash
config .env
```

#### Create a Database
Log into MySQL and create a new database:
```sql
CREATE DATABASE db_project_name;
```

#### Running Laravel Project
```bash
composer run dev
```

#### Set Proper Permissions for Storage and Cache
```bash
chmod -R 775 storage bootstrap/cache
chmod -R 775 storage/logs
```

---

### **Step 1: Generate the Seeder File**
Run the following Artisan command to generate the `UserSeeder.php` file:

```bash
php artisan make:seeder UserSeeder
```

This will create a new seeder file in the `database/seeders/` directory.

---

### **Step 2: Edit the `UserSeeder.php` File**
Open the generated `database/seeders/UserSeeder.php` file and **replace its content** with the optimized version:

```php
<?php

namespace Database\Seeders;

use Illuminate\Database\Seeder;
use App\Models\User;
use Illuminate\Support\Facades\Hash;
use Spatie\Permission\Models\Role;

class UserSeeder extends Seeder
{
    /**
     * Run the database seeds.
     */
    public function run(): void
    {
        // Define roles
        $roles = ['admin', 'manager', 'seller', 'customer'];

        // Ensure roles exist in the database
        foreach ($roles as $role) {
            Role::firstOrCreate(['name' => $role]); // Create role if it doesn't exist
        }

        // Create users and assign respective roles
        foreach ($roles as $role) {
            $user = User::updateOrCreate(
                ['email' => "{$role}@gmail.com"], // Ensure uniqueness by email
                [
                    'name' => ucfirst($role),
                    'password' => Hash::make('password'),
                ]
            );

            // Assign role to the user
            $user->assignRole($role);
        }
    }
}
```

---

### **Step 3: Run the Seeder**
After modifying the seeder, **run the seeder to populate the database**.

#### **Option 1: Run the Specific Seeder**
```bash
php artisan db:seed --class=UserSeeder
```

#### **Option 2: Run All Seeders (if added to DatabaseSeeder.php)**
If you want to run all seeders, first **add the `UserSeeder` to `DatabaseSeeder.php`**:

Open `database/seeders/DatabaseSeeder.php` and update the `run()` method:

```php
public function run(): void
{
    $this->call([
        UserSeeder::class, // Calls the UserSeeder
    ]);
}
```

Then, run **all seeders** using:
```bash
php artisan db:seed
```

---

### **Step 4: Verify the Seeded Data**
Check if the users were created with the assigned roles:

#### **1. Open Laravel Tinker**
```bash
php artisan tinker
```

#### **2. Fetch Users**
Run the following in Tinker to check users:
```php
User::all();
```

#### **3. Check a User’s Role**
To verify role assignments:
```php
$user = User::where('email', 'admin@gmail.com')->first();
$user->getRoleNames();
```

---

## **Step-by-Step Guide: Adding Authentication to Your Laravel Project**

Laravel provides multiple authentication solutions, such as **Laravel Breeze**, **Laravel Jetstream**, and **Laravel UI**. For most projects, **Laravel Breeze** is the best choice for simplicity and flexibility.

---

## **Step 1: Install Laravel Breeze**
Laravel Breeze provides a simple authentication scaffold including login, registration, password reset, and email verification.

Run the following command in your Laravel project root directory:

```bash
composer require laravel/breeze --dev
```

Then, install Breeze:

```bash
php artisan breeze:install
```

---

## **Step 2: Choose Your Authentication Stack**
After running `breeze:install`, you’ll be prompted to select a frontend stack:
For **most projects**, **Blade with Alpine.js** is the best option unless you need a **fully reactive frontend**.

## **Step 3: Run Database Migrations**
After selecting your stack, run the database migrations to create the necessary tables:

```bash
php artisan migrate
```

---

## **Step 4: Install Frontend Dependencies**
If you're using Blade (default stack), install and build frontend assets:

```bash
npm install && npm run dev
```

---

## **Step 5: Verify Authentication Routes**
Breeze automatically sets up authentication routes in `routes/web.php`:

```php
Route::get('/dashboard', function () {
    return view('dashboard');
})->middleware(['auth'])->name('dashboard');

require __DIR__.'/auth.php';
```

To check available routes, run:

```bash
php artisan route:list
```

---

## **Step 6: Test Authentication**
Now, start your Laravel server:

```bash
php artisan serve
```

Visit:

- **`/register`** → To create a new user.
- **`/login`** → To log in.
- **`/dashboard`** → Protected route (redirects to login if not authenticated).

---

## **Step 7: Protect Routes**
Use Laravel's built-in `auth` middleware to restrict access.

### **Example: Protect a Route**
```php
Route::get('/admin', function () {
    return view('admin.dashboard');
})->middleware('auth');
```

For **admin-only access**, create a middleware:

```bash
php artisan make:middleware AdminMiddleware
```

Modify `app/Http/Middleware/AdminMiddleware.php`:

```php
public function handle(Request $request, Closure $next)
{
    if (auth()->user() && auth()->user()->role !== 'admin') {
        abort(403, 'Unauthorized');
    }
    return $next($request);
}
```

Register it in `app/Http/Kernel.php`:

```php
protected $routeMiddleware = [
    'admin' => \App\Http\Middleware\AdminMiddleware::class,
];
```

Then, apply it to admin routes:

```php
Route::middleware(['auth', 'admin'])->group(function () {
    Route::get('/admin', [AdminController::class, 'index']);
});
```

---

## **Step 8: Email Verification (Optional)**
Enable email verification in `User.php`:

```php
use Illuminate\Contracts\Auth\MustVerifyEmail;

class User extends Authenticatable implements MustVerifyEmail
```

Update `routes/web.php`:

```php
Auth::routes(['verify' => true]);
```

---

## **Step 9: Customizing Authentication Views**
Breeze stores authentication views in:

```
resources/views/auth/
```

Modify files like:

- `login.blade.php`
- `register.blade.php`
- `dashboard.blade.php`

---

## **Step 10: Logout**
To log out users, use:

```php
Route::post('/logout', function () {
    Auth::logout();
    return redirect('/');
})->name('logout');
```

Or in Blade:

```blade
<form action="{{ route('logout') }}" method="POST">
    @csrf
    <button type="submit">Logout</button>
</form>
```

---

## **Step 1: Install Spatie Laravel Permission Package**

Spatie provides a robust and well-maintained package for handling roles and permissions. To install it, run:

```bash
composer require spatie/laravel-permission
```

Then, publish the package’s configuration files:

```bash
php artisan vendor:publish --provider="Spatie\Permission\PermissionServiceProvider"
```

This command will create a config file at:

```
config/permission.php
```

---

## **Step 2: Run Migrations**

Spatie provides default migrations to create the necessary database tables.

Run the migrations:

```bash
php artisan migrate
```

This will create the following tables:

- `roles` → Stores role names (e.g., Admin, Editor, User)
- `permissions` → Stores permission names (e.g., manage users, create posts)
- `model_has_roles` → Associates roles with users
- `model_has_permissions` → Associates permissions directly with users
- `role_has_permissions` → Assigns permissions to roles

---

## **Step 3: Modify the User Model**

Open `app/Models/User.php` and add the **HasRoles** trait:

```php
use Spatie\Permission\Traits\HasRoles;

class User extends Authenticatable
{
    use HasRoles;
}
```

This enables role and permission handling in the `User` model.

---

## **Step 4: Creating Roles and Permissions via Seeder**

We’ll use Laravel seeders to **pre-define roles and permissions**.

### **4.1 Create a Seeder for Roles**
Generate the `RoleSeeder`:

```bash
php artisan make:seeder RoleSeeder
```

Now, open `database/seeders/RoleSeeder.php` and define roles and permissions:

```php
use Illuminate\Database\Seeder;
use Spatie\Permission\Models\Role;
use Spatie\Permission\Models\Permission;

class RoleSeeder extends Seeder
{
    public function run()
    {
        // Define Roles
        $admin = Role::create(['name' => 'admin']);
        $editor = Role::create(['name' => 'editor']);
        $user = Role::create(['name' => 'user']);

        // Define Permissions
        $permissions = [
            'manage users',
            'edit articles',
            'delete articles',
            'publish articles',
            'view dashboard',
        ];

        foreach ($permissions as $permission) {
            Permission::create(['name' => $permission]);
        }

        // Assign Permissions to Admin
        $admin->givePermissionTo(Permission::all());

        // Assign Specific Permissions to Editor
        $editor->givePermissionTo(['edit articles', 'publish articles']);

        // User role doesn't need specific permissions
    }
}
```

Run the seeder:

```bash
php artisan db:seed --class=RoleSeeder
```

---

### **4.2 Assign Roles to Users via Seeder**
Generate a `UserRoleSeeder`:

```bash
php artisan make:seeder UserRoleSeeder
```

Now, open `database/seeders/UserRoleSeeder.php` and assign roles to existing users:

```php
use Illuminate\Database\Seeder;
use App\Models\User;
use Spatie\Permission\Models\Role;

class UserRoleSeeder extends Seeder
{
    public function run()
    {
        // Assign Admin Role
        $adminUser = User::where('email', 'admin@example.com')->first();
        if ($adminUser) {
            $adminUser->assignRole('admin');
        }

        // Assign Editor Role
        $editorUser = User::where('email', 'editor@example.com')->first();
        if ($editorUser) {
            $editorUser->assignRole('editor');
        }

        // Assign User Role
        $normalUser = User::where('email', 'user@example.com')->first();
        if ($normalUser) {
            $normalUser->assignRole('user');
        }
    }
}
```

Run the seeder:

```bash
php artisan db:seed --class=UserRoleSeeder
```

---

## **Step 5: Implement Role-Based Access Control (RBAC)**

### **5.1 Middleware for Role-Based Access**
To restrict routes based on roles, create a middleware:

```bash
php artisan make:middleware RoleMiddleware
```

Open `app/Http/Middleware/RoleMiddleware.php` and modify it:

```php
use Closure;
use Illuminate\Http\Request;

class RoleMiddleware
{
    public function handle(Request $request, Closure $next, $role)
    {
        if (!auth()->check() || !auth()->user()->hasRole($role)) {
            abort(403, 'Unauthorized.');
        }

        return $next($request);
    }
}
```

Register the middleware in `app/Http/Kernel.php`:

```php
protected $routeMiddleware = [
    'role' => \App\Http\Middleware\RoleMiddleware::class,
];
```

Now, apply it to routes:

```php
Route::middleware(['auth', 'role:admin'])->group(function () {
    Route::get('/admin', [AdminController::class, 'index']);
});
```

---

### **5.2 Check Roles & Permissions in Blade Views**
Use these Blade directives to show/hide UI elements:

```blade
@role('admin')
    <p>Welcome, Admin!</p>
@endrole

@hasanyrole('admin|editor')
    <p>Access granted for Admin or Editor</p>
@endhasanyrole

@can('publish articles')
    <p>You have permission to publish articles.</p>
@endcan
```

---

## **Step 6: Managing Roles & Permissions via Controller**

To programmatically assign roles and permissions:

```php
use App\Models\User;
use Spatie\Permission\Models\Role;
use Spatie\Permission\Models\Permission;

// Assign Role to User
$user = User::find(1);
$user->assignRole('admin');

// Remove Role
$user->removeRole('admin');

// Check if User has Role
if ($user->hasRole('admin')) {
    // Do something
}

// Assign Permission
$user->givePermissionTo('edit articles');

// Check if User has Permission
if ($user->can('edit articles')) {
    // Do something
}
```

---

## **Step 7: Testing Roles and Permissions**
To verify everything works:

1. Run **migrations & seeders**:
   ```bash
   php artisan migrate --seed
   ```

2. Log in with **admin@example.com** and check if they have admin access.

3. Try accessing a restricted page (`/admin`) with an unauthorized user to confirm the middleware works.

---

## **Step 8: Create a Role & Permission Management Panel**

Now that we have roles and permissions set up in Laravel, the next step is to create a **Role & Permission Management Panel** where an admin can manage **users, roles, and permissions dynamically** via a UI.

---

### **Step 8.1: Create Routes for Role Management**
We need to define routes to manage roles and permissions. Open `routes/web.php` and add:

```php
use App\Http\Controllers\Backend\RoleController;

Route::middleware(['auth', 'role:admin'])->prefix('admin')->group(function () {
    Route::resource('/roles', RoleController::class);
});
```

This ensures that only **admins** can access role management routes.

---

### **Step 8.2: Create a Role Management Controller**
Run the following command to generate the RoleController:

```bash
php artisan make:controller Backend/RoleController --resource
```

Now, open `app/Http/Controllers/Backend/RoleController.php` and modify it:

```php
namespace App\Http\Controllers\Backend;

use App\Http\Controllers\Controller;
use Illuminate\Http\Request;
use Spatie\Permission\Models\Role;
use Spatie\Permission\Models\Permission;

class RoleController extends Controller
{
    public function index()
    {
        $roles = Role::with('permissions')->get();
        return view('backend.roles.index', compact('roles'));
    }

    public function create()
    {
        $permissions = Permission::all();
        return view('backend.roles.create', compact('permissions'));
    }

    public function store(Request $request)
    {
        $request->validate([
            'name' => 'required|unique:roles,name',
            'permissions' => 'array',
        ]);

        $role = Role::create(['name' => $request->name]);
        $role->syncPermissions($request->permissions);

        return redirect()->route('roles.index')->with('success', 'Role created successfully.');
    }

    public function edit(Role $role)
    {
        $permissions = Permission::all();
        return view('backend.roles.edit', compact('role', 'permissions'));
    }

    public function update(Request $request, Role $role)
    {
        $request->validate([
            'name' => 'required|unique:roles,name,' . $role->id,
            'permissions' => 'array',
        ]);

        $role->update(['name' => $request->name]);
        $role->syncPermissions($request->permissions);

        return redirect()->route('roles.index')->with('success', 'Role updated successfully.');
    }

    public function destroy(Role $role)
    {
        $role->delete();
        return redirect()->route('roles.index')->with('success', 'Role deleted successfully.');
    }
}
```

---

### **Step 8.3: Create Views for Role Management**
We now create views inside `resources/views/backend/roles/` for managing roles.

#### **1️⃣ Role List Page (`index.blade.php`)**
Create `resources/views/backend/roles/index.blade.php`:

```blade
<x-admin-layout>
    <div class="container">
        <h1>Manage Roles</h1>
        <a href="{{ route('roles.create') }}" class="btn btn-primary">Create Role</a>

        <table class="table mt-4">
            <thead>
                <tr>
                    <th>Name</th>
                    <th>Permissions</th>
                    <th>Actions</th>
                </tr>
            </thead>
            <tbody>
                @foreach($roles as $role)
                    <tr>
                        <td>{{ $role->name }}</td>
                        <td>{{ implode(', ', $role->permissions->pluck('name')->toArray()) }}</td>
                        <td>
                            <a href="{{ route('roles.edit', $role) }}" class="btn btn-warning">Edit</a>
                            <form action="{{ route('roles.destroy', $role) }}" method="POST" style="display:inline-block;">
                                @csrf @method('DELETE')
                                <button type="submit" class="btn btn-danger">Delete</button>
                            </form>
                        </td>
                    </tr>
                @endforeach
            </tbody>
        </table>
    </div>
</x-admin-layout>
```

---

#### **2️⃣ Create Role Page (`create.blade.php`)**
Create `resources/views/backend/roles/create.blade.php`:

```blade
<x-admin-layout>
    <div class="container">
        <h1>Create Role</h1>

        <form action="{{ route('roles.store') }}" method="POST">
            @csrf
            <div class="mb-3">
                <label for="name">Role Name</label>
                <input type="text" name="name" class="form-control" required>
            </div>

            <div class="mb-3">
                <label for="permissions">Assign Permissions:</label><br>
                @foreach($permissions as $permission)
                    <input type="checkbox" name="permissions[]" value="{{ $permission->name }}"> {{ $permission->name }} <br>
                @endforeach
            </div>

            <button type="submit" class="btn btn-success">Create Role</button>
        </form>
    </div>
</x-admin-layout>
```

---

#### **3️⃣ Edit Role Page (`edit.blade.php`)**
Create `resources/views/backend/roles/edit.blade.php`:

```blade
<x-admin-layout>
    <div class="container">
        <h1>Edit Role</h1>

        <form action="{{ route('roles.update', $role) }}" method="POST">
            @csrf @method('PUT')

            <div class="mb-3">
                <label for="name">Role Name</label>
                <input type="text" name="name" class="form-control" value="{{ $role->name }}" required>
            </div>

            <div class="mb-3">
                <label for="permissions">Assign Permissions:</label><br>
                @foreach($permissions as $permission)
                    <input type="checkbox" name="permissions[]" value="{{ $permission->name }}" 
                        {{ $role->permissions->contains($permission) ? 'checked' : '' }}> {{ $permission->name }} <br>
                @endforeach
            </div>

            <button type="submit" class="btn btn-primary">Update Role</button>
        </form>
    </div>
</x-admin-layout>
```

---

### **Step 8.4: Role Management UI in Sidebar**
To allow admin users to navigate the Role Management Panel, add a link to the admin sidebar:

Modify `resources/views/backend/layouts/sidebar.blade.php`:

```blade
<ul class="nav flex-column">
    <li class="nav-item">
        <a class="nav-link" href="{{ route('roles.index') }}">Manage Roles</a>
    </li>
</ul>
```

---

## **Step 9: Testing Role Management**
### ✅ **Run the Laravel Server**
```bash
php artisan serve
```

### ✅ **Check if Role Management Works**
1. Log in as an **admin**.
2. Navigate to `http://localhost:8000/admin/roles`.
3. **Create new roles** and **assign permissions**.
4. **Edit or delete existing roles**.
5. **Ensure roles are assigned correctly in the database**.

---

## **Step 10: Secure Pages Based on Roles**
Once role management is working, we **restrict access** to certain pages:

### **Restricting Routes**
Modify `routes/web.php`:

```php
Route::middleware(['auth', 'role:admin'])->group(function () {
    Route::get('/admin/dashboard', [AdminController::class, 'index']);
});
```

### **Restricting Views**
Inside any **Blade template**, you can now use:

```blade
@role('admin')
    <a href="/admin/dashboard">Go to Admin Dashboard</a>
@endrole

@can('manage users')
    <button>Delete User</button>
@endcan
```

---

### **For Backend (Admin Panel)**
This command generates a model, migration, and a **fully functional resourceful controller** for managing products in the admin panel.

```bash
php artisan make:model Backend/Product -mcr
```
- **Generates:**
  - `app/Models/Backend/Product.php`
  - `app/Http/Controllers/Backend/ProductController.php`
  - `database/migrations/xxxx_xx_xx_xxxxxx_create_products_table.php`
- **Controller Methods Included:** `index()`, `create()`, `store()`, `show()`, `edit()`, `update()`, `destroy()`

---

### **For Frontend (User-Facing)**
Since the **frontend** only needs to display products (`index` & `show`), we generate a model and a controller with limited methods:

```bash
php artisan make:model Frontend/Product -m
php artisan make:controller Frontend/ProductController --resource --only=index,show
```
- **Generates:**
  - `app/Models/Frontend/Product.php`
  - `app/Http/Controllers/Frontend/ProductController.php`
- **Controller Methods Included:** `index()`, `show()`

---

### **To Run Migrations**
```bash
php artisan migrate
```

---

## **Step 1: Create Layout Components**

If **AppLayout** is your **default layout**, then it should act as the **base layout** for both **FrontendLayout** and **BackendLayout**. In this structure:

1. **AppLayout** → The main layout that includes the common structure (e.g., head, scripts, footer).
2. **BackendLayout** → Extends **AppLayout** and includes backend-specific features (e.g., sidebar, dashboard layout).
3. **FrontendLayout** → Extends **AppLayout** and includes frontend-specific features (e.g., navbar, main content).

Run the following command to generate the base application layout:

```bash
php artisan make:component AppLayout # Base layout for shared components (used as a foundation for both Backend and Frontend layouts)
```

Then, generate separate layouts for backend and frontend sections:

```bash
php artisan make:component BackendLayout # Backend - Admin Dashboard layout
php artisan make:component FrontendLayout  # Frontend layout
```

These commands will generate Blade component files in `resources/views/components/` and their corresponding class files in `app/View/Components/`.

## **Step 2: Structure of the Layout Components**

Each generated component consists of:

1. **Blade Template:** Located in `resources/views/components/`
2. **Class File:** Located in `app/View/Components/`

### **How to Structure It**
You should modify **BackendLayout** and **FrontendLayout** to extend **AppLayout** like this:

---

### **1. Default Application Layout (`app-layout.blade.php`)**
This will act as the **base template** for all views.

```blade
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{{ $title ?? 'My Application' }}</title>
    @vite(['resources/css/app.css', 'resources/js/app.js'])
</head>
<body>
    <header>
        @include('partials.navbar')
    </header>
    
    <main>
        {{ $slot }}
    </main>

    <footer>
        @include('partials.footer')
    </footer>
</body>
</html>
```

---

### **2. Backend Layout (`backend-layout.blade.php`)**
Now, **BackendLayout** extends **AppLayout** and adds backend-specific components.

```blade
<x-app-layout>
    @include('backend.sidebar')

    <div class="backend-content">
        {{ $slot }}
    </div>
</x-app-layout>
```

---

### **3. Frontend Layout (`frontend-layout.blade.php`)**
Similarly, **FrontendLayout** extends **AppLayout** and includes frontend-specific components.

```blade
<x-app-layout>
    @include('frontend.navbar')

    <main>
        {{ $slot }}
    </main>
</x-app-layout>
```

---

### **Why This Approach?**
✅ **AppLayout remains the foundation** – ensuring a **consistent structure** across both frontend and backend.  
✅ **Backend and Frontend Layouts only add specific features** – keeping everything modular and **easy to maintain**.  
✅ **Less redundancy** – no need to duplicate shared styles, scripts, or layouts.

### Example Directory Structure (Optimized):

```
app/
  ├── View/
  │   ├── Components/
  │   │   ├── AppLayout.php
  │   │   ├── BackendLayout.php
  │   │   ├── FrontendLayout.php
resources/
  ├── views/
  │   ├── components/
  │   │   ├── app-layout.blade.php
  │   │   ├── backend-layout.blade.php
  │   │   ├── frontend-layout.blade.php
  │   ├── layouts/
  │   │   ├── app-layout.blade.php
  │   │   ├── backend-layout.blade.php
  │   │   ├── frontend-layout.blade.php
```

---

## **Step 3: Using Layout Components**

#### **1. Using the Base Application Layout (`app-layout`)**

```blade
<x-app-layout>
    <h1>Welcome to Laravel</h1>
</x-app-layout>
```

#### **2. Using the Backend Layout (`backend-layout`)**

```blade
<x-backend-layout>
    <h1>Admin Dashboard</h1>
</x-backend-layout>
```

#### **3. Using the Frontend Layout (`frontend-layout`)**

```blade
<x-frontend-layout>
    <h1>User Profile</h1>
</x-frontend-layout>
```

---

Here’s a professional guide on creating a **Laravel Artisan Command** to generate **Blade view files** and classify them for **backend** and **frontend**.

---

# **Laravel Custom Command for Creating Views**

## **Introduction**

When working on large Laravel projects, manually creating view files and structuring them into frontend and backend folders can be time-consuming. To streamline this process, we will create a custom Artisan command called `MakeView` to generate Blade view files and organize them based on their usage.

---

## **Step 1: Create the Custom Artisan Command**

Run the following command to generate a new Artisan command:

```bash
php artisan make:command MakeView
```

This will create a command file located at:

```
app/Console/Commands/MakeView.php
```

---

## **Step 2: Define the Command Logic**

Open `app/Console/Commands/MakeView.php` and update it as follows:

```php
<?php

namespace App\Console\Commands;

use Illuminate\Console\Command;
use Illuminate\Support\Facades\File;

class MakeView extends Command
{
    protected $signature = 'make:view {name} {--backend} {--frontend}';
    protected $description = 'Generate a Blade view file and classify it into frontend or backend';

    public function handle()
    {
        $name = $this->argument('name');
        $backend = $this->option('backend');
        $frontend = $this->option('frontend');

        // Define base path
        $basePath = resource_path('views');

        // Determine the directory and content based on classification
        if ($backend) {
            $path = $basePath . "/backend/{$name}.blade.php";
            $content = <<<BLADE
        <!-- Backend View: {$name} -->

        <x-admin-layout>
            <h1>{$name}</h1>
        </x-admin-layout>
        BLADE;
        } elseif ($frontend) {
            $path = $basePath . "/frontend/{$name}.blade.php";
            $content = <<<BLADE
        <!-- Frontend View: {$name} -->

        <x-user-layout>
            <h1>{$name}</h1>
        </x-user-layout>
        BLADE;
        } else {
            $this->error('You must specify --backend or --frontend.');
            return;
        }

        // Check if the file already exists
        if (File::exists($path)) {
            $this->error("The view file {$name}.blade.php already exists!");
            return;
        }

        // Ensure the directory exists
        File::ensureDirectoryExists(dirname($path));

        // Create the view file with the appropriate template
        File::put($path, $content);

        $this->info("View file created: {$path}");
    }
}
```

---

## **Step 3: Register the Command**

Open `app/Console/Kernel.php` and register the new command:

```php
protected $commands = [
    \App\Console\Commands\MakeView::class,
];
```

Run the following command to refresh Artisan’s cache:

```bash
php artisan cache:clear
```

---

## **Step 4: Using the MakeView Command**

Now that the command is set up, you can use it to generate Blade view files in the **frontend** or **backend** directories.

### **Creating a Frontend View**
To create a view file inside `resources/views/frontend/`:
```bash
php artisan make:view products --frontend
```
This will generate:
```
resources/views/frontend/products.blade.php
```

### **Creating a Backend View**
To create a view file inside `resources/views/backend/`:
```bash
php artisan make:view dashboard --backend
```
This will generate:
```
resources/views/backend/dashboard.blade.php
```

---

### **📌 Full Module Command:**
```bash
php artisan make:full-module Product
```

**✨ This command will generate:**
- **Backend (`app/Http/Controllers/Backend/ProductController.php`)**: Full CRUD (index, show, create, store, edit, update, destroy).
- **Frontend (`app/Http/Controllers/Frontend/ProductController.php`)**: Only `index` and `show` methods.
- **Frontend Views (`resources/views/frontend/products/`)**: `index.blade.php` & `show.blade.php`.

---

### **📄 Full Updated Implementation**
```php
<?php

namespace App\Console\Commands;

use Illuminate\Console\Command;
use Illuminate\Support\Facades\File;

class MakeFullModule extends Command
{
    protected $signature = 'make:full-module {name}';
    protected $description = 'Create a model, migration, factory, seeder, backend resource controller, frontend controller, and views';

    public function handle()
    {
        $name = ucfirst($this->argument('name'));

        // Define paths
        $modelPath = "Product";
        $backendControllerPath = "Backend/{$name}Controller";
        $frontendControllerPath = "Frontend/{$name}Controller";
        $viewBasePath = resource_path("views/frontend/{$name}");

        // Step 1: Generate Model, Migration, Factory, and Seeder
        $this->call('make:model', [
            'name' => $modelPath,
            '-mfs' => true, // Includes Migration, Factory, and Seeder
        ]);

        // Step 2: Generate Backend Resource Controller (Full CRUD)
        $this->call('make:controller', [
            'name' => $backendControllerPath,
            '--resource' => true, // Generates full CRUD
        ]);

        // Step 3: Generate Frontend Controller (Only index and show methods)
        $frontendControllerContent = <<<PHP
        <?php

        namespace App\Http\Controllers\Frontend;

        use App\Http\Controllers\Controller;
        use App\Models\\{$name};
        use Illuminate\Http\Request;

        class {$name}Controller extends Controller
        {
            /**
             * Display a listing of the resource.
             */
            public function index()
            {
                \$items = {$name}::all();
                return view('frontend.{$name}.index', compact('items'));
            }

            /**
             * Display the specified resource.
             */
            public function show(\$id)
            {
                \$item = {$name}::findOrFail(\$id);
                return view('frontend.{$name}.show', compact('item'));
            }
        }
        PHP;

        $frontendControllerPath = app_path("Http/Controllers/Frontend/{$name}Controller.php");
        File::ensureDirectoryExists(dirname($frontendControllerPath));
        File::put($frontendControllerPath, $frontendControllerContent);
        $this->info("Frontend controller created: {$frontendControllerPath}");

        // Step 4: Generate Backend Resource Controller (Full CRUD Methods)
        $backendControllerContent = <<<PHP
        <?php

        namespace App\Http\Controllers\Backend;

        use App\Http\Controllers\Controller;
        use App\Models\\{$name};
        use Illuminate\Http\Request;

        class {$name}Controller extends Controller
        {
            /**
             * Display a listing of the resource.
             */
            public function index()
            {
                \$items = {$name}::all();
                return view('backend.{$name}.index', compact('items'));
            }

            /**
             * Show the form for creating a new resource.
             */
            public function create()
            {
                return view('backend.{$name}.create');
            }

            /**
             * Store a newly created resource in storage.
             */
            public function store(Request \$request)
            {
                \$validated = \$request->validate([
                    'name' => 'required|string|max:255',
                    'description' => 'nullable|string',
                ]);

                {$name}::create(\$validated);
                return redirect()->route('backend.{$name}.index')->with('success', '{$name} created successfully.');
            }

            /**
             * Display the specified resource.
             */
            public function show(\$id)
            {
                \$item = {$name}::findOrFail(\$id);
                return view('backend.{$name}.show', compact('item'));
            }

            /**
             * Show the form for editing the specified resource.
             */
            public function edit(\$id)
            {
                \$item = {$name}::findOrFail(\$id);
                return view('backend.{$name}.edit', compact('item'));
            }

            /**
             * Update the specified resource in storage.
             */
            public function update(Request \$request, \$id)
            {
                \$validated = \$request->validate([
                    'name' => 'required|string|max:255',
                    'description' => 'nullable|string',
                ]);

                \$item = {$name}::findOrFail(\$id);
                \$item->update(\$validated);
                return redirect()->route('backend.{$name}.index')->with('success', '{$name} updated successfully.');
            }

            /**
             * Remove the specified resource from storage.
             */
            public function destroy(\$id)
            {
                {$name}::findOrFail(\$id)->delete();
                return redirect()->route('backend.{$name}.index')->with('success', '{$name} deleted successfully.');
            }
        }
        PHP;

        $backendControllerPath = app_path("Http/Controllers/Backend/{$name}Controller.php");
        File::put($backendControllerPath, $backendControllerContent);
        $this->info("Backend controller created: {$backendControllerPath}");

        // Step 5: Generate Frontend Blade Views (index and show)
        File::ensureDirectoryExists($viewBasePath);

        $indexViewContent = <<<BLADE
        <!-- Frontend View: {$name} Index -->
        <x-user-layout>
            <h1>{$name} List</h1>
            <ul>
                @foreach(\$items as \$item)
                    <li><a href="{{ route('frontend.{$name}.show', \$item->id) }}">{{ \$item->name }}</a></li>
                @endforeach
            </ul>
        </x-user-layout>
        BLADE;

        $showViewContent = <<<BLADE
        <!-- Frontend View: {$name} Show -->
        <x-user-layout>
            <h1>{{ \$item->name }}</h1>
            <p>Description: {{ \$item->description }}</p>
        </x-user-layout>
        BLADE;

        File::put("{$viewBasePath}/index.blade.php", $indexViewContent);
        File::put("{$viewBasePath}/show.blade.php", $showViewContent);
        $this->info("Frontend views created in: {$viewBasePath}");

        // Success message
        $this->info("✅ Successfully created full module for: {$name}");
        $this->info("- Model, Migration, Factory, Seeder ✅");
        $this->info("- Backend Resource Controller ✅");
        $this->info("- Frontend Controller (index, show) ✅");
        $this->info("- Frontend Views (index, show) ✅");

        $this->info("\nNext Steps:");
        $this->info("1️⃣ Run migrations: php artisan migrate");
        $this->info("2️⃣ Seed data: php artisan db:seed --class={$name}Seeder");
        $this->info("3️⃣ Add routes in web.php and admin.php");
    }
}
```

---

### **🚀 How to Use This Command**
```bash
php artisan make:full-module Product
```

---

### **📌 Next Steps**
After running the command, **run migrations** and **seed data**:
```bash
php artisan migrate
php artisan db:seed --class=ProductSeeder
```

---

## **Step 1: Advanced Eloquent ORM**

Laravel’s **Eloquent ORM** simplifies database interactions and provides a clean way to work with your models.

### **1️⃣ Relationships**

Relationships are the backbone of Eloquent, allowing models to relate to one another.

#### **a. One-to-One**

Example: A **User** has one **Profile**.

```php
// User.php
public function profile() {
    return $this->hasOne(Profile::class);
}

// Profile.php
public function user() {
    return $this->belongsTo(User::class);
}

// Usage
$user = User::find(1);
echo $user->profile->bio;
```

#### **b. One-to-Many**

Example: A **Post** has many **Comments**.

```php
// Post.php
public function comments() {
    return $this->hasMany(Comment::class);
}

// Comment.php
public function post() {
    return $this->belongsTo(Post::class);
}

// Usage
$post = Post::find(1);
foreach ($post->comments as $comment) {
    echo $comment->content;
}
```

#### **c. Many-to-Many**

Example: **Users** belong to many **Roles**.

```php
// User.php
public function roles() {
    return $this->belongsToMany(Role::class);
}

// Role.php
public function users() {
    return $this->belongsToMany(User::class);
}

// Usage
$user = User::find(1);
foreach ($user->roles as $role) {
    echo $role->name;
}
```

---

### **2️⃣ Query Scopes**

Use query scopes to encapsulate common query logic.

```php
// User.php
public function scopeActive($query) {
    return $query->where('status', 'active');
}

// Usage
$activeUsers = User::active()->get();
```

---

### **3️⃣ Mutators and Accessors**

Transform attributes when retrieving or setting them.

```php
// User.php
public function getFullNameAttribute() {
    return "{$this->first_name} {$this->last_name}";
}

public function setPasswordAttribute($value) {
    $this->attributes['password'] = bcrypt($value);
}

// Usage
$user = User::find(1);
echo $user->full_name;

$user->password = 'secret'; // Automatically hashed
```

---

## **Step 2: Authentication**

Laravel provides built-in tools for authentication. You can use **Laravel Breeze** or **Jetstream** for scaffolding.

### **Quick Setup with Breeze**

1️⃣ Install Laravel Breeze:
```bash
composer require laravel/breeze --dev
php artisan breeze:install
npm install && npm run dev
php artisan migrate
```

2️⃣ Access authentication pages:
- Visit `/register` to register a user.
- Visit `/login` to log in.

### **Customizing Authentication**

1. Add custom logic in `AuthServiceProvider` or middleware.
2. Customize views in `resources/views/auth`.

---

## **Step 4: File Storage**

Laravel provides a powerful **file storage system**.

### **Uploading a File**

1️⃣ Create a file upload form:

```html
<form action="/upload" method="POST" enctype="multipart/form-data">
    @csrf
    <input type="file" name="file">
    <button type="submit">Upload</button>
</form>
```

2️⃣ Handle file upload in the controller:

```php
public function upload(Request $request) {
    $path = $request->file('file')->store('uploads');
    return response()->json(['path' => $path]);
}
```

### **Storing Files**

Store files in different disks (local, s3, etc.):

```php
use Illuminate\Support\Facades\Storage;

Storage::disk('local')->put('example.txt', 'File content');
```

---


### **✅ Next Steps**
- ✅ Install **Laravel**, **Symfony**, or your preferred PHP framework.
- ✅ Set up **Nginx or Apache** with proper configurations.
- ✅ Configure **MySQL/PostgreSQL databases**.
- ✅ Use **VS Code with PHP extensions** for better development.
- ✅ Set up **Redis and queues** for optimizing PHP applications.

Now you have a complete setup guide for **PHP development on Mac, Linux, and Windows!** 🚀
