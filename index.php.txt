
<!DOCTYPE html>
<html>
<head>
    <title>Job Application Form</title>
</head>

<body>

    <h2>Job Application Form</h2>

    <form action="process.php" method="POST">

        <label>Name:</label>
        <input type="text" name="name" required>
        <br><br>

        <label>Email:</label>
        <input type="email" name="email" required>
        <br><br>

        <label>Mobile:</label>
        <input type="text" name="mobile" required>
        <br><br>

        <label>Username:</label>
        <input type="text" name="username" required>
        <br><br>

        <label>Password:</label>
        <input type="password" name="password" required>
        <br><br>

        <button type="submit">Apply Now</button>

    </form>

</body>
</html>
