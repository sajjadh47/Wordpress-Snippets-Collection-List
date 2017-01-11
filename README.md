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
## 2. Change WordPress Login Pages Logo, URL, and Title

```php
## Changes Login LOGO Image
function custom_login_logo() {
	echo '<style type="text/css"> h1 a { background: url('.get_bloginfo('template_directory').' your logo image url here) 50% 50% no-repeat !important; }</style>';
}
add_action('login_head', 'custom_login_logo');

```
