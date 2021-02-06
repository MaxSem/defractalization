# Is PHP still a fractal of bad design?

Eevee's famous article [PHP: a fractal of bad design](https://eev.ee/blog/2012/04/09/php-a-fractal-of-bad-design/) is pretty influential because it hits pretty close to home. But it was written in 2012 - did PHP get any better since then? I'm writing this in early 2021, and PHP has evolved quite a bit over these 9 years, going from 5.4 to 8.0.

## [General stance](https://eev.ee/blog/2012/04/09/php-a-fractal-of-bad-design/#stance)

> PHP is full of surprises: mysql_real_escape_string, E_ALL

"Surprise" is an inherently subjective position. I've no doubt that there are a lot of surprising things in PHP, some of them have been resolved, including both of the aforementioned blunders: whole mysql extension was removed from PHP, taking `mysql_real_escape_string()` with it; and `E_ALL` now includes `E_STRICT`.

> PHP is inconsistent: strpos, str_rot13

No major improvements.

> PHP requires boilerplate: error-checking around C API calls, ===

> PHP is flaky: ==, foreach ($foo as &$bar)

> PHP is opaque: no stack traces by default or for fatals, complex error reporting

