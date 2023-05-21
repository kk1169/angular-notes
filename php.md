# Make connection
config/db.php
```
<?php

// Database configuration
$servername = "localhost";
$username = "root";
$password = "";
$database = "test_app";

// Create connection
$conn = new mysqli($servername, $username, $password, $database);

// Check connection
if ($conn->connect_error) {
    die("Connection failed: " . $conn->connect_error);
}
```
index.php
```
<?php

include "./config/db.php";

// SELECT USERS
$userSql = "SELECT * FROM users";
$userRecords = $conn->query($userSql);

?>

<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>

    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha3/dist/css/bootstrap.min.css" rel="stylesheet">
</head>

<body>

    <!-- table -->
    <div class="container mt-5">
        <table class="table">
            <thead>
                <tr>
                    <th scope="col">ID</th>
                    <th scope="col">NAME</th>
                    <th scope="col">EMAIL</th>
                    <th scope="col">MOBILE</th>
                </tr>
            </thead>
            <tbody>


                <?php 
                if ($userRecords->num_rows > 0) {

                    while ($userRow = $userRecords->fetch_assoc()) {
                        echo '<tr>
                        <th scope="row">'.$userRow['id'].'</th>
                        <td>'.$userRow['name'].'</td>
                        <td>'.$userRow['email'].'</td>
                        <td>'.$userRow['mobile'].'</td>
                        </tr>';
                    }
                }
                ?>
            </tbody>
        </table>
    </div>

</body>

</html>
```
