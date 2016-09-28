WP Filters Extras
=================

Add 2 PHP functions for WP Filter API, it's allow to remove filter/action on hooks for specific cases with CLASS PHP.

**These functions are very useful for removing filters and actions from plugin not very well developed!**

## Remove filters with method name only

`remove_filters_with_method_name( $hook_name = '', $method_name = '', $priority = 0 );`

Allow to remove method for an hook when, it's a class method used and class don't have global for instanciation !

## Remove filters with class and method name

 `remove_filters_for_anonymous_class( $hook_name = '', $class_name ='', $method_name = '', $priority = 0 );`

Allow to remove method for an hook when, it's a class method used and class don't have global for instanciation, but you know the class name :)

## Usage

### Example :

```php
// First sample
class MyClassA {
    function __construct() {
        add_action( 'wp_footer', array( $this, 'my_action' ), 10 );
    }
    function my_action() {
        print '<h1>' . __class__ . ' - ' . __function__ . '</h1>';
    }
}
new MyClassA();

// Second sample
class MyClassB {
    function __construct() {
        add_action( 'wp_footer', array( $this, 'my_action' ), 10 );
    }
    function my_action() {
        print '<h1>' . __class__ . ' - ' . __function__ . '</h1>';
    }
}
new MyClassB();
```

### The problem

With WordPress API, you can't remove the filter `my_action` because MyClassA and MyClassB don't have a variable !
The right way to load class is : `$my_class_b = new MyClassB();`

### Way 1

This first method, `remove_filters_with_method_name();`, is fairly aggressive because it removes all filters that have the name "my_action", whatever the class.

```php
remove_filters_with_method_name( 'wp_footer', 'my_action', 10 );
```

This call will remove the 2 filters for classes MyClassA and MyClassB.

### Way 2

The second method, `remove_filters_for_anonymous_class();`, is more accurate because it is necessary to specify both the name of the method but also the name of the PHP class.

```php
remove_filters_for_anonymous_class( 'wp_footer', 'MyClassB', 'my_action', 10 );
```

This call will only remove the filter for the class MyClassB.

## Tips

The priority argument of the two functions is important, you must use the same priority as the hook you want to remove !