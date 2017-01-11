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
	echo '<style type="text/css"> h1 a { background: url('.get_bloginfo('template_directory').' your logo image url here) 50% 50% no-repeat !important; }</style>';
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
