## Postmen SDK PHP
PHP for Postmen API.
This extension helps developers to integrate with Postmen easily.


## About Postmen


### Changes


## Installation
#### Manual installation

1. Download this repository as zip and extract where you desire OR `git clone` it.
2. Reference desired API class from withthin your PHP source. (very important to also include the `Handler.php` file)
```php
require('.../path/to/repository/src/Postmen/Postmen.php');

use Postmen\Postmen;

$key = 'your_api_key';
$region = 'us-west';

$api = new Postmen($key, $region);
```
#### Using Composer

0. If you don't yet have Composer, to download and Install Composer, open [here](https://getcomposer.org/download/)
1. You have 2 options to download the Postmen PHP SDK

Run the following command to require Postmen PHP SDK
```
composer require postmen/sdk-php
```
OR `git pull` this repo and run the following command
```
composer install
```
2. Autoload the `postmen-php-sdk` package.

```php
$loader = require __DIR__ . '/vendor/autoload.php';

use Postmen\Postmen;

$key = 'your_api_key';
$region = 'us-west';

$api = new Postmen($key, $region);
```

#### Requirements

Minimum PHP version required to use this SDK is `5.2` for just using the SDK. For development PHP `5.6` is required (to run automated tests).

Tested on PHP 5.3, 5.4, 5.5, 5.6.

## Usage

#### Rates

##### Calculate rates
```php
$item = array (
	'description' => 'PS4',
	'origin_country' => 'JPN',
	'quantity' => 2,
	'price' => array (
		'amount' => 50,
		'currency' => 'JPY',
	),
	'weight' => array (
		'value' => 0.59999999999999998,
		'unit' => 'kg',
	),
	'sku' => 'PS4-2015',
);
$sender = array (
	'contact_name' => 'Yin Ting Wong',
	'street1' => 'Flat A, 29/F, Block 17 Laguna Verde',
	'city' => 'Hung Hom',
	'state' => 'Kowloon',
	'country' => 'HKG',
	'phone' => '96679797',
	'email' => 'test@test.test',
	'type' => 'residential',
);
$receiver = array (
	'contact_name' => 'Mike Carunchia',
	'street1' => '9504 W Smith ST',
	'city' => 'Yorktown',
	'state' => 'Indiana',
	'postal_code' => '47396',
	'country' => 'USA',
	'phone' => '7657168649',
	'email' => 'test@test.test',
	'type' => 'residential',
);
$payload = array (
	'async' => false,
	'shipper_accounts' => array (
		0 => array (
			'id' => '00000000-0000-0000-0000-000000000000',
		),
	),
	'shipment' => array (
		'parcels' => array (
			0 => array (
				'box_type' => 'custom',
				'weight' => array (
					'value' => 0.5,
					'unit' => 'kg',
				),
				'dimension' => array (
					'width' => 20,
					'height' => 10,
					'depth' => 10,
					'unit' => 'cm',
				),
				'items' => array (
					0 => $item
				),
			),
		),
		'ship_from' => $sender,
		'ship_to' => $receiver,	
	),
	'is_document' => false
);

$api = new Postmen($key, $region);
$result = $api->create('rates', $payload);
```
##### List all rates
```php
$api = new Postmen($key, $region);
$results = $api->get('rates');
```
##### Retreive a rate
```php
$api = new Postmen($key, $region);
$result = $api->get('rates', $id);
```
#### Labels
##### create label
```php
$api = new Postmen($key, $region]);
$parcel = array(
	'box_type' => 'custom',
	'weight' => array (
		'value' => 1.5,
		'unit' => 'kg',
	),
	'dimension' => array (
		'width' => 20,
		'height' => 30,
		'depth' => 40,
		'unit' => 'cm',
	),
	'items' => array (
		0 => array (
			'description' => 'Food Bar',
			'origin_country' => 'USA',
			'quantity' => 2,
			'price' => array (
				'amount' => 50,
				'currency' => 'USD',
			),
			'weight' => array (
				'value' => 0.59999999999999998,
				'unit' => 'kg',
			),
			'sku' => 'Epic_Food_Bar',
			'hs_code' => '11111111',
		),
	),
);
$sender = array (
	'contact_name' => 'Nottingham Inc.',
	'company_name' => 'Nottingham Inc.',
	'street1' => '2511 S. Main St.',
	'city' => 'Grove',
	'state' => 'OK',
	'postal_code' => '74344',
	'country' => 'USA',
	'phone' => '1-403-504-5496',
	'email' => 'test@test.com',
	'type' => 'business',
);
$receiver = array (
	'contact_name' => 'Rick McLeod (RM Consulting)',
	'street1' => '71 Terrace Crescent NE',
	'street2' => 'This is the second streeet',
	'city' => 'Medicine Hat',
	'state' => 'Alberta',
	'postal_code' => 'T1C1Z9',
	'country' => 'CAN',
	'phone' => '1-403-504-5496',
	'email' => 'test@test.test',
	'type' => 'residential',
);
$payload = array (
	'is_document' => false,
	'async' => false,
	'return_shipment' => false,
	'paper_size' => '4x8',
	'service_type' => 'dhl_express_0900',
	'customs' => array (
		'billing' => array (
			'paid_by' => 'shipper',
			'method' => array (
				'account_number' => '950000002',
				'type' => 'account',
			),
		),
		'purpose' => 'gift',
	),
	'shipper_account' => array (
		'id' => '00000000-0000-0000-0000-000000000000',
	),
	'shipment' => array (
		'parcels' => array (
			0 => $parcel
		),
		'ship_from' => $sender,
		'ship_to' => $receiver 
	),
);

$api->create('labels', $payload));
```
##### List all labels
```php
$api = new Postmen($key, $region);
$results = $api->get('labels');
```
##### Retreive a label
```php
$api = new Postmen($key, $region);
$result = $api->get('labels', $id);
```

