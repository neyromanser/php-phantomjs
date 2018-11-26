PHP PhantomJS
=============

PHP PhantomJS is a flexible PHP library to load pages through the PhantomJS 
headless browser and return the page response. It is handy for testing
websites that demand javascript support and also supports screen captures.

[Full Documentation](http://jonnnnyw.github.io/php-phantomjs/)

[![Total Downloads](https://poser.pugx.org/jonnyw/php-phantomjs/downloads.png)](https://packagist.org/packages/jonnyw/php-phantomjs) [![Latest Stable Version](https://poser.pugx.org/jonnyw/php-phantomjs/v/stable.png)](https://packagist.org/packages/jonnyw/php-phantomjs) [![Build Status](https://travis-ci.org/jonnnnyw/php-phantomjs.svg?branch=master)](https://travis-ci.org/jonnnnyw/php-phantomjs) [![Scrutinizer Quality Score](https://scrutinizer-ci.com/g/jonnnnyw/php-phantomjs/badges/quality-score.png?s=631d32fa1fbb9300eb84b9b52702c7ffeac046a1)](https://scrutinizer-ci.com/g/jonnnnyw/php-phantomjs/) [![Code Coverage](https://scrutinizer-ci.com/g/jonnnnyw/php-phantomjs/badges/coverage.png?s=893b5997da45448e32983b8568a39630b0b2d91b)](https://scrutinizer-ci.com/g/jonnnnyw/php-phantomjs/)

Feature List
---------------------

*  Load webpages through the PhantomJS headless browser
*  View detailed response data including page content, headers, status code etc.
*  Handle redirects
*  View javascript console errors
*  View detailed PhantomJS debug information
*  Save screen captures to local disk
*  Output web pages to PDF document
*  Set viewport size
*  Set fixed header and footer for PDF output
*  Define screen capture x, y, width and height parameters
*  Delay page rendering for a specified time
*  Delay page rendering until page resources are fully loaded
*  Execute PhantomJS with command line options
*  Easily build and run custom PhantomJS scripts

## Config

Add the following provider to providers part of config/app.php
```php
JonnyW\PhantomJs\PhantomJsServiceProvider::class,
```

and the following Facade to the aliases part
```php
'PhantomJs' => JonnyW\PhantomJs\Facade\PhantomJs::class,
```

and then you can run vendor:publish command for generating phantomjs config file
 ```bash
 $ php artisan vendor:publish --provider="JonnyW\PhantomJs\PhantomJsServiceProvider"
 ```

#### Now you can config your phantomjs client in ```config/phantomjs.php``` file

## Basic Usage
The following illustrates how to make a basic GET request and output the page content:

### On Load Finished
```php
// Tells the client to wait for all resources before rendering

$request = \PhantomJs::get('http://phantomjs.org/');

\PhantomJs::isLazy()->send($request);
```

```php
// you can use Facade or app make function to use phantomjs
// ex: app('phantomjs') or \PhantomJs

$request = \PhantomJs::get('http://phantomjs.org/');

$response = \PhantomJs::send($request);

if($response->getStatus() === 200) {

    // Dump the requested page content
    echo $response->getContent();
}
```

Saving a screen capture to local disk:
```php

$request = \PhantomJs::createImage('http://phantomjs.org/', 'GET');

$request->setOutputFile(public_path('file.jpg'));

$request->setViewportSize(800, 600);

$request->setCaptureDimensions(800, 600, 0, 0);

$response = \PhantomJs::send($request);

if($response->getStatus() === 200) {

    // Dump the requested page content
    echo $response->getContent();
}
```

Outputting a page as PDF:

```php
$request = \PhantomJs::createPdf('http://phantomjs.org/', 'GET');
$request->setOutputFile(public_path('document.pdf'));
$request->setFormat('A4');
$request->setOrientation('landscape');
$request->setMargin('1cm');

$response = \PhantomJs::send($request);

if($response->getStatus() === 200) {

    // Dump the requested page content
    echo $response->getContent();
}
```

## License
The MIT License (MIT)