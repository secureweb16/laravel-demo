<?php member_user_middleware();
/* Template Name: Member Profile */
get_header();

$current_user = wp_get_current_user();
$updateError = 0;
$error = 0;
// updating user profile starts
if( isset( $_POST['submit'] ) )
{
  // print_a($_POST); exit;
  $first_name = isset( $_POST['first_name'] )?$_POST['first_name']:'';
  $last_name = isset( $_POST['last_name'] )?$_POST['last_name']:'';
  $email = isset( $_POST['email'] )?$_POST['email']:'';
  $new_password = isset( $_POST['new_password'] )?$_POST['new_password']:'';
  $confirm_password = isset( $_POST['confirm_password'] )?$_POST['confirm_password']:'';

  $updateArr = array( 'ID' => $current_user->ID, 'user_email' => $email );


  if( $new_password != $confirm_password )
  {
    $error = 1;
  }

  if( $new_password = $confirm_password)
  {
    $updateArr['user_pass'] = $new_password;
  }

  if( $error == 0 ){
    $user_id = wp_update_user( $updateArr );
    if ( !is_wp_error( $user_id ) ) {
      update_user_meta($current_user->ID,'first_name',$first_name);
      update_user_meta($current_user->ID,'last_name',$last_name);    
      $updateError = 2;
    } else {
      $updateError = 1;
    }
  }


}
// updating user profile ends

$current_user = wp_get_current_user();
$user_meta = get_user_meta($current_user->ID);

$user_login = isset( $current_user->user_login )? $current_user->user_login:'';
$user_email = isset( $current_user->user_email )? $current_user->user_email:'';

$first_name = ( $user_meta['first_name'][0] )?$user_meta['first_name'][0]:'';
$last_name = ( $user_meta['last_name'][0] )?$user_meta['last_name'][0]:'';

?>

<div id="main-content" class="patterbg"> 

 <div class="innerbanner" style="background:url('<?php echo $thumb['0'];?>') center center no-repeat; background-size:cover;">
  <div class="et_pb_row">
    <h1><?php the_title(); ?></h1>
  </div>		 
</div>  
<div class="forexcontainer">
  <div class="secpt45 secpb45">
    <div class="members">
     <div class="membermenu">
      <?php wp_nav_menu( array('menu'=>'Member Menu') );?>
     </div>
     <div class="memberpanel">
      <!-- Profile Form Starts -->
      <form method="POST">
        <div class="form-group" data-toggle="tooltip" title="Username can't be updated">       
          <input type="text" readonly class="form-control" value="<?php echo $user_login; ?>" />
        </div>
        <div class="form-group">
          <input type="text" class="form-control" placeholder="First Name" name="first_name" value="<?php echo $first_name;?>" />
        </div>
        <div class="form-group">
          <input type="text" class="form-control" placeholder="Last Name" name="last_name" value="<?php echo $last_name;?>" />
        </div>
        <div class="form-group">
          <input type="email" class="form-control" placeholder="test@gmail.com" name="email" value="<?php echo $user_email;?>" />
        </div>     
        <div class="form-group">
          <input type="password" class="form-control" placeholder="New Password" name="new_password" value="" />          
        </div> 
        <div class="form-group">
          <input type="password" class="form-control" placeholder="Confirm Password" name="confirm_password" value="" />       
        </div>     
        <div class="form-group">
          <input type="submit" value="Update" name="submit" class="bgSalmon" />
        </div>       
      </form>
      <div class="formmsgs">
        <?php 
          if( $error == 1 )
          {
            echo '<div class="alert alert-danger"> Password Mismatch </div>';
          }
          if( $updateError == 1 )
          {
            ?>
            <div class="alert alert-danger">
              <?php echo  $user_id->get_error_message(); ?>
            </div>
            <?php 
          }else if( $updateError == 2 )
          {
            ?>
              <div class="alert alert-success">
                Profile updated successfuly.
              </div>
            <?php
          }
        ?>
      </div>
      <!-- Profile Form Ends -->
    </div>      
  </div>
</div>  
</div> 	   
</div>

<?php get_footer(); ?>
