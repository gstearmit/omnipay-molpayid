# Omnipay: MOLPayID

** MOLPayID  driver for the Omnipay PHP payment processing library**

[![Build Status](https://travis-ci.org/leesiongchan/omnipay-MOLPayID.png?branch=master)](https://travis-ci.org/leesiongchan/omnipay-MOLPayID)
[![Latest Stable Version](https://poser.pugx.org/leesiongchan/omnipay-MOLPayID/v/stable)](https://packagist.org/packages/leesiongchan/omnipay-MOLPayID)
[![Total Downloads](https://poser.pugx.org/leesiongchan/omnipay-MOLPayID/downloads)](https://packagist.org/packages/leesiongchan/omnipay-MOLPayID)
[![Latest Unstable Version](https://poser.pugx.org/leesiongchan/omnipay-MOLPayID/v/unstable)](https://packagist.org/packages/leesiongchan/omnipay-MOLPayID)
[![License](https://poser.pugx.org/leesiongchan/omnipay-MOLPayID/license)](https://packagist.org/packages/leesiongchan/omnipay-MOLPayID)

[Omnipay](https://github.com/thephpleague/omnipay) is a framework agnostic, multi-gateway payment
processing library for PHP 5.3+. This package implements MOLPayID support for Omnipay.

[MOLPayID](http://www.MOLPayID.com) is a payment gateway offering from MOLPayID Sdn Bhd. This package follows the **MOLPayID API Specification (Version 12.1: Updated on 12 April 2015)**.

## Installation

Omnipay is installed via [Composer](http://getcomposer.org/). To install, simply add it
to your `composer.json` file:

```json
{
    "require": {
        "gstearmit/omnipay-molpay-id": "~2.0"
    }
}
```
 or 
 
```json
{
    "require": {
        "weshop/omnipaymolpayid": "~2.0"
    }
}
```

And run composer to update your dependencies:

    $ curl -s http://getcomposer.org/installer | php
    $ php composer.phar update

## Basic Usage

The following gateways are provided by this package:

* MOLPayID (MOLPayID Payment)

For general usage instructions, please see the main [Omnipay](https://github.com/thephpleague/omnipay)
repository.

## Example

### Create a purchase request

The example below explains how you can create a purchase request then send it.

```php
$gateway = Omnipay::create('MOLPayID');

$gateway->setCurrency('RP');
$gateway->setEnableIPN(true); // Optional
$gateway->setLocale('en'); // Optional
$gateway->setMerchantId('test1234');
$gateway->setVerifyKey('abcdefg');

$options = [
    'amount' => '10.00',
    'card' => new CreditCard(array(
        'country' => 'ID',
        'email' => 'abc@example.com',
        'name' => 'Lee Siong Chan',
        'phone' => '0123456789',
    )),
    'description' => 'Test Payment',
    'transactionId' => '20160331082207680000',
    'paymentMethod' => 'credit', // Optional
];

$response = $gateway->purchase($options)->send();

// Get the MOLPayID payment URL (https://www.onlinepayment.com.my/MOLPayID/pay/...)
$redirectUrl = $response->getRedirectUrl(); 
```

### Complete a purchase request

When the user submit the payment form, the gateway will redirect you to the return URL that you have specified in MOLPayID. The code below gives an example how to handle the server feedback answer.

```php
$response = $gateway->completePurchase($options)->send();

if ($response->isSuccessful()) {
    // Do something
    echo $response->getTransactionReference();
} elseif ($response->isPending()) {
    // Do something
} else {
    // Error
}
```

## Out Of Scope

Omnipay does not cover recurring payments or billing agreements, and so those features are not included in this package. Extensions to this gateway are always welcome. 

--------------------
phpunit --bootstrap  src/Message/PurchaseRequest.php  tests/Message/PurchaseRequestTest.php



