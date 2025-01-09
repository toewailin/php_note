Great! Let’s move on to PHP and the Web. This section focuses on how PHP interacts with forms, sessions, and file uploads.

Step 1: Handling Forms with PHP

Forms are used to collect data from users. PHP processes the form data using the GET or POST methods.

Example: A Simple Form
	1.	Create an form.php file:

<!DOCTYPE html>
<html>
<body>
    <form action="process.php" method="POST">
        Name: <input type="text" name="name"><br>
        Age: <input type="number" name="age"><br>
        <input type="submit" value="Submit">
    </form>
</body>
</html>


	2.	Create a process.php file:

<?php
if ($_SERVER["REQUEST_METHOD"] == "POST") {
    $name = $_POST['name'];
    $age = $_POST['age'];

    echo "Hello, $name! You are $age years old.";
} else {
    echo "Invalid Request!";
}
?>


	3.	Open form.php in your browser, fill out the form, and see how process.php displays the submitted data.

Step 2: Sessions and Cookies

Sessions

Sessions allow you to store user data across multiple pages.
	1.	Start a session and store data:

<?php
session_start(); // Start the session
$_SESSION['username'] = 'JohnDoe';

echo "Session set for username.";
?>


	2.	Access session data on another page:

<?php
session_start();
echo "Welcome, " . $_SESSION['username'];
?>


	3.	Destroy a session:

<?php
session_start();
session_unset();
session_destroy();

echo "Session destroyed.";
?>



Cookies

Cookies store data in the user’s browser for later use.
	1.	Set a cookie:

<?php
setcookie("user", "JohnDoe", time() + (86400 * 7)); // 7 days
echo "Cookie set!";
?>


	2.	Access a cookie:

<?php
if (isset($_COOKIE['user'])) {
    echo "Welcome back, " . $_COOKIE['user'];
} else {
    echo "No cookie found.";
}
?>


	3.	Delete a cookie:

<?php
setcookie("user", "", time() - 3600);
echo "Cookie deleted.";
?>

Step 3: File Uploads

HTML Form for File Upload
	1.	Create a file upload form:

<!DOCTYPE html>
<html>
<body>
    <form action="upload.php" method="POST" enctype="multipart/form-data">
        Select file: <input type="file" name="fileToUpload"><br>
        <input type="submit" value="Upload File">
    </form>
</body>
</html>



PHP Script to Handle Upload
	2.	Handle the uploaded file in upload.php:

<?php
if ($_SERVER['REQUEST_METHOD'] == 'POST') {
    $targetDir = "uploads/";
    $targetFile = $targetDir . basename($_FILES["fileToUpload"]["name"]);
    $uploadOk = 1;

    // Check if file is an image
    $fileType = strtolower(pathinfo($targetFile, PATHINFO_EXTENSION));
    if (in_array($fileType, ['jpg', 'png', 'jpeg', 'gif'])) {
        $uploadOk = 1;
    } else {
        echo "Sorry, only image files are allowed.";
        $uploadOk = 0;
    }

    // Check if $uploadOk is set to 0 by an error
    if ($uploadOk == 0) {
        echo "File not uploaded.";
    } else {
        if (move_uploaded_file($_FILES["fileToUpload"]["tmp_name"], $targetFile)) {
            echo "The file " . basename($_FILES["fileToUpload"]["name"]) . " has been uploaded.";
        } else {
            echo "Sorry, there was an error uploading your file.";
        }
    }
}
?>


	3.	Test by creating an uploads folder in your project directory and uploading a file.

Step 4: Practice Exercise
	1.	Create a simple form where a user can upload a text file.
	2.	Process the uploaded file to read and display its content.

Once you’ve tried this, let me know, and we’ll move on to Object-Oriented PHP!