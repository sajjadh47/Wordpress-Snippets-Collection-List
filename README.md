# Wordpress Snippets Collection List

Wordpress Code Snippets Collection List for WP Theme And Plugin Development

## 1. Remove URL Field From Comments

```php
function remove_comment_fields( $fields )
{
    unset( $fields['url'] ); //Deletes URL From The Comment
    
    return $fields;
}

add_filter( 'comment_form_default_fields', 'remove_comment_fields' );

```
## 2. Change WordPress Login Pages LOGO

```php
## Changes Login LOGO Image
function custom_login_logo()
{
    echo '<style type="text/css"> h1 a { background: url( " your logo image url here " ) 50% 50% no-repeat !important; } </style>';
}

add_action( 'login_head', 'custom_login_logo' );

```
## 3. Change Author Slug URL

```php
add_action( 'init', 'cng_author_base' );
function cng_author_base()
{
    global $wp_rewrite;
    
    $author_slug = 'profile'; // change slug name
    
    $wp_rewrite->author_base = $author_slug;
}
```
## 4. Put PHP Codes In Wordpress Text Widget Field

```php
add_filter( 'widget_text', 'php_text', 99 );

function php_text( $text )
{
    if ( strpos( $text, '<' . '?' ) !== false )
    {
  	ob_start();
 	    eval( '?' . '>' . $text );
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
function login_with_email_address( $username )
{
    $user = get_user_by( 'email', $username );
    
    if( ! empty( $user->user_login ) )
    	$username = $user->user_login;
    return $username;
}

add_action( 'wp_authenticate', 'login_with_email_address' );

function change_username_wps_text( $text )
{
    if( in_array( $GLOBALS['pagenow'], array( 'wp-login.php' ) ) )
    {
    	if ( $text == 'Username' )
	{
	    $text = 'Username / Email'; }
        }
        
	return $text;
   }
}

add_filter( 'gettext', 'change_username_wps_text' );

```
## 7. Register A Shortcode To Create A Woocommerce Product Categories Dropdown List

```php

<?php

/**
 * WooCommerce Extra Feature
 * --------------------------
 *
 * Register a shortcode that creates a product categories dropdown list
 *
 * Use: [product_categories_dropdown orderby="title" count="0" hierarchical="0"]
 *
 */
add_shortcode( 'product_categories_dropdown', 'woo_product_categories_dropdown' );

function woo_product_categories_dropdown( $atts )
{
	extract( shortcode_atts( array(
	'count'         => '0',
	'hierarchical'  => '0',
	'orderby' 	    => ''
	), $atts ) );

	ob_start();
	
	$c = $count;
	$h = $hierarchical;
	$o = ( isset( $orderby ) && $orderby != '' ) ? $orderby : 'order';

	// Stuck with this until a fix for http://core.trac.wordpress.org/ticket/13258
	woocommerce_product_dropdown_categories( $c, $h, 0, $o );

	?>
	<script type='text/javascript'>
	/* <![CDATA[ */
		var product_cat_dropdown = document.getElementById("dropdown_product_cat");
		function onProductCatChange() {
			if ( product_cat_dropdown.options[product_cat_dropdown.selectedIndex].value !=='' ) {
				location.href = "<?php echo home_url(); ?>
				/?product_cat="+product_cat_dropdown.options[product_cat_dropdown.selectedIndex].value;
			}
		}
		product_cat_dropdown.onchange = onProductCatChange;
	/* ]]> */
	</script>
	<?php

	return ob_get_clean();
}
```
## 8. Woocommerce Email Info Echo Codes For Email Templating

```php
//Code For Order ID
<?php echo $order->id; ?>
 
//Code For Order Date
<?php echo $order->order_date; ?>
 
//Code For Shipping First and Last Name
<?php echo $order->shipping_first_name . " " . $order->shipping_last_name; ?>
 
//Code For Shipping address
<?php echo $order->shipping_address_1; ?>
 
//Code For Shipping Apartment Number
<?php if( $order->shipping_address_2 != "" ){ echo '<br>' . $order->shipping_address_2; } ?>
 
//Code For Shipping Country
<?php $countries = new WC_Countries; $shipping_country = $order->shipping_country;
echo ( $shipping_country && isset( $countries->countries[ $shipping_country ] ) ) ? 
$countries->countries[ $shipping_country ] : $shipping_country; ?>
 
//Code For Billing First and Last Name
<?php echo $order->billing_first_name . " " . $order->billing_last_name; ?>
 
//Code For Billing address
<?php echo $order->billing_address_1; ?>
 
//Code For Billing Apartment Number
<?php if( $order->billing_address_2 != "" ){ echo '<br>' . $order->billing_address_2; } ?>
 
//Code For Billing Country
<?php $countries = new WC_Countries; $billing_country = $order->billing_country; 
echo ( $billing_country && isset( $countries->countries[ $billing_country ] ) ) ? 
$countries->countries[ $billing_country ] : $billing_country; ?>
 
//Code For Order Items/Products
<?php do_action( 'woocommerce_email_order_details', $order, $sent_to_admin, $plain_text, $email ); ?>

```
## 9. Add Custom Currency Symbol In Woocommerce Shop

```php
add_filter( 'woocommerce_currency_symbol', 'change_existing_currency_symbol', 10, 2 );

function change_existing_currency_symbol( $currency_symbol, $currency )
{
	switch( $currency )
	{
		case 'AUD': $currency_symbol = 'AUD$'; break;
	}

	return $currency_symbol;
}

```
## 10. Create Custom Page After Theme Activation

```php
if ( isset( $_GET['activated'] ) && is_admin() )
{
	$new_page_title 	= 'This is the page title';
	$new_page_content 	= 'This is the page content';
	$new_page_template 	= ''; //ex. template-custom.php. Leave blank if you don't want a custom page template.
	//don't change the code bellow, unless you know what you're doing
	$page_check 		= get_page_by_title($new_page_title);
	$new_page 		= array(
		'post_type' 	=> 'page',
		'post_title' 	=> $new_page_title,
		'post_content'	=> $new_page_content,
		'post_status' 	=> 'publish',
		'post_author' 	=> 1,
	);
	
	if( ! isset( $page_check->ID ) )
	{
		$new_page_id 	= wp_insert_post( $new_page );
		
		if( ! empty( $new_page_template ) )
		{
			update_post_meta( $new_page_id, '_wp_page_template', $new_page_template );
		}
	}
}
```
