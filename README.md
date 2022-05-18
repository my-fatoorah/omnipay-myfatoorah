# Omnipay: MyFatoorah

**MyFatoorah gateway integration for the Omnipay PHP payment processing library**

## Introduction

[![Build Status](https://scrutinizer-ci.com/g/my-fatoorah/omnipay-myfatoorah/badges/build.png?b=master)](https://scrutinizer-ci.com/g/my-fatoorah/omnipay-myfatoorah/build-status/master)
[![Scrutinizer Code Quality](https://scrutinizer-ci.com/g/my-fatoorah/omnipay-myfatoorah/badges/quality-score.png?b=master)](https://scrutinizer-ci.com/g/my-fatoorah/omnipay-myfatoorah/?branch=master)
[![Code Intelligence Status](https://scrutinizer-ci.com/g/my-fatoorah/omnipay-myfatoorah/badges/code-intelligence.svg?b=master)](https://scrutinizer-ci.com/code-intelligence)
[![Total Downloads](http://poser.pugx.org/myfatoorah/omnipay/downloads)](https://packagist.org/packages/myfatoorah/omnipay)

[Omnipay](https://github.com/thephpleague/omnipay) is a framework agnostic, multi-gateway payment
processing library for PHP 5.3+. This package implements [acapture](https://www.acapture.com/) support for Omnipay.

## Installation

To install you can [composer](http://getcomposer.org/) require the package;

```
$ composer require myfatoorah/omnipay
```

You can also include the package directly in the `composer.json` file
```
{
    "require": {
        "myfatoorah/omnipay": "dev-master"
    }
}
```
## Usage

### Creating a payment link

```php
use Omnipay\Omnipay;

$data                      = array();
$data['Amount']            = '50';
$data['OrderRef']          = 'orderId-123'; 
$data['Currency']          = 'KWD';
$data['returnUrl']         = 'http://websiteurl.com/callback.php';
$data['Card']['firstName'] = 'fname';
$data['Card']['lastName']  = 'lname';
$data['Card']['email']     = 'test@test.com';
//
// Do a purchase transaction on the gateway
$transaction               = $gateway->purchase($data)->send();
if ($transaction->isSuccessful()) {
    $invoiceId   = $transaction->getTransactionReference();
    echo "Invoice Id = " . $invoiceId . "<br>";
    $redirectUrl = $transaction->getRedirectUrl();
    echo "Redirect Url = <a href='$redirectUrl' target='_blank'>" . $redirectUrl . "</a><br>";
} else {
    echo $transaction->getMessage();
}
```
### Checking payment status

In the callback, Get Payment status for a specific Payment ID

```php
$callBackData = ['paymentId' => '100202113817903101'];
// or 
$callBackData = ['invoiceid' => '75896'];
$callback     = $gateway->completePurchase($callBackData)->send();
if ($callback->isSuccessful()) {
    echo "<pre>";
    print_r($callback->getPaymentStatus('orderId-123', '100202113817903101'));
} else {
    echo $callback->getMessage();
}
```
### Make a refund

Refund a specific Payment ID

```php
$refundData = ['paymentId' => '100202113817903101', 'Amount'=>1];
$refund     = $gateway->refund($refundData)->send();
if ($refund->isSuccessful()) {
    echo "<pre>";
    print_r($refund->getRefundInfo());
} else {
    echo $refund->getMessage();
}

```
