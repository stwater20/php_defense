### php_defense code

---

<code>
  
  <?php


require("database.inc.php");
$link = mysqli_connect($SERVER, $USERNAME, $PASSWORD) or die("Connect Failed");
mysqli_query($link, "SET NAMES 'UTF8'");
mysqli_select_db($link, $DATABASE) or die("Select Failed");

// define variables and set to empty values
$user_id = $email = $picture_url = "";

$allowed_hosts = array('{your_url}');
if (!isset($_SERVER['HTTP_HOST']) || !in_array($_SERVER['HTTP_HOST'], $allowed_hosts)) {
  header($_SERVER['SERVER_PROTOCOL'] . ' 400 Bad Request');
} else {
  if ($_SERVER["REQUEST_METHOD"] == "POST") {
    $user_id = test_input($_POST["user_id"]);
    $display_name = test_input($_POST["display_name"]);
    $picture_url = test_input($_POST["picture_url"]);
    $query_show = "SELECT * FROM LINE_profiles WHERE user_id = '$user_id'";
    $result_show = mysqli_query($link, $query_show) or die();
    if (mysqli_num_rows($result_show) == 0) {
      $sql = "INSERT INTO {table} VALUES('$user_id','$display_name','$picture_url',0)";
      $result = mysqli_query($link, $sql) or die();
    } else {
      $target_row = mysqli_fetch_array($result_show);
      $count = $target_row[3] + 1;
      $sql = "UPDATE {table} SET {columns} = $count WHERE {columns} = '$user_id'";
      $result = mysqli_query($link, $sql) or die();
    }
  }
}

function test_input($data)
{
  $data = trim($data);
  $data = stripslashes($data);
  $data = htmlspecialchars($data);
  return $data;
}

echo $user_id . " " . $display_name . " " . $picture_url;

</code>
  