#### Manifests
##### Create a manifest
```php
$api = new Postmen($key, $region);
$payload = array (
	'shipper_account' => array (
        	'id' => '00000000-0000-0000-0000-000000000000',
	)
);
$result = $api->create('manifests', $payload);
```
##### List all manifests
```php
$api = new Postmen($key, $region);
$results = $api->get('manifests');
```
##### Retreive a manifest
```php
$api = new Postmen($key, $region);
$result = $api->get('manifests', $id);
```
#### Cancel Labels
##### Cancel a label
```php
$api = new Postmen($key, $region);
$result = $api->cancel($id);
```
##### List all cancel labels
```php
$api = new Postmen($key, $region);
$results = $api->get('cancel-labels');
```
##### Retreive a cancel label
```php
$api = new Postmen($key, $region);
$result = $api->get('cancel-labels', $id);
```
#### Options
##### Custom endpoint

If you need to use different endpoint then one generated using `region` value, it is possible to set it during construction by setting `endpoint` value. Standard `$sandbox` argument field in constructor function can be `null`, `undefined` or just an empty string, but must be present as this argument is not optional.

```php
$rates = new Postmen($key, "", array("endpoint" => "https://api.examples.com"));
```

##### Safe mode

Initiating API object with `safe` option set to true will prevent SDK from throwing an exception in case of an error. Instead called method will return `undefined` value and set store the occured exception object in its `$_error` attribute.

```php
$api = new Postmen($key, $region);
$result = $api->get('rates', $id, array("safe" => true));
$error = $api->getError();
```

##### Using proxy

Proxy is defined in an array object containing `host`, `port`, `username` and `password` informations. Only `host` is required field.
We can set such proxy object in the constructor to be used by default on any call.

```php
$proxy = array(
	"host" => "someproxy.com",
	"port" => 31280,
	"username" => "username",
	"password" => "password"
);
$api = new Postmen($key, $region, array('proxy' => $proxy));
$result = $api->get('rates', $id);
```

Or either we can set it as optional parameter to a particular call to be used only for that one time call. This is usefull when we do not need to use proxy by default or when we need to temporary overwrite the default proxy.

```php
$proxy = array(
	"host" => "someproxy.com",
	"port" => 31280,
	"username" => "username",
	"password" => "password"
);
$api = new Postmen($key, $region);
$result = $api->get('rates', $id, array('proxy' => $proxy));
```

##### Raw JSON response
A raw JSON string response returned fromfined in an array object containing `host`, `port`, `username`, example as follows:
```php
$api = new Postmen($key, $region);
$json_string = $api->get('rates', $id, array('raw' => true));
```

##### Automatic retry if exception occurs
If retry option is activated, in case of `retryable` error from Postmen API, this SDK will automatically try to make a call again, at most 5 times, each time doubling waiting time which start at 1s.

```php
$api = new Postmen($key, $region);
$json_string = $api->get('rates', $id, array('retry' => true));
```
##### Rate limiting
Number of authorized requests per quantity of time is limited, this SDK can either throw an exception with error code `429` either wait number of seconds needed to perform the call and this depends on `rate` optional parameter.

Rate by default is set to TRUE, so if you want to avoid long running scripts and prefer to catch an exception and handle it yourself just manually set it to FALSE.

```php
$api = new Postmen($key, $region);
$json_string = $api->get('rates', $id, array('rate' => true));
```


## Testing
If you contribute it is recommended to run automated test before you pull request your changes.

`phpunit --bootstrap tests/bootstrap.php tests/Postmen.php`

## The License (MIT)
Released under the MIT license. See the LICENSE file for the complete wording.

## Contributor
- Sunny Chow - [view contributions](https://github.com/postmen/sdk-php/commits?author=sunnychow)
- Marek Narozniak - [view contributions](https://github.com/postmen/sdk-php/commits?author=marekyggdrasil)

