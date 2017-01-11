# Wordpress Snippets Collection List
Wordpress Code Snippets Collection List for WP Theming And Plugin Development


function remove_comment_fields($fields) {
    unset($fields['url']); //Deletes URL From The Comment
    return $fields;
}
add_filter('comment_form_default_fields','remove_comment_fields'); // Job done using filter function
