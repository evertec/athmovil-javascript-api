# ATH Móvil Web Javascript and Services


## Introduction
ATH Móvil's Web API provides a simple, secure and fast checkout experience to customers using your website. After integrating our simple checkout process on your website, you will be able to receive instant payments from more than a million ATH Móvil users.

## Prerequisites
Before you begin, please review the following prerequisites:

* An active ATH Móvil Business account is required to continue. *To sign up, download the ATH Móvil Business application on your iOS or Android on your mobile device.*

* Your ATH Móvil Business account needs to have a registered, verified and active ATH® card.

* Have the API key of your Business account at hand. You can view your API key on the settings section of the ATH Móvil Business application for iOS or Android.

If you need help signing up, adding a card or have any other question please refer to https://athmovilbusiness.com/preguntas or contact our support team at (787) 773-5466.

## Installation
Before getting started you will need to reference both jQuery and ATH Móvil's API on your project's checkout file.
```javascript
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>
<script src="https://www.athmovil.com/api/js/athmovil.js"></script>
```

*jQuery can be referenced on other locations. Downloaded versions can also be used. Version 3.3.1 or higher is required.*

## Usage
To integrate ATH Móvil’s checkout process to your website follow these steps:

### HTML
Add the “Pay with ATH Móvil” button to your checkout view.
```html
  <div id="ATHMovil_Checkout_Button"></div>
```
*Button width is set to fill its parent while height changes proportionally to the width.*

---

### Javascript
Configure the payment values using javascript on your checkout view.
```javascript
<script type="text/javascript">

    ATHMovil_CHECKOUT_EXPRESS = {

        env: 'sandbox',
        token: 'fb1f7ae2849a07da1545a89d997d8a435a5f21ac',

        timeout: 300000,

        ordertype: 'ATHMovil_CHECKOUT_EXPRESS',

        style: 'btn',
        lang: 'en',

        paymenttotal: 1.00,
        paymenttax: 1.00,
        paymentsubtotal: 1.00,

        items: [
            {
                "name":"First Item Name",
                "description":"This is an item description.",
                "quantity":"1",
                "price":"1.00",
                "tax":"1.00",
                "sku":"aw2518h"
            },
            {
                "name":"Second Item Name",
                "description":"This is an item description.",
                "quantity":"1",
                "price":"1.00",
                "tax":"1.00",
                "sku":"aw2518hf"
            }
        ],
	...
	</script>
```

| Variable  | Data Type | Required | Values | Description |
| ------------- |:-------------:|:-----:| ------------- |  ------------- |
| `env` | String  | Yes | `sandbox` or  `production` | Use `sandbox` to perform test payments and `production` for real payments . |
| `token` | String | Yes | Business account API key. | Required for security purposes. |
| `timeout` | Number | No | Number between `120000` and `600000`. | Use this optional variable to limit the amount of time (in milliseconds) that the user has to complete the payment. Timer starts the moment ATH Móvil's instance is displayed. The default value of this method is set to 600000 (10 mins). |
| `ordertype` | String | Yes | `ATHMovil_CHECKOUT_EXPRESS` | Always set to provided string. |
| `style` | String | Yes | `btn`, `btn-dark` or `btn-light`. | Defines the theme of the “Pay with ATH Móvil” button that is displayed to the end user. |
| `lang` | String | Yes | `en` for english or `es` for spanish. | Defines the language of the “Pay with ATH Móvil” button that is displayed to the end user. |
| `paymenttotal` | Number | Yes | From `1.00` to `1500.00`. | The amount provided on this method is the final amount to be paid by the end user. |
| `paymenttax` | Number | No || Use this optional variable to display the payment tax. |
| `paymentsubtotal` | Number | No || Use this optional variable to display the payment subtotal. |
| `items` | Array | Yes || Use this variable to display on ATH Móvil's checkout screen the items that the user is purchasing. *`sku` and `tax` are required but can be set as null.* *If you don't want to include items leave this variable empty `items[]`* |

* `styles`:

