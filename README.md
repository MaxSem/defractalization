# Is PHP still a fractal of bad design?

Eevee's famous article [PHP: a fractal of bad design](https://eev.ee/blog/2012/04/09/php-a-fractal-of-bad-design/) is pretty influential because it hits pretty close to home. But it was written in 2012 - did PHP get any better since then? I'm writing this in early 2021, and PHP has evolved quite a bit over these 9 years, going from version 5.4 to 8.0. My article concentrates on analyzing whether PHP has improved, not on debating the original article. Full disclosure: I'm a (very minor) contributor to the PHP project.

For a numerical tl;dr, see the [tally](#Tally).

## [General stance](https://eev.ee/blog/2012/04/09/php-a-fractal-of-bad-design/#stance)

> PHP is full of surprises: mysql_real_escape_string, E_ALL

"Surprise" is an inherently subjective word. While I've no doubt that there are a lot of surprising things in PHP, some of them have been resolved, including both of the aforementioned blunders: whole mysql extension was removed from PHP, taking `mysql_real_escape_string()` with it; and `E_ALL` now includes `E_STRICT`.

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

PHP has added a lot of syntax borrowed from or inspired by other languages, including lambdas (`(x) => x * x`) and the spaceship operator (`<=>`). Can't say that these outpace the old, "incomprehensible" parts though. However, one criticism from this point has been resolved:

> `(int)` looks like C, but `int` doesn’t exist.

You can now specify `int` (and other basic types) as a type of parameters or properties.

> Weak typing (i.e., silent automatic conversion between strings/numbers/et al) is so complex that whatever minor programmer effort is saved is by no means worth it.

PHP is still a weakly typed language, however with types enforced in a lot of places and [some strictening of type coercion](https://www.php.net/manual/en/migration80.incompatible.php#migration80.incompatible.core.string-number-comparision), it's definitely getting better.

> Little new functionality is implemented as new syntax; most of it is done with functions or things that look like functions.

Lots of syntactic sugar was added.

> Some of the problems listed on this page do have first-party solutions—if you’re willing to pay Zend for fixes to their open-source programming language.

Too vague of an allegation to say what was meant here.

### [Operators](https://eev.ee/blog/2012/04/09/php-a-fractal-of-bad-design/#operators)

> `==` is useless.

While it has gotten somewhat more strict, I'd say this is mostly unresolved.

> Comparison isn’t much better

Same, probably even less improved.

> Despite the craziness above, and the explicit rejection of Perl’s pairs of string and numeric operators, PHP does not overload `+`. `+` is always addition, and `.` is always concatenation.

Yes, but PHP does not have operator overloading for userland code at all. This is not an inherent problem, as many shiny new languages (like Go) intentionally don't have operator overloading.

> The `[]` indexing operator can also be spelled `{}`.

Not anymore. [Deprecated in 7.4 and completely outlawed in 8.0](https://3v4l.org/MCHum).

> `[]` can be used on any variable, not just strings and arrays. It returns null and issues no warning.

[Deprecated in 7.4](https://3v4l.org/BSHrZ).

> `[]` cannot slice; it only retrieves individual elements.

Author's preference.

> `foo()[0]` is a syntax error. (Fixed in PHP 5.4.)

Fixed? Oh, cool!

> Unlike (literally!) every other language with a similar operator, `?:` is *left* associative.

Now combining ternary operators [requires parentheses](https://3v4l.org/t8io2), which is arguably even better since it removes any possibility of wrong interpretation.

### [Variables](https://eev.ee/blog/2012/04/09/php-a-fractal-of-bad-design/#variables)

> There is no way to declare a variable. Variables that don’t exist are created with a null value when first used.

Still the case.

> Global variables need a global declaration before they can be used.

Basically, Eevee has a personal preference of Python scoping rules here.

> There are no references. What PHP calls references are really aliases

Same as above, another case of personal preference.

> “Referenceness” infects a variable unlike anything else in the language

Still the case.

> Okay, I lied. There are “SPL types” which also infect variables

???

> A reference can be taken to a key that doesn’t exist within an undefined variable (which becomes an array)

Still the case.

> Constants are defined by a function call taking a string; before that, they don’t exist.

Some improvements: you can define class constants and constants in global scope (`const FOO = 'bar';`), but the latter doesn't even work in conditionals.

> Variable names are case-sensitive. Function and class names are not.

Still the case.

### [Constructs](https://eev.ee/blog/2012/04/09/php-a-fractal-of-bad-design/#constructs)

> `array()` and a few dozen similar constructs are not functions. `array` on its own means nothing, `$func = "array"; $func();` doesn’t work.

With the introduction of special syntax to replace constructs like `array()` and `list()`, they've gotten much rarer encounter in the wild. Also, you can now use `array` to specify parameter or property type.

> Array unpacking can be done with the `list($a, $b) = ...` operation. `list()` is function-like syntax just like `array`.

As mentioned above, a pseudo-function is not needed anymore: `[$a, $b] = ...`

> `(int)` is obviously designed to look like C, but it’s a single token; there’s nothing called `int` in the language. Try it: not only does `var_dump(int)` not work, it throws a parse error because the argument looks like the cast operator.

`int` and friends can now be used as type specifiers, though Python-like approach where type name is an expression is still not supported.

> `(integer)` is a synonym for `(int)`. There’s also `(bool)`/`(boolean)` and `(float)`/`(double)`/`(real)`.

With the exception of `(real)` cast that was so rare that it was removed outright, the remaining type aliases are still present.

> There’s an `(array)` operator for casting to array and an `(object)` for casting to object.

Still the case.

> `include()` and friends are basically C’s `#include`: they dump another source file into yours. There is no module system, even for PHP code.

Still the case, although PSR-4 autoloaders mostly hide this from the programmer.

> There’s no such thing as a nested or locally-scoped function or class.

Still the case.

> Appending to an array is done with `$foo[] = $bar`.

Not clear why it's a problem.

> `echo` is a statement-y kind of thing, not a function.

Still the case, though not clear why it is a problem.

> `empty($var)` is so extremely not-a-function that anything but a variable, e.g. `empty($var || $var2)`, is a parse error. Why on Earth does the parser need to know about `empty`? (Fixed in 5.5.)

Fixed, whee.

> There’s redundant syntax for blocks: `if (...): ... endif;`, etc.

Still the case.

### [Error handling](https://eev.ee/blog/2012/04/09/php-a-fractal-of-bad-design/#error-handling)

> PHP’s one unique operator is `@` (actually borrowed from DOS), which *silences* errors.

As more errors/warnings are converted to exceptions, this operator is less needed.

> PHP errors don’t provide stack traces. You have to install a handler to generate them. (But you can’t for fatal errors—see below.)

Still not done.

> PHP parse errors generally just spew the parse state and nothing more, making a forgotten quote terrible to debug.

While parse errors have gotten a bit better, much is still left to be desired. Also see below.

> PHP’s parser refers to e.g. `::` internally as `T_PAAMAYIM_NEKUDOTAYIM`, and the `<<` operator as `T_SL`. I say “internally”, but as above, this is what’s shown to the programmer when `::` or `<<` appears in the wrong place.

Resolved, internal token names are now not included in errors.

> Most error handling is in the form of printing a line to a server log nobody reads and carrying on.

Improvement, lots of errors are now exceptions though there's still a long road to go.

> `E_STRICT` is a thing, but it doesn’t seem to actually prevent much and there’s no documentation on what it actually does.

Error levels are still badly documented.

> `E_ALL` includes all error categories—except `E_STRICT`. (Fixed in 5.4.)

Fixed, whee.

> Weirdly inconsistent about what’s allowed and what isn’t. I don’t know how `E_STRICT` applies here, but these things are okay: \[...] And these things are not: \[...]

Too opinionated to judge, but the rule of the thumb is that parse errors are fatal while runtime errors are fatal only if there's no way to continue (e.g. call to a nonexistent function is game over while reading a nonexistent variable can produce a `null` and continue with a warning). The error handling has gotten stricter over time, but still could use more; not breaking existing code bases too much is a limiting factor here.

> The \__toString method can’t throw exceptions. If you try, PHP will… er, throw an exception. (Actually a fatal error, which would be passable, except…)

Fixed [in 7.4](https://www.php.net/manual/en/migration74.new-features.php).

# Tally
...
