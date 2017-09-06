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
        "gstearmit/omnipay-molpayid": "~2.0"
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
1. PHPUNIT 
   $ phpunit --bootstrap  src/Message/PurchaseRequest.php  tests/Message/PurchaseRequestTest.php
   $ phpunit --coverage-text
   $ phpunit 

2. $ composer update --verbose

3. composer dump-autoload

4. Bao cao tien do kiem tra nhanh
   $ phpunit --testdox

    Omnipay\MOLPayID\Gateway
     [x] Purchase
     [x] Complete purchase success
     [x] Complete purchase invalid s key
     [x] Complete purchase error
     [x] Get name not empty
     [x] Get short name not empty
     [x] Get default parameters returns array
     [x] Default parameters have matching methods
     [x] Test mode
     [x] Currency
     [x] Supports authorize
     [x] Supports complete authorize
     [x] Supports capture
     [x] Supports purchase
     [x] Supports complete purchase
     [x] Supports refund
     [x] Supports void
     [x] Supports create card
     [x] Supports delete card
     [x] Supports update card
     [x] Authorize parameters
     [x] Complete authorize parameters
     [x] Capture parameters
     [x] Purchase parameters
     [x] Complete purchase parameters
     [x] Refund parameters
     [x] Void parameters
     [x] Create card parameters
     [x] Delete card parameters
     [x] Update card parameters
    
    Omnipay\MOLPayID\Message\CompletePurchaseRequest
     [ ] Get data
     [ ] Send success
     [ ] Send pending
    
    Omnipay\MOLPayID\Message\CompletePurchaseResponse
     [x] Complete purchase success
     [x] Complete purchase pending
    
    Omnipay\MOLPayID\Message\PurchaseRequest
     [x] Get data
     [x] Send success
    
    Omnipay\MOLPayID\Message\PurchaseResponse
     [x] Purchase success

5. $ phpunit --debug  : Xuất thông tin gỡ lỗi như tên của một bài kiểm tra khi bắt đầu thực hiện


    Time: 1.09 seconds, Memory: 19.00MB
    
    There were 3 errors:
    
    1) Omnipay\MOLPayID\Message\CompletePurchaseRequestTest::testGetData
    Omnipay\Common\Exception\InvalidResponseException: Invalid security key
    
    /Applications/XAMPP/xamppfiles/htdocs/omnipay-molpayid/src/Message/CompletePurchaseRequest.php:203
    /Applications/XAMPP/xamppfiles/htdocs/omnipay-molpayid/tests/Message/CompletePurchaseRequestTest.php:29
    
    2) Omnipay\MOLPayID\Message\CompletePurchaseRequestTest::testSendSuccess
    Omnipay\Common\Exception\InvalidResponseException: Invalid security key
    
    /Applications/XAMPP/xamppfiles/htdocs/omnipay-molpayid/src/Message/CompletePurchaseRequest.php:203
    /Applications/XAMPP/xamppfiles/htdocs/omnipay-molpayid/vendor/omnipay/common/src/Omnipay/Common/Message/AbstractRequest.php:676
    /Applications/XAMPP/xamppfiles/htdocs/omnipay-molpayid/tests/Message/CompletePurchaseRequestTest.php:40
    
    3) Omnipay\MOLPayID\Message\CompletePurchaseRequestTest::testSendPending
    Omnipay\Common\Exception\InvalidResponseException: Invalid security key
    
    /Applications/XAMPP/xamppfiles/htdocs/omnipay-molpayid/src/Message/CompletePurchaseRequest.php:203
    /Applications/XAMPP/xamppfiles/htdocs/omnipay-molpayid/vendor/omnipay/common/src/Omnipay/Common/Message/AbstractRequest.php:676
    /Applications/XAMPP/xamppfiles/htdocs/omnipay-molpayid/tests/Message/CompletePurchaseRequestTest.php:54
    
    ERRORS!
    Tests: 38, Assertions: 87, Errors: 3
    
6. 
