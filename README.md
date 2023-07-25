# wordpress-coding-cheatsheet

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
