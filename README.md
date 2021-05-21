# Validate date format in (PHP 5 >= 5.3.0, PHP 7, PHP 8)

```php
# Enable strict_types in PHP >= 7.0
//declare(strict_types=1);

/**
 * Correctly determine if date string is a valid date in that format.
 * Compatible with PHP >= 5.3.0, also valid in PHP 7 and PHP 8 with strict_types = 1.
 * @see https://3v4l.org/8knDn Output
 * @see https://www.php.net/manual/es/datetime.createfromformat.php#refsect1-datetime.createfromformat-parameters Parameters supported in the format
 * @see https://www.php.net/manual/es/function.checkdate.php#113205 based on this answer
 * @param string $date
 * @param string $format
 * @return bool
 */
function validateDate($date, $format = 'Y-m-d H:i:s')
{
    $d = DateTime::createFromFormat($format, (string)$date);
    return !(false === $d) && $d->format($format) === (string)$date;
}
```
In php 8 it would look like this:
```php
function validateDate(string $date, string $format = 'Y-m-d H:i:s'): bool
{
    $d = DateTime::createFromFormat($format, $date);
    return !(false === $d) && $d->format($format) === $date;
}
```

Example outputs:

```php

var_dump (validateDate ('2012-2-25 4:05:13')); # false, the correct is: 'Y-n-d G:i:s'
var_dump (validateDate ('2012-2-25 4:05:13', 'Y-n-d G:i:s')); # true
var_dump (validateDate ('2012-2-25', 'Y-m-d')); # false, the correct is: 'Y-n-d'
var_dump (validateDate ('2012-2-25', 'Y-n-d')); # true

print "-------------". PHP_EOL;
var_dump(validateDate("4", 'd')); # false, the correct is: 'j'
var_dump(validateDate("4", 'j')); # true
var_dump(validateDate("12", 'H')); # true
var_dump(validateDate("0", 'H')); # false, the correct is: 'G'
var_dump(validateDate("4", 'm')); # false, the correct is: 'n'
var_dump(validateDate("4", 'n')); # true

print "-------------". PHP_EOL;
var_dump(validateDate('2021-03-16T14:22:20-06:00', DATE_RFC3339)); # true
var_dump(validateDate("0", 'G')); #true
var_dump(validateDate("00", 'H')); # true
var_dump(validateDate('14:50', 'H:i')); # true
var_dump(validateDate('14', 'H')); # true
var_dump(validateDate('2012-02-28', 'Y-m-d')); # true
var_dump(validateDate('28/02/2012', 'd/m/Y')); # true
var_dump(validateDate('2012-02-28 12:12:12')); # true
var_dump(validateDate('2012-02-28T12:12:12+02:00', DATE_ATOM)); # true
print "-------------". PHP_EOL;
var_dump(validateDate('2012-02-30 12:12:12')); # false
var_dump(validateDate('30/02/2012', 'd/m/Y')); # false
var_dump(validateDate('14:77', 'H:i')); # false
print "-------------". PHP_EOL;
var_dump(validateDate('Mon, 27 Feb 2012 12:12:12 +0200', DATE_RSS)); # true
var_dump(validateDate('Tue, 27 Feb 2012 12:12:12 +0200', DATE_RSS)); # false
 ```
