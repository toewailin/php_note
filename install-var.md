Great! Let’s start with Basic PHP Fundamentals. Here’s the first step: Setting Up Your Environment.

Step 1: Setting Up Your Environment
	1.	Install PHP:
	•	Download and install the latest version of PHP from php.net.
	•	Confirm installation by running php -v in your terminal or command prompt.
	2.	Install Composer:
	•	Composer is a dependency manager for PHP. Download it from getcomposer.org.
	•	Verify the installation by running composer --version.
	3.	Install a Local Development Server:
	•	Use XAMPP (Windows/Mac/Linux): Includes PHP, MySQL, and Apache.
	•	Alternatively, use Laravel Valet or Homestead (for advanced setups).
	4.	Set Up a Code Editor:
	•	Install Visual Studio Code (VS Code) or PHPStorm for writing PHP code.

Step 2: Writing Your First PHP Script
	1.	Create a file named index.php in your project folder.
	2.	Add the following code:

<?php
echo "Hello, World!";
?>


	3.	Place the file in the root directory of your server (e.g., htdocs for XAMPP).
	4.	Open your browser and go to http://localhost/index.php. You should see “Hello, World!”.

Step 3: Understanding PHP Basics

Let’s go over some key concepts step by step.

1. Variables
	•	Variables in PHP start with $ and can store different data types.

<?php
$name = "John";
$age = 25;
$isStudent = true;

echo "Name: $name, Age: $age";
?>



2. Data Types
	•	PHP supports several data types:
	•	Strings: "Hello"
	•	Integers: 123
	•	Floats: 3.14
	•	Booleans: true or false
	•	Arrays: ["apple", "banana"]
	•	Objects and NULL.

3. Control Structures
	•	Conditional statements like if, else, and switch:

<?php
$score = 85;

if ($score > 90) {
    echo "Grade: A";
} elseif ($score > 75) {
    echo "Grade: B";
} else {
    echo "Grade: C";
}
?>


	•	Loops like for, while, and foreach:

<?php
for ($i = 1; $i <= 5; $i++) {
    echo "Number: $i<br>";
}
?>

Step 4: Practice Exercise
	1.	Create a PHP file that does the following:
	•	Defines a variable for your name and age.
	•	Uses an if condition to check if you are an adult (18 or older).
	•	Displays a message indicating whether you’re an adult.
	2.	Example output:

Hello, John! You are an adult.



Try this exercise and share your code/output with me! Once you’re comfortable, we’ll move to PHP and the Web.