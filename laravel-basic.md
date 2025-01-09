Awesome! Now that you understand Object-Oriented PHP, let’s move on to Intermediate Laravel concepts. These are crucial for building dynamic and feature-rich applications.

Step 1: Advanced Eloquent ORM

Laravel’s Eloquent ORM simplifies database interactions and provides a clean way to work with your models.

1. Relationships

Relationships are the backbone of Eloquent, allowing models to relate to one another.

a. One-to-One

Example: A user has one profile.

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

b. One-to-Many

Example: A post has many comments.

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

c. Many-to-Many

Example: Users belong to many roles.

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

2. Query Scopes

Use query scopes to encapsulate common query logic.

// User.php
public function scopeActive($query) {
    return $query->where('status', 'active');
}

// Usage
$activeUsers = User::active()->get();

3. Mutators and Accessors

Transform attributes when retrieving or setting them.

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

Step 2: Authentication

Laravel provides built-in tools for authentication. You can use Laravel Breeze or Jetstream for scaffolding.

Quick Setup with Breeze
	1.	Install Laravel Breeze:

composer require laravel/breeze --dev
php artisan breeze:install
npm install && npm run dev
php artisan migrate


	2.	Access authentication pages:
	•	Visit /register to register a user.
	•	Visit /login to log in.

Customizing Authentication
	1.	Add custom logic in AuthServiceProvider or middleware.
	2.	Customize views in resources/views/auth.

Step 3: Building RESTful APIs

Laravel makes it simple to create APIs with routes, controllers, and resources.

1. Define API Routes

In routes/api.php:

use App\Http\Controllers\Api\BookController;

Route::get('/books', [BookController::class, 'index']);
Route::post('/books', [BookController::class, 'store']);

2. Create an API Controller

php artisan make:controller Api/BookController

// BookController.php
public function index() {
    return Book::all();
}

public function store(Request $request) {
    $book = Book::create($request->all());
    return response()->json($book, 201);
}

3. Use Resources for API Responses

php artisan make:resource BookResource

// BookResource.php
public function toArray($request) {
    return [
        'id' => $this->id,
        'title' => $this->title,
        'author' => $this->author,
    ];
}

// Usage in Controller
return BookResource::collection(Book::all());

Step 4: File Storage

Laravel provides a powerful file storage system.

Uploading a File
	1.	Create a file upload form:

<form action="/upload" method="POST" enctype="multipart/form-data">
    @csrf
    <input type="file" name="file">
    <button type="submit">Upload</button>
</form>


	2.	Handle file upload in the controller:

public function upload(Request $request) {
    $path = $request->file('file')->store('uploads');
    return response()->json(['path' => $path]);
}



Storing Files

Store files in different disks (local, s3, etc.):

use Illuminate\Support\Facades\Storage;

Storage::disk('local')->put('example.txt', 'File content');

Practice Exercise
	1.	Create a Blog model with:
	•	title and content columns.
	•	A relationship where one blog has many comments.
	2.	Implement CRUD APIs for blogs.
	3.	Add a file upload feature where users can upload an image for their blog.

Once you’ve completed this, let me know, and we’ll dive into Advanced Laravel concepts!