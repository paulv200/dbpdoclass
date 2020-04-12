# phppdodbclass

This is a simple db pdo class which also logs errors.  It is able to do SELECT, UPDATE, INSERT and DELETE statements with parameters entered into an arrray.


## Database configuration file.

You will require a config.php file in the same folder as the Dbpdo.class.php file.

$dbhost 	= "localhost";				//usually localhost
$dbusername = "database-user-name";
$dbpass 	= "database-password";"
$dbname		= "database-name";"


## The files

Dbpdo.class.php is the database class itself.
Log.class.php is the class for error logging.
config.php is where you place your database configuration details.
examples.php is a file showing some examples.
testing.php is a file used for testing against an example database table.
testing.sql is a file for creating a table for use with testing.php.
logs folder is the location of any error logs that may be generated


## Basic use
$recid = 3;
$publish = true;

require_once("config.php");
require_once("Dbpdo.class.php");

$db = new Db();
//$db->setdebug(true);		//Use this to display some debug infomation
	
$sql = "SELECT * FROM tblitems WHERE recid = :recid AND publish = :publish";
$params = array( 
	'0' => array ("recid" => $recid, "type" => PDO::PARAM_INT), 
	'1' => array ("publish" => $publish, "type" => PDO::PARAM_BOOL)
	);

$productdetails = $db->query($sql, $params);
foreach( $productdetails as $d_row ) {
	echo( $d_row["item_name"] . "<br/>");
}


## params array
The $params array consists of the parmater name and an optional data type for that parameter.  So in the above example we have recid which is an INT type and a publish flag which is a boolean type.

Some further examples of parameter arrays:

The following shows string entries with an optional length field.

$params = array( 
	'0' => 	array ("item_name" => $item_name, "type" => PDO::PARAM_STR, 100),
	'1' => 	array ("item_number" => $item_number, "type" => PDO::PARAM_STR, 20)
	);


The following shows an example where no optional entries are used:

$params = array( 
	'0' => array ("recid" => $recid)
	'1' => array ("publish" => $publish),
	'2' => 	array ("item_name" => $item_name)
	);


## Fetching data
$sql = "SELECT * FROM tblitems";
$productdetails =  $db->query($sql);
foreach( $productdetails as $d_row ) {
	echo( $d_row["item_name"] . "<br/>");
}

By default, this returns an assoicate array using FETCH_ASSOC as the fetch mode.

If you want to use another fetchmode just give it as a parameter.  So in the above example:

$productdetails =  $db->query($sql, null, PDO::FETCH_NUM);


## Fetch values
$recid = 3;
$sql = "SELECT item_name FROM tblitems WHERE recid = :recid";
$params = array( 
	'0' => array ("recid" => $recid, "type" => PDO::PARAM_INT), 
	);

$products = $db->query($sql, $params);
echo( $products[0]["item_name"] );


## Fetching single value
$recid = 2;
$sql = "SELECT item_name FROM tblitems WHERE recid= :recid";
$params = array( 
	'0' => array ("recid" => $recid, "type" => PDO::PARAM_INT)
	);
$item_name = $db->single($sql, $params);
echo($item_name);


## Fetch a Single Row
$recid = 3;
$sql = "SELECT item_name, item_number FROM tblitems WHERE recid = :recid";
$params = array( 
	'0' => array ("recid" => $recid, "type" => PDO::PARAM_INT)
	);
$items = $db->row( $sql, $params );
print_r($items);


## Fetch a column	
$sql = "SELECT item_name FROM tblitems";
$items = $db->column($sql);
print_r($items);


## Insert Statement
$item_number = "Fruit";
$mc_gross = "2.27";
sql = "INSERT INTO tblitems ( item_number, mc_gross) VALUES ( :item_number, :mc_gross )";
$params = array( 
	'0' => array ("item_number" => $item_number, "type" => PDO::PARAM_STR), 
	'1' => array ("mc_gross" => $mc_gross, "type" => PDO::PARAM_STR)
	);
$insert	= $db->query( $sql, $params );	//the number of rows affected
$lastinsertedid = $db->lastInsertId();	//returns last inserted id	


## Update Statement
$recid = 3;
$item_name = "Apples";
$sql = "UPDATE tblitems SET item_name = :item_name WHERE recid = :recid";
$params = array( 
	'0' => array ("item_name" => $item_name, "type" => PDO::PARAM_STR), 
	'1' => array ("recid" => $recid, "type" => PDO::PARAM_INT)
	);
$update	= $db->query( $sql, $params );


## Delete statement
$recid = 4;
$sql = " DELETE FROM tblitems WHERE recid = :recid ";
$params = array( 
	'0' => array ("recid" => $recid, "type" => PDO::PARAM_INT), 
	);
$delete	 =  $db->query( $sql, $params ); 


## Testing

Run the testing.sql on a MysQL database to create a testing table, then use testing.php to show results.

Set a debug flag using:

$db->setdebug(true);

