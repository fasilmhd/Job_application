
<?php
require_once "db.php";

// Fetch all applicant records
$sql = "SELECT id, name, email, mobile, username FROM applicants ORDER BY id DESC";

$result = mysqli_query($conn, $sql);

if (!$result) {
    die("Error fetching records: " . mysqli_error($conn));
}
?>

<!DOCTYPE html>
<html>
<head>
    <title>Applicant Details</title>

    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f6f9;
            margin: 0;
            padding: 40px;
        }

        .container {
            max-width: 1000px;
            margin: auto;
            background: white;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
        }

        h2 {
            text-align: center;
            color: #333;
            margin-bottom: 25px;
        }

        table {
            width: 100%;
            border-collapse: collapse;
        }

        th, td {
            padding: 14px;
            text-align: left;
            border-bottom: 1px solid #ddd;
        }

        th {
            background-color: #263b70;
            color: white;
        }

        tr:hover {
            background-color: #f1f4fa;
        }

        .empty {
            text-align: center;
            padding: 25px;
            color: #777;
        }
    </style>
</head>

<body>

<div class="container">

    <h2>Applicant Details</h2>

    <table>
        <tr>
            <th>ID</th>
            <th>Name</th>
            <th>Email</th>
            <th>Mobile Number</th>
            <th>Username</th>
        </tr>

        <?php if (mysqli_num_rows($result) > 0) { ?>

            <?php while ($row = mysqli_fetch_assoc($result)) { ?>

                <tr>
                    <td><?php echo htmlspecialchars($row['id']); ?></td>
                    <td><?php echo htmlspecialchars($row['name']); ?></td>
                    <td><?php echo htmlspecialchars($row['email']); ?></td>
                    <td><?php echo htmlspecialchars($row['mobile']); ?></td>
                    <td><?php echo htmlspecialchars($row['username']); ?></td>
                </tr>

            <?php } ?>

        <?php } else { ?>

            <tr>
                <td colspan="5" class="empty">
                    No applicants found.
                </td>
            </tr>

        <?php } ?>

    </table>

</div>

</body>
</html>

<?php
mysqli_free_result($result);
mysqli_close($conn);
?>
