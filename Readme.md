# PayWay Payment Gateway - Rest API - 2022

This package will allow you to make payment by navigating to payment gateway website and verify transaction made by Credit Card, Debit Card.

2 Steps will be called make payment. INITIATE PAYMENT and VERIFY PAYMENT. Steps are given below in the package.

You can install the sdk via composer

```bash
composer require rohitrajv5/payway
```


INITIATE PAYMENT

```php

require __DIR__.'/vendor/autoload.php';

use PayWay\init\IgfsCgInit;

// Call API with your client and get a response for your call
$init = new IgfsCgInit();
//$init->disableCheckSSLCert();
$init->serverURL="https://paymentgateway.it/IGFS_CG_SERVICES/services";
$init->timeout=30000;
$init->tid="TID-CODE1234";
$init->kSig= "0000000000000000000000000000";
$init->shopID="8384738437892728472";
 $init->shopUserRef="customer-email";
$init->trType="AUTH";
$init->currencyCode="EUR";
 $init->amount=4400; //Amount without comma, 1,00EUR will be 100
$init->langID="IT"; //Language iso code
$init->notifyURL="http://localhost/my-thankyou-page.php";           
$init->errorURL = "http://localhost/failure.php";


if(!$init->execute()){
echo $init->rc; // This statement will return error if any configuration is incorrect
}else{
 header("location: ".$init->redirectURL); // User will navigate to payway payment gateway from here
}

 ```

 REMEMBER !

 $init will return paymentID. Save this payment id. It will be use to verify transaction. Success/Failure

```
$init->paymentID // Payment id of current transaction

```

VERIFY PAYMENT

```php

// Call API with your client and get a response for your call
$init = new IgfsCgVerify();
//$init->disableCheckSSLCert();
$init->serverURL="https://paymentgateway.it/IGFS_CG_SERVICES/services";
$init->timeout=30000;
$init->tid="TID-CODE1234";
$init->kSig= "0000000000000000000000000000";
$init->langID="IT"; //Language iso code
$init->shopID=$shopID;
 $init->shopUserRef="customer-email";
$init->trType="AUTH";           
$init->paymentID=YOUR_PAYMENT_ID_GENERATED_IN_INITIALIZATION;           

if(!$init->execute()){
  return ['success'=>false,'message'=>$init->rc];  
}

```

RESPONSE 

You will get ``` $init->tranID ```  and ``` $init->errorDesc ``` in response. It transaction is successfull you will giet TRANSAZIONE OK message.


Enjoy :)