# wordpress-coding-cheatsheet

# Plugin development
## Restrict direct access
```php
if ( ! defined( 'ABSPATH' ) ) {
	die();
}
```
