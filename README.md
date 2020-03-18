# ATH Móvil Payment Button - Javascript Integration and Services

## Introduction
ATH Móvil's Javascript integration provides a simple, secure and fast checkout experience to customers paying on your website. After integrating our Payment Button on your website, you will be able to receive instant payments from more than a million ATH Móvil users.

## Prerequisites
Before you begin, please review the following prerequisites:

* An active ATH Móvil Business account is required to continue.
Note: *To sign up, download "ATH Móvil Business"x on the App Store if you have an iOS device or on the Play Store if you have an Android device.*

* Your ATH Móvil Business account needs to have a registered, verified and active ATH® card.

* Have the public and private API keys of your Business account at hand. Note: *You can view your API keys on the settings section of the ATH Móvil Business application for iOS or Android.*

If you need help signing up, adding a card or have any other question please refer to https://athmovilbusiness.com/preguntas or contact our support team at (787) 773-5466. For technical support please complete the following form:  https://forms.gle/ZSeL8DtxVNP2K2iDA.

## Installation
Before getting started you will need to add references to both jQuery and ATH Móvil's javascript on your project's HTML checkout file.
```javascript
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>
```
```javascript
<script src="https://www.athmovil.com/api/js/v2/athmovilV2.js"></script>
```
*Notes:*
* *ATH Móvil's javascript must be referenced at the bottom of the document since some variables need to be previously initialized.*
* *jQuery can be referenced on any location. Local versions can also be used. Version 3.3.1 or higher is required.*

## Usage
To integrate ATH Móvil’s Payment Button on your website follow these steps:

### HTML
Add the “Pay with ATH Móvil” button to your checkout view.
```html
  <div id="ATHMovil_Checkout_Button"></div>
```
* Button width is set to fill its parent.
* Button height is proportional to the width.

---

### Javascript
Configure the payment values using the javascript script below on your checkout view.
```javascript
<script type="text/javascript">

    ATHM_Checkout = {

        env: 'sandbox',
        publicToken: 'sandboxtoken01875617264',

        timeout: 600,

        theme: 'btn',
        lang: 'en',

        total: 1.00,
        tax: 1.00,
        subtotal: 1.00,

        metadata1: 'metadata1 test',
        metadata2: 'metadata2 test',

        items: [
            {
                "name":"First Item",
                "description":"This is a description.",
                "quantity":"1",
                "price":"1.00",
                "tax":"1.00",
                "metadata":"metadata test"
            },
            {
                "name":"Second Item",
                "description":"This is another description.",
                "quantity":"1",
                "price":"1.00",
                "tax":"1.00",
                "metadata":"metadata test"
            }
        ],
	...
	</script>
```
* Details:

| Variable  | Data Type | Required | Values | Description |
| ------------- |:-------------:|:-----:| ------------- |  ------------- |
| `env` | String  | Yes | `sandbox` or  `production` | Determines the environment to be used for the payment. Use `sandbox` for simulated payment responses or `production` for real payments. |
| `publicToken` | String | Yes | Business account public token. | Determines the Business account where the payment will be sent to. |
| `timeout` | Number | No | Number between `120` and `600`. | Expires the payment process if the payment hasn't been completed by the user after the provided amount of time (in seconds). Countdown starts immediately after the user presses the Payment Button. Default value is set to 600 seconds (10 mins). |
| `theme` | String | Yes | `btn`, `btn-dark` or `btn-light`. | Determines the colors of the “Pay with ATH Móvil” button that is displayed on your view. |
| `lang` | String | Yes | `en` for english or `es` for spanish. | Determines the language of the “Pay with ATH Móvil” button and the payment process.|
| `total` | Number | Yes | From `1.00` to `1500.00`. |  Total amount to be paid by the end user. |
| `tax` | Number | No || Optional  variable to display the payment tax (if applicable). |
| `subtotal` | Number | No || Optional variable to display the payment subtotal (if applicable). |
| `metadata1` | String | No || Optional variable to attach key-value data to the payment object. |
| `metadata2` | String | No || Optional variable to attach key-value data to the payment object. |
| `items` | Array | Yes || Optional variable to display the items that the user is purchasing on ATH Móvil's payment screen. *`metadata` and `tax` are required but they can be set as `null`.* |

