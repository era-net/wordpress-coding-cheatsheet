# wordpress-coding-cheatsheet
## Table of contents
* [Plugin development](#plugin-development)
    * [Restrict direct access](#restrict-direct-access)
    * [Plugin activation](#plugin-activation)
    * [Plugin deactivation](#plugin-deactivation)
    * [Plugin uninstallation](#plugin-uninstallation)
    * [Enqueue scripts in admin section](#enqueue-scripts-in-admin-section)
    * [Add new sub menu item with page](#add-new-sub-menu-item-with-page)
    * [Add some content after each post](#add-some-content-after-each-post)

## Plugin development
### Restrict direct access
```php
if ( ! defined( 'ABSPATH' ) ) {
    die();
}
```

### Plugin activation
```php
function activate_my_plugin() {
    // do stuff for activation ...
}
register_activation_hook(__FILE__, 'activate_my_plugin');
```

### Plugin deactivation
```php
function deactivate_my_plugin() {
    // do stuff for deactivation ...
    flush_rewrite_rules();
}
register_deactivation_hook(__FILE__, 'deactivate_my_plugin');
```

### Plugin uninstallation
```php
function uninstall_my_plugin() {
    // do stuff for uninstallation ...
}
register_uninstall_hook(__FILE__, 'uninstall_my_plugin');
```

### Enqueue scripts in admin section
**Method 1 (simple)**
```php
function enqueue_my_scripts() {
    wp_enqueue_script('era_index_script', plugin_dir_url(__FILE__) . 'js_file.js', array(), '1.0.0', true);
}
add_action('admin_enqueue_scripts', 'enqueue_my_scripts');
```
**Method 2 (with localization to pass arguments from php to javascript)**
```php
function enqueue_my_localized_scripts() {
    wp_register_script('script_name', plugin_dir_url(__FILE__) . 'js_file.js', array(), '1.0.0', true);
    wp_enqueue_script('script_name');
    wp_localize_script('script_name', 'samle_obj', ['url' => plugin_dir_url(__FILE__)."inc/get_count.php"]);
}
add_action('admin_enqueue_scripts', 'enqueue_my_localized_scripts');
```
now use the passed argument:
```jquery
jQuery(document).ready(() => {
    console.log(sample_obj.url);
});
```

### Add new sub menu item with page
```php
function new_menu_page_content() {
    // check if user is allowed to access
    if ( ! current_user_can('manage_options') ) {
        return;
    }

    echo '<div class="wrap"><p>Hello World !</p></div>';
}
function era_simple_options_page() {
    add_submenu_page(
		'options-general.php',
		"My Options",
		"My Options",
		'manage_options',
		'some-options',
		'new_menu_page_content'
	);
}
add_action('admin_menu', 'era_simple_options_page');
```

### Add some content after each post
```php
function append_after_content($content) {
    return $content . '<div style="text-align: right;"><small>im so small :)</small></div>';
}
add_filter('the_content', 'append_after_content');
```
