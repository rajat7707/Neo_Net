# neo-net

<?php

session_start();
error_reporting(0);
include_once('dbcon.php');

$query = mysqli_query($con , "select * from registeruser");
$res = mysqli_fetch_array($query);

// distroying session to stop user to access pages with out login

$user = $_GET['user'];
if(isset($user)){
  session_destroy();
  unset($_SESSION['username']); 
     echo "<script> alert('you  are not Login !! please Login') </script>";
     echo "<script>window.location = 'login.php '</script>";
   }
//-----------------------------------------------------------------------------------------------------------------------------------

if(isset($_POST['login'])){

    $name = $_POST['uname'];
    $_SESSION['username'] = $name ;
    $password=$_POST['pass'];
    $_SESSION['userpass'] = $password ;
    $shidden  = $_POST['shidden'];
    $_SESSION['status'] = $shidden ;


//code for free user

    $q = mysqli_query($con , "select timestamp from registeruser where username = '$name' AND user_duration = 'guest user' ");
    $arr = mysqli_fetch_array($q);
    date_default_timezone_set('Asia/kolkata');
    $regd = $arr['timestamp'];
    $sregdt = date('Y-m-d H:i:s', strtotime($regd . ' +1 day'));
    //echo "<br/> Ur account will be expired On : ".$sregdt." </br>" ;
    //echo "<br/> u create the account on : ".$regd." </br>" ;

     $_SESSION['daytime'] = $regd;
     $_SESSION['oneday'] = $sregdt ;

    $td = date('Y-m-d H:i:s');
      
         if($td > $sregdt){  
              if(mysqli_query($con , "update registeruser set status = 0 , timestamp = '$regd'  where username = '$name' ")){
                echo "<script> alert(<span style=color:red>+'Your Trial Of One Day For Our Service Is Completed !! Your account is                              deactivated'+</span>); </script>";
                     }
           }else{
                 if( mysqli_num_rows(mysqli_query($con,"select username,password,status from registeruser where username='$name' AND                            password='$password' AND status='$shidden' ")) ){
                      //  header('Location:userinformation.php');
                          echo "<script>window.location = 'userinformation.php '</script>";
                    }
               }
     }

//-----------------------------------------------------------------------------------------------------------------------------------

// code for one week user 

if(isset($_POST['login'])){           
      
$q = mysqli_query($con , "select timestamp from registeruser where username = '$name' AND user_duration = '1 week User' ");
$arr = mysqli_fetch_array($q);
date_default_timezone_set('Asia/kolkata');
$regddate = $arr['timestamp'];
$sevendaysuser = date('Y-m-d H:i:s', strtotime($regddate . ' +7 days'));
//echo  "<br/> u create the account on : ". $regddate."</br>";
//echo "Ur account will be expired On : " .$sevendaysuser."</br>";
 $td = date('Y-m-d H:i:s'); 

     $_SESSION['weektime'] = $regddate;
     $_SESSION['sevenday'] = $sevendaysuser ; 
              
              if($td > $sevendaysuser){  
                mysqli_query($con , "update registeruser set status = 0 , timestamp = '$regddate' where username = '$name'  ");
              }else{
                if(mysqli_num_rows(mysqli_query($con,"select username,password,status from registeruser where username='$name' AND                           password='$password' AND status='$shidden' "))){
                   // header('Location:userinformation.php');
                   echo "<script>window.location = 'userinformation.php '</script>";
                }
               }

        }
        
  //-----------------------------------------------------------------------------------------------------------------------------------


// code for one monty user

if(isset($_POST['login'])){

$q = mysqli_query($con , "select timestamp from registeruser where username = '$name' AND user_duration = '1 month user' ");
$arr = mysqli_fetch_array($q);
date_default_timezone_set('Asia/kolkata');
$regddate1 = $arr['timestamp'];
$onemonthuser = date('Y-m-d H:i:s', strtotime($regddate1 . ' +1 month'));
//echo "<br/> u create the account on : ".$regddate."</br>";
//echo "Ur account will be expired On : " .$onemonthuser."</br>";
 $td = date('Y-m-d H:i:s');  

     $_SESSION['monthtime'] = $regddate1 ;
     $_SESSION['onemonth'] = $onemonthuser ;
              
              if($td > $onemonthuser){  
                  if(mysqli_query($con , "update registeruser set status = 0 , timestamp = '$regddate1' where username = '$name' ")){
                    echo "<script> alert('Your account is deactivated'); </script>";
                         }
                    }else{
                       if( mysqli_num_rows(mysqli_query($con,"select username,password,status from registeruser where username='$name'                               AND password='$password' AND status='$shidden' ")) ){
                           // header('Location:userinformation.php');
                             echo "<script>window.location = 'userinformation.php '</script>";
                        }
               }
    }
    
//-----------------------------------------------------------------------------------------------------------------------------------
    

//code for one year user

if(isset($_POST['login'])){
$q = mysqli_query($con , "select timestamp from registeruser where username = '$name' AND user_duration = '1 year user' ");
$arr = mysqli_fetch_array($q);
date_default_timezone_set('Asia/kolkata');
$regddate2 = $arr['timestamp'];   // fetching the register date from the database.
$oneyearuser = date('Y-m-d H:i:s', strtotime($regddate2 . ' +1 year')); // adding one year in the registered date coming from database
//echo "<br/> u create the account on : ".$regddate."</br>";
//echo "Ur account will be expired On : " .$oneyearuser."</br>";
 $td = date('Y-m-d H:i:s'); 
 
 $_SESSION['yeartime'] = $regddate2;
 $_SESSION['oneyear'] = $oneyearuser ; 
              
              if($td > $oneyearuser){  
                  if(mysqli_query($con , "update registeruser set status = 0 , timestamp = '$regddate2' where username = '$name' ")){
                    echo "<script> alert('Your account is deactivated'); </script>";
                         }
                    }else{
                       if( mysqli_num_rows(mysqli_query($con,"select username,password,status from registeruser where username='$name'                                AND password='$password' AND status='$shidden' AND user_duration = '1 year user' ")) ){
                          //  header('Location:userinformation.php');
                             echo "<script>window.location = 'userinformation.php '</script>";
                        }
               }
}
