# Introduction to PHP Extensions

PHP extensions:
- additional functions & classes
- written in C
- compiled or shared objects
- bundled with PHP
- distributed through PECL or distributed separately

Why write an extension?
- Wrap an existing library
    - DB drivers
    - Image processing
    - Image recognition
- Not everything can be done in userland
    - keep information between requests: persistent connections, PHP objects
    - Peeking into internals
- Speed
    - PHP is fast, but can be faster: Twig extension for example, a few functions were rewritten
    - C/C++ much faster

Why not?
- Takes more time to write
- Takes more time debug
- Takes more time maintain

Ramer-Douglas-Peucker algorithm: simplify polygons
- PHP function: 45s
- C extension: 0.37s

https://derickrethans.nl/talks/phpext-confoo24