| Style  | Example |
| ------------- |-------------|
| `btn` | ![alt text](https://image.ibb.co/e7883o/Default.png) |
| `btn-light` | ![alt text](https://image.ibb.co/jAOaio/Light.png) |
| `btn-dark` | ![alt text](https://image.ibb.co/kSmvio/Dark.png) |

* `lang`:

| Languages  | Example |
| ------------- |-------------|
| `en` | ![alt text](https://image.ibb.co/e7883o/Default.png) |
| `es` | ![alt text](https://image.ibb.co/mLyVG8/Default.png) |

Handle all the possible payment responses.
* Completed
```javascript
onResponsePayment: function(response)
		 {
				 //Handle Completed response
		 },
```

* Cancelled
```javascript
onCancelPayment: function(response)
		{
				//Handle Cancelled response
		},
```

* Timeout
```javascript
onTimeoutPayment: function(response)
		{
				//Handle Timeout response
		}
```

## Testing
To test ATH Móvil’s checkout process on your website follow these steps:
* Set the `env` variable to the value `sandbox`.

* Open the payment process.

* Use any username or password to log in

* Complete or cancel the payment process to test both responses.  Timeouts can be tested by waiting for the set timer to expire.

*Testing payment completions on mobile devices is not supported by the sandbox.*

## Additional Services
ATH Móvil's API provides two services that can be used to verify the status of transactions and perform refunds.

### Transaction Status Service
* Method:` POST`
* Headers: `Content-Type` -	`application/json`
* Endpoint: `https://www.athmovil.com/rs/transactionStatus`
* Request:
```javascript
{
    "token":"fb1f7ae2849a07da1545a89d997d8a435a5f21ac",
    "transactionId":"874639874bjhvhgfiugff4392890-"
}
```

* Reponse:
```javascript
{
    "paymentDetails": "[{\"Item Name\":\"First Item Name\",\"description\":\"This is a description\",\"quantity\":\"1\",\"price\":\"1.00\",\"tax\":\"1.00\",\"sku\":\"123123kjkajsdas\",\"formattedPrice\":\"$35.00\"},{\"name\":\"Nier: Automata DLC\",\"description\":\"VideoGame\",\"quantity\":\"1\",\"price\":\"17.2\",\"tax\":\"0\",\"sku\":\"lloiasdasd123123\",\"formattedPrice\":\"$17.20\"}]",
    "transactionStatus": "COMPLETED",
    "refundedStatus": "NOT REFUNDED"
}
```

| Response Field  | Possible Values |
| ------------- | ------------- |
| paymentDetails | Returns the provided item array. |
| transactionStatus | `COMPLETED` - The payment was successfully completed.; `FAILED` & `IN_PROCESS_BTRANS` - The payment was not processed.
| refundedStatus | `NOT REFUNDED` - A refund for the payment has not been performed.; `REFUNDED` - Payment was already refunded. A new refund cannot be performed.|

----

### Refund Service
* Method:` POST`
* Headers: `Content-Type` -	`application/json`
* Endpoint: `https://www.athmovil.com/rs/refund`
* Request:
```javascript
{
	"token":"B6FOICTMRV9V49R5BPEHH19DE8XZ7E0U2KINS77H",
	"transactionId":"ff80808164adb9470164cd74ac57018d",
	"amount":"1.00"
}
```
* Refund Completed Response:
```javascript
{
	"refundStatus": "Completed",
	"completed": true,
	"error": "null",
	"errorMessage": "null"
}
```
*Values are fixed.*

* Refund Failed Response:
```javascript
{
	"idError": "7010",
	"error": "Transaction already Refunded",
}
```

| `idError`  | Description |
| ------------- | ------------- |
| 4010 | Transaction and Token missing |
| 4011 | Transaction, Token and amount missing |
| 4020 | Transaction type not eCommerce |
| 3010 | Token is invalid |
| 3020 | Token is off |
| 3030 | Token missing |
| 5010 | Transaction not found |
| 5020 | Transaction does not match business |
| 5030 | Transaction missing |
| 7010 | Transaction already Refunded |
| 7020 | Amount invalid |
| 7030 | Amount Missing |
| 7040 | Error transaction |



## User Experience
![alt text](https://preview.ibb.co/c61Xqe/API_Flow.png)

## Legal
### API Terms of Service
You have the option of using the API code described herein, free of charge, which will allow you to integrate the ATH Móvil Business service (the “Service”) as a method of payment in your webpages or applications. In order to use the API, you must (1) be registered in the Service and (2) comply with the Service’s terms and conditions of use and with the API documentation (as made available to you herein). You hereby acknowledge that any use, reproduction or distribution of the API or API documentation, or any derivatives or portions thereof, constitutes your acceptance of these terms and conditions, including all other sections within the API documentation. The API documentation may not be modified. No title to the intellectual property in the API or API documentation is transferred to you under these terms and conditions. Your use of the API or the API documentation, whether through a developer or otherwise, is made with the understanding that neither your financial institution nor Evertec will provide you with any technical support, customer support or maintenance in relation to the use of the API. You may discontinue the use of the API at any time. In the event that you are assisted by a developer, you understand and acknowledge that you will be solely responsible for your developer’s compliance with the terms and conditions of the Service, these terms and conditions, and the rest of the API documentation.

### Disclaimer of Warranty
You hereby understand and acknowledge that the API is provided “AS IS” and that your use of the API is completely voluntary and at your own risk. Both Evertec and your financial institution disclaim all warranties and make no representations of any kind, whether express or implied, as to (1) the merchantability or fitness of the API for any particular purpose; (2) the APIs performance or availability; (3) the APIs condition of titled; or (4) that the APIs use or process derived or produced therefrom will not infringe any patent, copyright or other third parties. You agree that in no event shall Evertec or your financial institution be liable for any direct, indirect, special, consequential or accidental damages or loss (including, but not limited to, loss of anticipated profits or data, or other commercial damage), however arising, or any kind with relation to the API and its use or inability to use with the Service.