* `theme`:

| Theme | Example |
| ------------- |-------------|
| `btn` | ![alt text](https://image.ibb.co/e7883o/Default.png) |
| `btn-light` | ![alt text](https://image.ibb.co/jAOaio/Light.png) |
| `btn-dark` | ![alt text](https://image.ibb.co/kSmvio/Dark.png) |

* `lang`:

| Languages | Example |
| ------------- |-------------|
| `en` | ![alt text](https://image.ibb.co/e7883o/Default.png) |
| `es` | ![alt text](https://image.ibb.co/mLyVG8/Default.png) |

Handle all payment responses.
* Completed
```javascript
 onCompletedPayment: function (response)
		 {
				 //Handle response
		 },
```


* Cancelled
```javascript
onCancelledPayment: function (response)
		{
				//Handle response
		},
```


* Expired
```javascript
onExpiredPayment: function (response)
		{
				//Handle response
		}
```

* `response` data
```javascript
{
  "referenceNumber": "a387643827-fdew98ffw9fbfewkjb",
  "total": 1.00,
  "tax": 1.00,
  "subtotal": 1.00,
  "metadata1": "metadata1 test",
  "metadata2": "metadata2 test",
  "items": "[{\"name\":\"First Item\",\"description\":\"This is a description.\",\"quantity\":\"1\",\"price\":\"1.00\",\"tax\":\"1.00\",\"metadata\":\"metadata test\"},{\"name\":\"Second Item\",\"description\":\"This is another  description.\",\"quantity\":\"1\",\"price\":\"1.00\",\"tax\":\"1.00\",\"metadata\":\"metadata test\"}]"
}
```

## Testing
To test ATH Móvil’s Payment Button on your website follow these steps:
* Set the `env` variable to the value `sandbox`.

* Set the `publicToken` variable to the value `sandboxtoken01875617264`.

* Open the payment process.

* Use any username and password to log in.

* Complete, cancel or wait for the payment to expire to test all possible responses.


## Services
The following services can be used to verify the status of a transaction, perform refunds and request a list of all the payments received in a given time frame.

### Transaction Status
* Method:` POST`
* Headers: `Content-Type` -	`application/json`
 Endpoint: `https://www.athmovil.com/rs/v2/transactionStatus`
* Body Example:
```javascript
{
    "publicToken": "hdb932832klnasKJGDW90291",
    "privateToken": "JHEFEWP2048FNDFLKJWB2",
    "referenceNumber": "a387643827-fdew98ffw9fbfewkjb"
}
```

* Reponse Example:
```javascript
{
    "status": "completed",
    "referenceNumber": "212831546-402894d56b240610016b2e6c78a6003a",
    "date": "2019-06-06 16:12:02.0",
    "refundedAmount": "0.00",
    "total": "1.00",
    "tax": "1.00",
    "subtotal": "1.00",
    "metadata1": "metadata1 test",
    "metadata2": "metadata2 test",
    "items": "[{\"name\":\"First Item\",\"description\":\"This is a description.\",\"quantity\":\"1\",\"price\":\"1.00\",\"tax\":\"1.00\",\"metadata\":\"metadata test\"},{\"name\":\"Second Item\",\"description\":\"This is another description.\",\"quantity\":\"1\",\"price\":\"1.00\",\"tax\":\"1.00\",\"metadata\":\"metadata test\"}]"
}
```
----

### Refund
* Method:` POST`
* Headers: `Content-Type` -	`application/json`
* Endpoint: `https://www.athmovil.com/rs/v2/refund`
* Body Example:
```javascript
{
    "publicToken": "hdb932832klnasKJGDW90291",
    "privateToken": "JHEFEWP2048FNDFLKJWB2",
    "referenceNumber": "387643827-fdew98ffw9fbfewkjb"
    "amount":"1.00"
}
```
* Response Example:
```javascript
{
  "refundStatus": "completed",
  "referenceNumber": "212831546-402894d56b240610016b2e6c78a6003a",
  "date": "2019-06-06 16:12:02.0",
  "refundedAmount": "0.00",
  "total": "1.00",
  "tax": "1.00",
  "subtotal": "1.00",
  "metadata1": "metadata1 test",
  "metadata2": "metadata2 test",
  "items": "[{\"name\":\"First Item\",\"description\":\"This is a description.\",\"quantity\":\"1\",\"price\":\"1.00\",\"tax\":\"1.00\",\"metadata\":\"metadata test\"},{\"name\":\"Second Item\",\"description\":\"This is another description.\",\"quantity\":\"1\",\"price\":\"1.00\",\"tax\":\"1.00\",\"metadata\":\"metadata test\"}]"
}
```
----

### Transactions
* Method:` GET`
* Headers: `Content-Type` -	`application/json`
* Endpoint: `https://www.athmovil.com/transactions/v1/business`
* Body Example:
```javascript
{
    "publicToken": "hdb932832klnasKJGDW90291",
    "privateToken": "JHEFEWP2048FNDFLKJWB2",
    "fromDate": "2019-01-01 16:12:02",
    "toDate":"2019-06-06 16:12:02"
}
```
* Response Example:
```javascript
{
    [
      "transactionType": "refund",
      "referenceNumber": "212831546-7638e92vjhsbjbsdkjqbjkbqdq",
      "date": "2019-06-06 17:12:02.0",
      "refundedAmount": "1.00",
      "total": "1.00",
      "tax": "1.00",
      "subtotal": "1.00",
      "metadata1": "metadata1 test",
      "metadata2": "metadata2 test",
      "items": "[{\"name\":\"First Item\",\"description\":\"This is a description.\",\"quantity\":\"1\",\"price\":\"1.00\",\"tax\":\"1.00\",\"metadata\":\"metadata test\"},{\"name\":\"Second Item\",\"description\":\"This is another description.\",\"quantity\":\"1\",\"price\":\"1.00\",\"tax\":\"1.00\",\"metadata\":\"metadata test\"}]"
    ],
    [
      "transactionType": "payment",
      "status": "completed",
      "referenceNumber": "212831546-402894d56b240610016b2e6c78a6003a",
      "date": "2019-06-06 16:12:02.0",
      "refundedAmount": "0.00",
      "total": "1.00",
      "tax": "1.00",
      "subtotal": "1.00",
      "metadata1": metadata1 test,
      "metadata2": metadata2 test,
      "items": "[{\"name\":\"First Item\",\"description\":\"This is a description.\",\"quantity\":\"1\",\"price\":\"1.00\",\"tax\":\"1.00\",\"metadata\":\"metadata test\"},{\"name\":\"Second Item\",\"description\":\"This is another description.\",\"quantity\":\"1\",\"price\":\"1.00\",\"tax\":\"1.00\",\"metadata\":\"metadata test\"}]"
    ]
}
```
----

### Errors Codes

| Error Code  | Description |
| ------------- | ------------- |
| 3010 | publicToken is invalid |
| 3020 | publicToken is revoked |
| 3030 | publicToken is required |
| 3040 | privateToken is invalid |
| 3050 | privateToken is revoked |
| 3060 | privateToken is required |
| 3070 | tokens are from different accounts |
| 4010 | referenceNumber is required |
| 5010 | transaction does not exist |
| 5020 | transaction is from another business |
| 7010 | transaction already refunded |
| 7020 | amount is invalid |
| 7030 | amount is required |
| 7040 | error completing refund |

## User Experience
![paymentux](paymentux.png)

## Legal
The use of this API and any related documentation is governed by and must be used in accordance with the Terms and Conditions of Use of ATH Móvi Business ®, which may be found at: https://athmovilbusiness.com/terminos.
