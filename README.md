# Wordpress Snippets Collection List
Wordpress Code Snippets Collection List for WP Theming And Plugin Development

## 1. Remove URL Field From Comments

```php
function remove_comment_fields($fields) {
    unset($fields['url']); //Deletes URL From The Comment
    return $fields;
}
add_filter('comment_form_default_fields','remove_comment_fields'); // Job done using filter function

```
## 2. Change WordPress Login Pages LOGO

```php
## Changes Login LOGO Image
function custom_login_logo() {
	echo '<style type="text/css"> 
			h1 a { background: url('.get_bloginfo('template_directory') . ' your logo image url here) 
				50% 50% no-repeat !important; }
	</style>';
}
add_action('login_head', 'custom_login_logo');

```
## 3. Change Author Slug URL

```php
add_action('init', 'cng_author_base');
function cng_author_base() {
    global $wp_rewrite;
    $author_slug = 'profile'; // change slug name
    $wp_rewrite->author_base = $author_slug;
}
```
## 4. Put PHP Codes In Wordpress Text Widget Field

```php
add_filter('widget_text', 'php_text', 99);

function php_text($text) {
 if (strpos($text, '<' . '?') !== false) {
  	ob_start();
 		eval('?' . '>' . $text);
	    $text = ob_get_contents();
	ob_end_clean();
 }
 return $text;
}

```
## 5. Empty Your WordPress Trash Using Code

```php

define( 'EMPTY_TRASH_DAYS', 10 ); // 10 days

```
## 6. Login With Username or Email Address

```php
function login_with_email_address($username) {
        $user = get_user_by('email',$username);
        if(!empty($user->user_login))
                $username = $user->user_login;
        return $username;
}
add_action('wp_authenticate','login_with_email_address');
function change_username_wps_text($text){
       if(in_array($GLOBALS['pagenow'], array('wp-login.php'))){
         if ($text == 'Username'){$text = 'Username / Email';}
            }
                return $text;
         }
add_filter( 'gettext', 'change_username_wps_text' );

```
