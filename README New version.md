# ATH Móvil Payment Button - Javascript Integration and Services
# <a name="_toc133417448"></a>Change Log


|Date|Changes|Comments|
| - | - | - |
|08/17/2023|Initial version 1.0||
||||
||||
||||
||||
||||
||||
||||
||||
||||


























# **Table of Contents**
[Introduction	3****](#_toc143170424)**

[**Prerequisites	3****](#_toc143170425)

[**Support	4****](#_toc143170426)

[**Customer Experience	4****](#_toc143170427)

[**Installation	5****](#_toc143170428)

[**Usage	8****](#_toc143170429)

[**Callback Functions	11****](#_toc143170430)

[**Find Payment Service	14****](#_toc143170431)

[**Transaction Expired or Canceled Response: Status CANCEL	17****](#_toc143170432)

[**Refund Payment	17****](#_toc143170433)

[**Cancel Payment	19****](#_toc143170434)

[**Error Messages	20****](#_toc143170435)

[**User Flow	27****](#_toc143170436)

[**Legal	28****](#_toc143170437)

[**Reporting	28****](#_toc143170438)

[**Other information	28****](#_toc143170439)

## Introduction
ATH Móvil's Javascript integration provides a simple, secure and fast checkout experience to customers paying on your website. After integrating our Payment Button on your website, you will be able to receive real time payments from more than 1.5 million ATH Móvil users.

The API called for this JavaScript code is build based on JWT protocol to securely authenticate the communication between our services.


## Prerequisites
Before using the ATH Móvil’s payment you need to have:

1\. An active ATH Business account.

2\. A card registered in your ATH Business profile. 

3\. The public and private key assigned to your business.

To start working with the Javascript for ATH Móvils Payment Button with all its services, it is mandatory to have a Public Token per each business. This Public Token is found in the settings section of the ATH Business app and is assigned one unique token per ATH Business account. 

Additionally have the link for athmovil\_base.js to add it in your ecommerce platform with a tag<script></script>.

Production link: https://payments.athmovil.com/api/js/athmovil\_base.js

<script src=" https://payments.athmovil.com/api/js/athmovil\_base.js script>


```javascript
public token: a66ce73d04f2087615f6320b724defc5b4eedc55
<script src="js/athmovil_base.js"></script>```

ATH Business Settings:


## Support
If you need help signing up, adding a card or have any other question please refer to https://athmovilbusiness.com/preguntas or contact our support team at (787) 773-5466. For technical support please complete the following form:  https://forms.gle/ZSeL8DtxVNP2K2iDA.
## Customer Experience
The new version of the payment button will introduce more security and synchronization with both the merchant and the customer. Here you can see a brief diagram on the high-level flow of the transaction:
## Installation
From your ecommerce website identify the checkout page where you will display the payment button, for example: "my\_cart.html".

Next add in your tag <body></body> two scripts using <script></scrip> tag. 

The first script should have the athmovil\_base.js link in **src** property, for example:  
```javascript
<script src="https://payments.athmovil.com/api/js/athmovil\_base.js"></script>
```
The second script should have a JSON object called "ATHM\_Checkout" where you should put your public token as the value for the property publicToken from ATHM\_Checkout object.

Also, this second script should have three callback functions:

- **authorizationATHM()**
- **cancelATHM()**
- **expiredATHM()**

Finally, you should add in your body html a <div></div> tag with value "ATHMovil\_Checkout\_Button\_payment" in **id** property.

*Example:*
```html
<body>
<div id="ATHMovil_Checkout_Button_payment"></div>
<script src="js/athmovil_base.js"></script>
<script type="text/javascript">
          const ATHM_Checkout = {
              env: 'dev',
              publicToken: 'a66ce73d04f2087615f6320b724defc5b4eedc55',
              timeout: 600,
              orderType: '',
              theme: 'btn',
              lang: 'en',
              total: 1,
              subtotal: 1,
              tax: 1,
              metadata1: 'Prueba1.1',
              metadata2: 'Prueba2.2',
              items: [
                  {
                      "name":"Nombre de arreglo",
                      "description":"Prueba de items",
                      "quantity":"3",
                      "price":"2",
                      "tax":"1",
                      "metadata":"prueba metadata"
                  }
            ],
            phoneNumber: ""
          }
          async function authorizationATHM(){
            const responseAuth = await authorization();
            console.log(responseAuth);
          }
          async function cancelATHM(){
            const responseCancel = await findPaymentATHM();
            console.log(responseCancel);
          }
          async function expiredATHM(){
            const responseExpired = await findPaymentATHM();
            console.log(responseExpired);
          }
    </script>
</body>
```

## Usage
The correct implementation of div and scripts, should show the payment button like this example:

![boton](boton.png)
 
The correct implementation of div and scripts, should show the payment button like this example:

After clicking the Javascript consumes the first service "/payment", this service could response a success or an error status.

If you receive a success status, you will also get a ecommerceId and auth\_token in the data response property and open a modal that shows you a message for waiting.

```javascript
{
    "status": "success",
    "data": {
        "ecommerceId": "ad42df37-f989-11ed-8935-cd14e3558bc7",
        "auth_token": "eyJraWQiOiJNeUtVRXZvb2NSMWptbnZocHZXVEI0WmZvcU1wbEx6TWF5VzdjUWd1ck5FIiwidHlwIjoiSldUIiwiYWxnIjoiUlMyNTYifQ.eyJzdWIiOiI0MjdmOTZiMTExMmYyZGZlNTk4NjM0YWVkNmYyOTA4NmJmNWU5OTdlYjYyYTVjMDJlOTI0YTdmNTIzZDI3ZDliMzI2OGE1N2RmYWQ4ZWE3NGY1M2JhNWQzMjMyNTRkYTEiLCJmaUlkIjoiIiwibmJmIjoxNjg0ODYwNTIzLCJhenAiOltdLCJwZXJtaXNzaW9ucyI6WyJjdXN0b21lci5idXNpbmVzcy5lY29tbWVyY2UuYXV0aG9yaXphdGlvbjp3cml0ZSJdLCJpc3MiOiJQcm9jZXNzIFBheW1lbnQiLCJzY29wZXMiOlsiY3VzdG9tZXIuYnVzaW5lc3MuZWNvbW1lcmNlLmF1dGhvcml6YXRpb246d3JpdGUiXSwiZXhwIjoxNjg0ODYxNDIzLCJpYXQiOjE2ODQ4NjA1MjN9.HFPQncPDvIIqU4DeORiirntetxoU-KaRLWBK_bIAqJdR2cOWyhTTjVhVtbnCMN6qjsWB3knhp9N0aaVXPOi9DhYoWRlGVWLhSByp4K7c1fJwKFLhJoasQCew8SlXwQlalbYHt1F5s1hQgGmStGATIwnXRrE-4doBKpNedQn9CKo3qX08QGk78eAPnejzJKMlYOr__kFDR1c-L7P2btOvlx5vYDXhqmq_gljqp8f5a28pBFVh6DMx12IUu_FiQrI4ofinjiij3CWfXOVcqzBbE0UJudlS43Jb7JlZPflDrD6TM3PR4a8_KtM89Solm-r4__aIw02Gqf5ROsan_YT7FA"
    }
}
```
![screen_uno](screen_uno.png)

Immediately the modal should open the phoneNumberATHM.html screen, here the customer has to enter the phone number that will receive the transaction and press continue. This screen consumes “/updatePhoneNumber” service, then closes the phoneNumberATHM.html and opens the waitingPaymentATHM.html screen. Simultanously the customer will receive a push notification on the ATHMovil app stating that a payment is waiting to be completed.

![screen_dos](screen_dos.png)

![screen_tres](screen_tres.png)

After receiving the push notification, the customer must open the ATHMovil app and confirm the transaction. After the customer confirms the transaction  the Javascript will then consume the ”authorization” service automatically and should close waitingPaymentATHM.html with  a success message on the main screen where you have a payment button.

![screen_cuatro](screen_cuatro.png)

## Callback functions

`authorizationATHM`. This function returns a JSON object with the details of the transaction after it has been completed and processed.

```javascript
{
    "status": "success",
    "data": {
        "ecommerceStatus": "COMPLETED",
        "ecommerceId": "870633c9-f994-11ed-8935-c155d7fc6afe",
        "referenceNumber": "215070440-8a36d420882a293a018849cae9f500a8",
        "businessCustomerId": "402894d56e713892016e7f2963de0010",
        "transactionDate": "2023-05-23 14:06:54",
        "dailyTransactionId": "0001",
        "businessName": "Tdameritrade",
        "businessPath": "Tdameritrade",
        "industry": "COMPUTERS",
        "subTotal": 1.33,
        "tax": 1.00,
        "total": 2.33,
        "fee": 0.06,
        "netAmount": 2.28,
        "totalRefundedAmount": 0,
        "metadata1": "Metadata 1",
        "metadata2": "Metada 2",
        "items": [
            {
                "name": "Diego MO",
                "description": "Diego",
                "quantity": 1,
                "price": 1.33,
                "tax": 1,
                "metadata": "Bitcoin es lo mejor",
                "formattedPrice": "",
                "sku": ""
            }
        ],
        "isNonProfit": false
    }
}
```

`cancelATHM`. This function consumes “/findPayment” service  to retrieve the status of the transaction in the event that it gets cancelled and returns a JSON object with the details of the transaction.

```javascript
{
    "status": "success",
    "data": {
        "ecommerceStatus": "CANCEL",
        "ecommerceId": "a5f8143a-f997-11ed-8935-a9b922a1efbc",
        "referenceNumber": "",
        "businessCustomerId": "402894d56e713892016e7f2963de0010",
        "transactionDate": "",
        "dailyTransactionId": "",
        "businessName": "Tdameritrade",
        "businessPath": "Tdameritrade",
        "industry": "COMPUTERS",
        "subTotal": 1.33,
        "tax": 1.00,
        "total": 2.33,
        "fee": 0.00,
        "netAmount": 0,
        "totalRefundedAmount": 0,
        "metadata1": "Metadata 1",
        "metadata2": "Metada 2",
        "items": [
            {
                "name": "Diego MO",
                "description": "Diego",
                "quantity": 1,
                "price": 1.33,
                "tax": 1,
                "metadata": "Bitcoin es lo mejor",
                "formattedPrice": "",
                "sku": ""
            }
        ],
        "isNonProfit": false
    }
}
```

`expiredATHM`. This function consumes “/findPayment” service to retrieve the status of the transaction in the event that it expires and returns a JSON object with the details of the transaction. 

```javascript
{
    "status": "success",
    "data": {
        "ecommerceStatus": "CANCEL",
        "ecommerceId": "a5f8143a-f997-11ed-8935-a9b922a1efbc",
        "referenceNumber": "",
        "businessCustomerId": "402894d56e713892016e7f2963de0010",
        "transactionDate": "",
        "dailyTransactionId": "",
        "businessName": "Tdameritrade",
        "businessPath": "Tdameritrade",
        "industry": "COMPUTERS",
        "subTotal": 1.33,
        "tax": 1.00,
        "total": 2.33,
        "fee": 0.00,
        "netAmount": 0,
        "totalRefundedAmount": 0,
        "metadata1": "Metadata 1",
        "metadata2": "Metada 2",
        "items": [
            {
                "name": "Diego MO",
                "description": "Diego",
                "quantity": 1,
                "price": 1.33,
                "tax": 1,
                "metadata": "Bitcoin es lo mejor",
                "formattedPrice": "",
                "sku": ""
            }
        ],
        "isNonProfit": false
    }
}
```

## Find Payment
This service can be used to find the status of a transaction. This service “/business/findPayment” requires a payload with two mandatory attributes "ecommerceId" and “publicToken”, which will be validated by the same service.


**Endpoint:** https://payments.athmovil.com/api/business-transaction/ecommerce/business/findPayment

**Headers:** Content-type – application/json

**Request**

- **ecommerceId**: This ID represent the ticket of the transaction to be paid with the information provided in the request.
- **publicToken**: Determines the business account that the payment will be sent to.


curl --location --request POST 'https://vpce-04edaf73e4e83adea-flbxnqbx.execute-api.us-east-1.vpce.amazonaws.com/api/business-transaction/ecommerce/business/findPayment' \

--header 'Host: ozm9fx7yw5.execute-api.us-east-1.amazonaws.com' \

--header 'Accept: application/json' \

--header 'Authorization: Bearer \

--header 'Content-Type: application/json' \

--data-raw '{

`    "ecommerceId"': "177a50fd-39fb-11ed-8b3d-230262020527",

`    "publicToken"': "a66ce73d04f2087615f6320b724defc5b4eedc55",

}' 


**Response**

- **status**: Confirm status of the service response.
- **ecommerceStatus**: represents the status of the ecommerce transaction.
- **transactionDate**: Authorization date
- **referenceNumber**: Unique transaction identifier
- **dailyTransactionID**: ID count for the transaction in the day.
- **businessName**: Buiness Name for the ATH Business account
- **businessPath**: Business Path for the ATH Business account
- **industry**: Industry of the business
- **total**: Total amount of the transaction
- **tax**: Tax to be charged in the transaction.
- **subtotal**: Subtotal amount of the transaction.
- **fee**: Fee to be charged in the transaction.
- **netAmount**: Net amount of the transaction
- **totalRefundedAmount**:  amount to be refunded from the original transaction.
- **metadata1**: variable that can be left empty or filled with additional transaction information. Max length 40 characters.
- **metadata2**: variable that can be left empty or filled with additional transaction information. Max length 40 characters.
- **items**: Items paid in the transaction.



Completed transaction (/Payment +/Confirmed & /Authorize) Response: Status **COMPLETED**

{

```
"status": "success",

"data": {

"ecommerceStatus": "COMPLETED",

"ecommerceId": "730e2c49-9387-11ed-8f43-c31784ccfc6c",

"referenceNumber": "215070443-402894c185ab1be40185acfe61c2000b",

"businessCustomerId": "402894d56e713892016e7f2963de0010",

"transactionDate": "2023-01-13 16:17:06",

"dailyTransactionId": "0006",

"businessName": "Tdameritrade",

"businessPath": "Tdameritrade",

"industry": "ENTERTAINMENT",

"subTotal": 0,

"tax": 0.00,

"total": 1,

"fee": 0.6000000238418579,

"netAmount": 0.40,

"totalRefundedAmount": 0,

"metadata1": "Metadata 1",

"metadata2": "Metada 2",

"items": [

{

"name": "Diego MO",

"description": "Diego",

"quantity": 1,

"price": 10,

"tax": 0,

"metadata": "Bitcoin es lo mejor"

}

],

"isNonProfit": false

}

}
```



Transaction Pending to be confirmed by the ATH Móvil customer (/payment) Response: Status **OPEN**
```
{

   "status": "success",

    "data": {

        "ecommerceStatus": "OPEN",

        "ecommerceId": "39906664-e44e-11ed-b127-a519df48811e",

        "referenceNumber": "",

        "businessCustomerId": "402894d56e713892016e7f2963de0010",

        "transactionDate": "",

        "dailyTransactionId": "",

        "businessName": "Tdameritrade",

        "businessPath": "Tdameritrade",

        "industry": "COMPUTERS",

        subTotal": 1.33,

          "tax": 1.00,

        "total": 2.33,

       "fee": 0.00,

       "netAmount": 0,

        "totalRefundedAmount": 0,

       "metadata1": "Metadata 1",

       "metadata2": "Metada 2",

       "items": [

           {

                "name": "Diego MO",

                "description": "Diego",

                "quantity": 1,

                "price": 1.33,

                "tax": 1,

                "metadata": "Bitcoin es lo mejor",

                "formattedPrice": "",

                "sku": ""

            }

        ],

        "isNonProfit": false

    }
```


Transaction confirmed by the ATH Móvil customer but pending to be processed by the merchant (/payment+/confirm) Response: Status **CONFIRM**

```
{

"status": "success",

"data": {

"ecommerceStatus": "CONFIRM",

"ecommerceId": "39906664-e44e-11ed-b127-a519df48811e",

"referenceNumber": "",

"businessCustomerId": "402894d56e713892016e7f2963de0010",

"transactionDate": "",

"dailyTransactionId": "",

"businessName": "Tdameritrade",

"businessPath": "Tdameritrade",

"industry": "COMPUTERS",

"subTotal": 1.33,

"tax": 1.00,

"total": 2.33,

"fee": 0.00,

"netAmount": 0,

"totalRefundedAmount": 0,

"metadata1": "Metadata 1",

"metadata2": "Metada 2",

"items": [

{

"name": "Diego MO",

"description": "Diego",

"quantity": 1,

"price": 1.33,

"tax": 1,

"metadata": "Bitcoin es lo mejor",

"formattedPrice": "",

"sku": ""

}

],

"isNonProfit": false

}

}
```

```
# <a name="_toc133417456"></a><a name="_toc143170432"></a>Transaction Expired or Canceled Response: Status **CANCEL**
{

"status": "success",

"data": {

"ecommerceStatus": "CANCEL",

"ecommerceId": "29bc7846-e44f-11ed-b127-839ef0792c17",

"referenceNumber": "",

"businessCustomerId": "402894d56e713892016e7f2963de0010",

"transactionDate": "",

"dailyTransactionId": "",

"businessName": "Tdameritrade",

"businessPath": "Tdameritrade",

"industry": "COMPUTERS",

"subTotal": 1.33,

"tax": 1.00,

"total": 2.33,

"fee": 0.00,

"netAmount": 0,

"totalRefundedAmount": 0,

"metadata1": "Metadata 1",

"metadata2": "Metada 2",

"items": [

{

"name": "Diego MO",

"description": "Diego",

"quantity": 1,

"price": 1.33,

"tax": 1,

"metadata": "Bitcoin es lo mejor",

"formattedPrice": "",

"sku": ""

}

],

"isNonProfit": false

}

}
```


## Errors

If you close phonePaymentATHM.html or waitingPaymentATHM.html screen you should see next error message on the main payment button screen.

![error_uno](error_uno.png)

If you want to make another payment from other website or mobile app using ATHM's payment button (from same customer to same business), you should see next error message on the main payment button screen.

![error_dos](error_dos.png)

When you press ATHM's payment button and the don´t exist any problem, this transaction is created with an expiration time (timeout property in ATHM_Checkout object).

After you receive a success response with ecommerceId and auth_token, should open next screen (phonePaymentATHM.html) and you should add your phoneNumber for you confirm the transaction from ATHM personal app.

If you are late to make the confirm from your ATHM personal app, you should see next error message on the main payment button screen.

![error_tres](error_tres.png)

## Testing



## User experience

![user_experience](user_experience.png)

## Legal

The use of this API and any related documentation is governed by and must be used in accordance with the Terms and Conditions of Use of ATH Móvi Business ®, which may be found at: https://athmovilbusiness.com/terminos.
