Az functions
================
2016-06-16



Those are debug functions for lazy and impatient people like me.

They are var_dump based, but they tend to enhance the visual output (depending on whether or not you use xdebug, and depending 
on whether or not you are inside a cli environment), and they accept multiple arguments too.


a() is the equivalent of the var_dump() function (but is faster to type in the long run).

az() is the equivalent of the var_dump() function followed by an exit statement (useful for clearing the mess that sometimes makes it
hard to spot the debug string).



How to use?
--------------

Copy paste this where you want (probably in an init file).


```php
//------------------------------------------------------------------------------/
// BONUS FUNCTIONS, SO HANDFUL... (a huge time saver in the end)
//------------------------------------------------------------------------------/
if (!function_exists('a')) {
    function a()
    {
        foreach (func_get_args() as $arg) {
            ob_start();
            var_dump($arg);
            $output = ob_get_clean();
            if ('1' !== ini_get('xdebug.default_enable')) {
                $output = preg_replace("!\]\=\>\n(\s+)!m", "] => ", $output);
            }
            if ('cli' === PHP_SAPI) {
                echo $output;
            }
            else {
                echo '<pre>' . $output . '</pre>';
            }
        }
    }
    function az()
    {
        call_user_func_array('a', func_get_args());
        exit;
    }
}
```