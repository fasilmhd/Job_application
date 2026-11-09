
<?php
error_reporting(E_ALL);
ini_set('display_errors', 1);

// Check whether the form was submitted
if ($_SERVER["REQUEST_METHOD"] != "POST") {
    exit("Please submit the application form first.");
}

// Receive form data safely
$name = trim($_POST['name'] ?? '');
$email = trim($_POST['email'] ?? '');
$mobile = trim($_POST['mobile'] ?? '');
$username = trim($_POST['username'] ?? '');
$password = $_POST['password'] ?? '';

$errors = [];

// Name validation
if (empty($name)) {
    $errors[] = "Name is required";
} elseif (!preg_match("/^[a-zA-Z ]+$/", $name)) {
    $errors[] = "Name should contain only letters and spaces.";
}

// Email validation
if (empty($email)) {
    $errors[] = "Email is required";
} elseif (!filter_var($email, FILTER_VALIDATE_EMAIL)) {
    $errors[] = "Invalid email format";
}

// Mobile validation
if (empty($mobile)) {
    $errors[] = "Mobile number is required";
} elseif (!preg_match('/^[0-9]{10}$/', $mobile)) {
    $errors[] = "Invalid mobile number format";
}

// Username validation
if (empty($username)) {
    $errors[] = "Username is required";
}

// Password validation
if (
    strlen($password) < 8 ||
    !preg_match("/[A-Z]/", $password) ||
    !preg_match("/[a-z]/", $password) ||
    !preg_match("/[0-9]/", $password)
) {
    $errors[] = "Password must have at least 8 characters, one uppercase letter, one lowercase letter and one number.";
}

// Display validation errors
if (!empty($errors)) {
    echo "<h3>Please correct these errors:</h3>";
    foreach ($errors as $error) {
        echo "<p>" . htmlspecialchars($error) . "</p>";
    }
    exit();
}

// Connect to database
require_once "db.php";

// Hash password
$hashed_password = password_hash($password, PASSWORD_DEFAULT);

// Insert applicant details
$sql = "INSERT INTO applicants
        (name, email, mobile, username, password)
        VALUES (?, ?, ?, ?, ?)";

$stmt = mysqli_prepare($conn, $sql);

if (!$stmt) {
    die("Query preparation failed: " . mysqli_error($conn));
}

mysqli_stmt_bind_param(
    $stmt,
    "sssss",
    $name,
    $email,
    $mobile,
    $username,
    $hashed_password
);

// Execute query
if (mysqli_stmt_execute($stmt)) {
    echo "<h2>Application submitted successfully!</h2>";
} else {
    echo "Error saving application: " . mysqli_stmt_error($stmt);
}

mysqli_stmt_close($stmt);
mysqli_close($conn);
?>
