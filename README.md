# ATH Móvil Payment Button - Javascript Integration and Services

## Introduction
ATH Móvil's Javascript integration provides a simple, secure and fast checkout experience to customers paying on your website. After integrating our Payment Button on your website, you will be able to receive instant payments from more than a million ATH Móvil users.

## Prerequisites
Before you begin, please review the following prerequisites:

* An active ATH Móvil Business account is required to continue.
Note: *To sign up, download "ATH Móvil Business"x on the App Store if you have an iOS device or on the Play Store if you have an Android device.*

* Your ATH Móvil Business account needs to have a registered, verified and active ATH® card.

* Have the public and private API keys of your Business account at hand. Note: *You can view your API keys on the settings section of the ATH Móvil Business application for iOS or Android.*

If you need help signing up, adding a card or have any other question please refer to https://athmovilbusiness.com/preguntas or contact our support team at (787) 773-5466.

## Installation
Before getting started you will need to add references to both jQuery and ATH Móvil's javascript on your project's HTML checkout file.
```javascript
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>
```
```javascript
<script src="https:// www.athmovil.com/api/js/v2/athmovilV2.js"></script>
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
    "fromDate": "2019-01-01 16:12:02.0",
    "toDate":"2019-06-06 16:12:02.0"
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
![alt text](https://preview.ibb.co/c61Xqe/API_Flow.png)

## Legal
### Terms for the use of the API
1. General. You have the option of using the API code described herein, free of charge, which will allow you to integrate the ATH Móvil®  as a method of payment in your business’ webpages or applications through the ATH Móvil Business® service (the “Service”) provided by Evertec Group, LLC (“Evertec,” “we,” or “us”). You can also find the API code and related API documentation (the “Licensed Materials”) free of charge in www.athmovilbusiness.com under the “Botón de Pago” section. For purposes of these terms and conditions, references to “you” or “your” refer to you as a user of the service who operates a business and wishes to receive payments for goods and services through the Service.
To use the API, you must:
(1) be registered in the Service,
(2) comply with these terms and conditions of use, and 
(3) comply with the API documentation. 
By accessing and using the Licensed Materials, you are agreeing to these terms and all sections within the API documentation, which shall also form a part of these terms and conditions of use. You will only access (or attempt to access) the API by the means described in the API documentation, which may not be modified by you or a third party on your behalf. No title to the intellectual property in the Licensed Materials is transferred to you hereunder. In the event that you are assisted by a developer, you will be solely responsible for your developer’s compliance with these terms and conditions of use and the API documentation. You may discontinue the use of the API at any time. We reserve the right to terminate these terms and conditions of use with you or discontinue the API or any portion or feature or your access to the Licensed Materials at any time by without liability or other obligation to you if we reasonably believe that you are in violation of these terms and conditions of use. We will notify upon any such termination by Evertec by sending you an email to your email address registered in the Service. Upon any termination of these terms and conditions or discontinuation of your access to the Licensed Materials, you will immediately stop using the API. Your use of the Licensed Materials, whether through a developer or otherwise, is made with the understanding that neither your financial institution or credit union (altogether from now on, “Financial Institution”) will provide you with any technical, customer support or maintenance support in relation to the use of the API. For technical support in the integration of the API in your business’ webpages or applications, you may call us at (787) 773-5466.

2. License Grant and Restrictions. 
•	We grant you a non-exclusive, non-transferable, non-sublicensable, worldwide, revocable right and license to use the Licensed Materials only for commercial purposes in accordance with these terms and conditions, to integrate ATH Móvil as a method of payment in your business’ webpages or applications for the duration of these terms and conditions of use.
•	We can set and enforce limits on your use of the API in our sole discretion. You agree to, and will not attempt to circumvent, such limitations (if any), whether documented or not, in the API documentation. 
•	You will not make any statement or representation regarding your use of the API which suggests partnership, teaming, sponsorship by, or endorsement by us or your Financial Institution.
•	You may not (and will not allow those acting on your behalf to): sublicense the API for use by a third party; create an API that functions substantially the same as the API provided hereunder and offer it for use by third parties; interfere with or disrupt the API; reverse engineer, decompile, disassemble or attempt to extract the source code from the API; use the Licensed Materials in any manner that could potentially undermine the security of the Licensed Materials; or use the Licensed Materials to carry out, encourage or promote any illegal activity.
•	Any derivative works created or invented by you or your developer based on the licensed materials hereunder shall remain the sole property of Evertec.

3. Obligations. 
•	You agree to comply with any and all applicable laws and regulations.
•	You agree to comply with Payment Card Industry Data Security Standards (PCI DSS).
•	You agree that Evertec may monitor use of the API to ensure quality, improve its services, and verify your compliance with these terms and conditions of use and the Licensed Materials.
•	You will use commercially reasonable efforts to protect user information collected by your website, including personally identifiable information, from unauthorized access or use, and will be responsible for reporting to your users any unauthorized access or use of such information to the extent required by applicable law. 
•	By using the API, Evertec may use submitted information in accordance with its privacy policy as published in www.athmovilbusiness.com.
•	If you provide feedback or suggestions about the API, then Evertec may use such information without obligation to you.

4. Disclaimer of Warranty, Limitation of Liability and Indemnity. Except as otherwise stated in these terms and conditions of use, neither Evertec nor your Financial Institution make any specific promises about the API. We provide the API “AS IS.” Your use of the API is completely voluntary and at your own risk. Evertec and your Financial Institution disclaim all warranties and make no representations of any kind, whether express or implied, as to (1) the merchantability or fitness of the API for any particular purpose; (2) the API’s performance or availability; (3) the API’s condition of title; or (4) that the API’s use or process derived or produced therefrom will not infringe any patent, copyright or other third parties intellectual property rights. You agree that in no event shall Evertec or your Financial Institution be liable for any direct, indirect, special, consequential or accidental damages or loss (including, but not limited to, loss of anticipated profits or data, or other commercial damage), of any kind, with relation to the API and its use or inability to use with the Service. Furthermore, you agree to defend and indemnify us and your Financial Institution and each of their respective directors, employees and representatives against all liabilities, damages, losses, costs, fees and expenses relating to any allegation or third-party legal proceeding to the extent arising from: (a) your use of the API, (b) your violation of these terms and conditions of use (including the Licensed Materials), (c) your violation of any and all applicable laws and regulations, (d) your failure to comply PCI DSS, (e) your infringement or other violation of a third-party’s or Evertec’s intellectual property rights, or (f) any content or data routed into or used with the API by you or those acting on your behalf (including your developer, if any).

### Use of pATH
By using the Service, you allow Evertec to use the pATH and name of your business to advertise or promote the Service or offer information regarding the Service in Evertec’s website and social media webpages. If at any time you decide that you don’t want Evertec to use the pATH and/or name of your business for this purpose, you can opt out by writing an email to marketing@evertecinc.com requesting that your pATH and/or name no longer be used for this purpose.
