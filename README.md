# Is PHP still a fractal of bad design?

Eevee's famous article [PHP: a fractal of bad design](https://eev.ee/blog/2012/04/09/php-a-fractal-of-bad-design/) is pretty influential because it hits pretty close to home. But it was written in 2012 - did PHP get any better since then? I'm writing this in early 2021, and PHP has evolved quite a bit over these 9 years, going from 5.4 to 8.0. My article concentrates on analyzing whether PHP has improved, not on debating the original article. Full disclosure: I'm a (very minor) contributor to the PHP project.

For a numerical tl;dr, see the [tally](#Tally).

## [General stance](https://eev.ee/blog/2012/04/09/php-a-fractal-of-bad-design/#stance)

> PHP is full of surprises: mysql_real_escape_string, E_ALL

"Surprise" is an inherently subjective position. I've no doubt that there are a lot of surprising things in PHP, some of them have been resolved, including both of the aforementioned blunders: whole mysql extension was removed from PHP, taking `mysql_real_escape_string()` with it; and `E_ALL` now includes `E_STRICT`.

> PHP is inconsistent: strpos, str_rot13

No major improvements.

> PHP requires boilerplate: error-checking around C API calls, ===

> PHP is flaky: ==, foreach ($foo as &$bar)

> PHP is opaque: no stack traces by default or for fatals, complex error reporting

## Core language
### [Philosophy](https://eev.ee/blog/2012/04/09/php-a-fractal-of-bad-design/#philosophy)
> PHP was originally designed explicitly for non-programmers (and, reading between the lines, non-programs); it has not well escaped its roots.

I believe that over the last years, PHP has proven itself to be a language suitable for professional development, as evidenced by a large number of high quality Composer packages that enable rapid development of good-quality code. I'm not getting into debates if there are better alternatives to PHP here.

> PHP is built to keep chugging along at all costs. When faced with either doing something nonsensical or aborting with an error, it will do something nonsensical.

While clearly there are a lot of places where PHP still uses the "garbage in, garbage out" approach, the theme of PHP changes over the last decades was being stricter. While all the changes in this area are way too numerous to list here, [strict types mode](https://www.php.net/manual/en/language.types.declarations.php#language.types.declarations.strict) is a major improvement.

> PHP takes vast amounts of inspiration from other languages, yet still manages to be incomprehensible to anyone who knows those languages.

PHP has added a lot of syntax borrowed from or inspired by other languages, including lambdas (`(x) => x * x`) and the spaceship operator (`<=>`). Can't say that these outpace the old, "incomprehensible" parts though. One criticism from this point has been resolved though:

> `(int)` looks like C, but `int` doesn’t exist.

You can now specify `int` as a type of parameters or properties.

> Weak typing (i.e., silent automatic conversion between strings/numbers/et al) is so complex that whatever minor programmer effort is saved is by no means worth it.

> Little new functionality is implemented as new syntax; most of it is done with functions or things that look like functions.

Lots of syntactic sugar was added.

> Some of the problems listed on this page do have first-party solutions—if you’re willing to pay Zend for fixes to their open-source programming language.

Too vague of an allegation to say what was meant here.

### [Operators](https://eev.ee/blog/2012/04/09/php-a-fractal-of-bad-design/#operators)


# Tally
...
