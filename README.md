```php
<?php
include 'config.php';

$uid = $_SESSION['uid'];

if($_POST['impartSubmit'])
{
    $file = $_FILES['importfile']['tmp_name'];
    $countfile = new SplFileObject($file, 'r');
    $countfile->seek(PHP_INT_MAX);
    $rowCount = $countfile->key(); //count the no of rows available in csv file

    if (!empty($file))
    {
        $handle = fopen($file, 'r');
        if ($handle !== false)
        {
            $counter = 0;

            while (($csv = fgetcsv($handle)) !== false && $counter < $rowCount)
            {
                $student_mobile = $csv[0];
                $user_id = $csv[1];
                $premium_ac_id = $csv[2];
                $name = $csv[3];
                $gender = $csv[4];

                //insert data from CSV file
                $sql = "insert into `tablename`(`student_mobile`, `user_id`, `premium_ac_id`, `name`, `gender`) values ('$student_mobile', '$user_id',  '$premium_ac_id', '$name', '$gender')";

                $res = $conn->query($sql) or die($conn->error);

                $counter++;
            }
            fclose($handle);
            ?>
    		<script type="text/javascript">
    			alert('Data Imported Successfully');
    			window.location.href='report_upload.php';
    		</script>
        	<?php
        }
        else {
            ?>
    		<script type="text/javascript">
    			alert('Failed to open the CSV file.');
    			window.location.href='report_upload.php';
    		</script>
        	<?php
        }

    }
    else {
        ?>
        <script type="text/javascript">
            alert('Please select a CSV file.');
            window.location.href='report_upload.php';
        </script>
        <?php
    }
}
?>


```
