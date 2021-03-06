[![Latest Stable Version](https://poser.pugx.org/bitolaco/geoipcheck/v/stable)](https://packagist.org/packages/bitolaco/geoipcheck) 
[![Total Downloads](https://poser.pugx.org/bitolaco/geoipcheck/downloads)](https://packagist.org/packages/bitolaco/geoipcheck) 
[![Latest Unstable Version](https://poser.pugx.org/bitolaco/geoipcheck/v/unstable)](https://packagist.org/packages/bitolaco/geoipcheck) 
[![License](https://poser.pugx.org/bitolaco/geoipcheck/license)](https://packagist.org/packages/bitolaco/geoipcheck)

This is simple function which prints different strings depending on the
location of the HTTP request.

It makes use of the MaxMind GeoIP API for PHP.
See [https://github.com/maxmind/geoip-api-php](github.com/maxmind/geoip-api-php)
for more details about the API.

## Requirements

You must have Composer installed and/or downloaded. Visit [getcomposer.org](http://getcomposer.org) for more info.

You must also install the GeoIP City database, since we can't package it with the code due to license restrictions.
Your Linux distro will likely have packages for the PHP geo-ip extension.
If you're using Ubuntu, it's as simple as:

    sudo apt-get install php5-geoip

Have instructions for another system? Create a pull request and we'll merge it.

PHP 5.3 or newer is also required, primarily due to the reliance on anonymous functions.

## Installation

The MaxMind API was installed here via Composer, and is included in this package.
If you're already using composer, change the require line in example.php
to match the location of your Composer autoload file.

Make sure it's a requirement in your composer.json file.

    {
      "require": {
        "bitolaco/geoipcheck": "~0.1"
      }
    }

Make sure your php includes the file:

    require_once '/path/to/composer/autoload.php';

## Usage

Usage is very simple.

    $geoIp = new GeoIpCheck();

If your using a MaxMind database which is not in one of the following locations, you'll need to enter the path to the
database when initializing the object. The script automatically looks for the file in these locations:

    /usr/local/share/GeoIP/GeoIP.dat
    /usr/local/share/GeoIP/GeoLiteCity.dat
    /usr/local/share/GeoIP/GeoIPCity.dat
    /usr/share/GeoIP/GeoIP.dat
    /usr/share/GeoIP/GeoLiteCity.dat
    /usr/share/GeoIP/GeoIPCity.dat

If you're using a custom location, simply specify it like this:

    $geoIp = new GeoIpCheck('/path/to/geoip/database.dat');

If you want to override the IP address (i.e. not use the IP of the request, simple change the first line to:

    $geoIp = new GeoIpCheck();
    $geoIp->overrideRequestIp('<any valid ipv4 address>');

Then, to run a check, specify both the search type and the value to match.

    $geoIp->check(
        'Name of city/cities/regions/etc.',
        'Type of search',
        function() { /* Callback to execute if the check is true.  */ },
        function() { /* Callback to execute if the check is false. */ }
    );

Valid search types include:

    country_code, country_code3, city, latitude, longitude, area_code, metro_code, region, postal_code, dma_code, continent_code

Valid search values, the first argument, can be a string, a comma separated string,
or an array. All these would be valid.

    $type = 'Boston';
    $type = 'Boston,Cambridge';
    $type = array('Boston', 'Cambridge');

The whole example, if you wanted to check if the current visitor was from Boston or Cambridge:

    $geoIp = new GeoIpCheck();
    $geoIp->check(
        'Boston,Cambridge',
        'city',
        function() { echo "You are not from Boston or Cambridge"; },
        function() { echo "You are from Boston or Cambridge"; }
    );

If the details of the last request are stored in the `GeoIpCheck::$lastResult` variable. So anywhere in your script,
you could access data such as:

    $geoIp->lastRequest->city;
    $geoIp->lastRequest->country_code;
    $geoIp->lastRequest->area_code;
    // And so forth...

## Full Example

You can see an example in the `example/index.php` file. It doesn't have any external requirements other than the code
in this repository, so you can run it using the built-in PHP web-server.

## To Do

- Add basic unit testing.
- Cache results in memory (memcached, apc, etc.), if available.
- Cache entire database in memory, if available.

## Suggestions

This was built for a client, and you're seeing the vanilla version here.

Have any suggestions for features? Bugs? Comments? Create an issue in the issue tracker.

