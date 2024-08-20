# ATH Móvil Payment Button - Javascript Integration and Services
# <a name="_toc133417448"></a>Change Log


|Date|Changes|Comments|
| - | - | - |
|08/17/2023|Initial version 1.0|
| 07/17/2024 | Version 1.2 |  General information related to ATH Business & ATH Móvil with instructions on how to open an account. |


























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

# Introduction
ATH Móvil's Javascript integration provides a simple, secure and fast checkout experience to customers paying on your website. After integrating our Payment Button on your website, you will be able to receive real time payments from more than 1.5 million ATH Móvil users.

The API called for this JavaScript code is build based on JWT protocol to securely authenticate the communication between our services.


Disclaimer: The Payment Button ATH Móvil is not compatible with any major Ecommerce platform. This includes Shopify, Wix, Woocommerce or Stripe.


Disclaimer: We currently **do not** have a **Testing environment**. You need to have an active ATH Business account and a active ATH Móvil account.

## Prerequisites
Before using the ATH Móvil’s payment you need to have:

### ATH Business

1\. An active ATH Business account.

2\. A card registered in your ATH Business profile. 

3\. The public and private key assigned to your business.

For instructions on how to open a ATH Business account please refer to: [ATHB flyer eng letter 1.pdf](https://github.com/user-attachments/files/16267504/ATHB.flyer.eng.letter.1.pdf)

For more information related to ATH Business and how it works please refer to:[ATH BUSINESS_Apr2024.pptx](https://github.com/user-attachments/files/16267585/ATH.BUSINESS_Apr2024.pptx)

### ATH Móvil

To complete the payment for testing purposes you need to have:

1\. An active ATH Móvil account.

2\. A card registered in your ATH Móvil profile. It can not be the same card that is registered in ATH Business.

For more information related to ATH Móvil and how it works please refer to:[ATH Móvil_Apr2024.pptx](https://github.com/user-attachments/files/16267592/ATH.Movil_Apr2024.pptx)


To start working with the Javascript for ATH Móvils Payment Button with all its services, it is mandatory to have a Public Token per each business. This Public Token is found in the settings section of the ATH Business app and is assigned one unique token per ATH Business account. 

Additionally have the link for athmovil\_base.js to add it in your ecommerce platform with a tag<script></script>.

Production link: https://payments.athmovil.com/api/js/athmovil_base.js

```javascript
<script>
    var publicToken = "a66ce73d04f2087615f6320b724defc5b4eedc55";
</script>
<script src="https://payments.athmovil.com/api/js/athmovil_base.js"></script>
```
ATH Business Settings:

<img width="98" alt="image" src="https://github.com/evertec/athmovil-javascript-api/assets/99409598/c661343f-45e1-4a70-8d50-e74ee1854986">

<img width="101" alt="image" src="https://github.com/evertec/athmovil-javascript-api/assets/99409598/7fe083e1-4537-423a-826e-10b5ff2a0a11">


## Support
If you need help signing up, adding a card or have any other question please refer to https://ath.business.com/preguntas. For technical support please complete the following form:  https://ath.business/botondepago.

## Customer Experience
The new version of the payment button will introduce more security and synchronization with both the merchant and the customer. Here you can see a brief diagram on the high-level flow of the transaction:

<img width="468" alt="image" src="https://github.com/evertec/athmovil-javascript-api/assets/99409598/3e10742b-7541-4eb6-8cac-8eaf6a40b8e9">

## Installation
From your ecommerce website identify the checkout page where you will display the payment button, for example: "my\_cart.html".

Next add in your tag <body></body> two scripts using <script></scrip> tag. 

The first script should have the athmovil\_base.js link in **src** property, for example:  
```javascript
<script src="https://payments.athmovil.com/api/js/athmovil_base.js"></script>
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
<script src="https://payments.athmovil.com/api/js/athmovil_base.js"></script>
<script type="text/javascript">
          const ATHM_Checkout = {
              env: 'production',
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

<img width="192" alt="image" src="https://github.com/evertec/athmovil-javascript-api/assets/99409598/ef770810-cb48-4ba5-ac8d-acd8b81386da">

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
<img width="468" alt="image" src="https://github.com/evertec/athmovil-javascript-api/assets/99409598/dff19ba8-954f-4e13-9ac6-4f68af94a5fc">


Immediately the modal should open the phoneNumberATHM.html screen, here the customer has to enter the phone number that will receive the transaction and press continue. This screen consumes “/updatePhoneNumber” service, then closes the phoneNumberATHM.html and opens the waitingPaymentATHM.html screen. Simultanously the customer will receive a push notification on the ATHMovil app stating that a payment is waiting to be completed.

<img width="468" alt="image" src="https://github.com/evertec/athmovil-javascript-api/assets/99409598/4b7c4ac5-c80d-4b5f-bc14-915f55ac8a94">


<img width="468" alt="image" src="https://github.com/evertec/athmovil-javascript-api/assets/99409598/98575bd6-330e-41fe-96f1-2362d800863c">


After receiving the push notification, the customer must open the ATHMovil app and confirm the transaction. After the customer confirms the transaction  the Javascript will then consume the ”authorization” service automatically and should close waitingPaymentATHM.html with  a success message on the main screen where you have a payment button.

<img width="468" alt="image" src="https://github.com/evertec/athmovil-javascript-api/assets/99409598/76e0b50a-be89-4e36-9fc8-02df7403c53e">


## Callback functions

`authorizationATHM`. This function returns a JSON object with the details of the transaction after it has been completed and processed.

```json
{
    "status": "success",
    "data": {
        "ecommerceStatus": "COMPLETED",
        "ecommerceId": "870633c9-f994-11ed-8935-c155d7fc6afe",
        "referenceNumber": "215070440-8a36d420882a293a018849cae9f500a8",
        "businessCustomerId": "402894d56e713892016e7f2963de0010",
        "transactionDate": "2023-05-23 14:06:54",
        "dailyTransactionId": "0001",
        "businessName": "I Love Puerto Rico",
        "businessPath": "ilovepr",
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
                "metadata": "ATH Movil es lo mejor",
                "formattedPrice": "",
                "sku": ""
            }
        ],
        "isNonProfit": false
    }
}
```

`cancelATHM`. This function consumes “/findPayment” service  to retrieve the status of the transaction in the event that it gets cancelled and returns a JSON object with the details of the transaction.

```json
{
    "status": "success",
    "data": {
        "ecommerceStatus": "CANCEL",
        "ecommerceId": "a5f8143a-f997-11ed-8935-a9b922a1efbc",
        "referenceNumber": "",
        "businessCustomerId": "402894d56e713892016e7f2963de0010",
        "transactionDate": "",
        "dailyTransactionId": "",
        "businessName": "I Love Puerto Rico",
        "businessPath": "ilovepr",
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
                "metadata": "ATH Movil es lo mejor",
                "formattedPrice": "",
                "sku": ""
            }
        ],
        "isNonProfit": false
    }
}
```

`expiredATHM`. This function consumes “/findPayment” service to retrieve the status of the transaction in the event that it expires and returns a JSON object with the details of the transaction. 

```json
{
    "status": "success",
    "data": {
        "ecommerceStatus": "CANCEL",
        "ecommerceId": "a5f8143a-f997-11ed-8935-a9b922a1efbc",
        "referenceNumber": "",
        "businessCustomerId": "402894d56e713892016e7f2963de0010",
        "transactionDate": "",
        "dailyTransactionId": "",
        "businessName": "I Love Puerto Rico",
        "businessPath": "ilovepr",
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
                "metadata": "ATH Movil es lo mejor",
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


`**Endpoint:**` https://payments.athmovil.com/api/business-transaction/ecommerce/business/findPayment

`**Headers:**` Content-type – application/json

**Request**

- `**ecommerceId**`: This ID represent the ticket of the transaction to be paid with the information provided in the request.
- `**publicToken**`: Determines the business account that the payment will be sent to.

```bash
curl --location --request POST 'https://vpce-04edaf73e4e83adea-flbxnqbx.execute-api.us-east-1.vpce.amazonaws.com/api/business-transaction/ecommerce/business/findPayment' \
  --header 'Host: ozm9fx7yw5.execute-api.us-east-1.amazonaws.com' \
  --header 'Accept: application/json' \
  --header 'Authorization: Bearer <your_access_token>' \
  --header 'Content-Type: application/json' \
  --data-raw '{
    "ecommerceId": "177a50fd-39fb-11ed-8b3d-230262020527",
    "publicToken": "a66ce73d04f2087615f6320b724defc5b4eedc55"
  }'
```


**Response**

- `**status**`: Confirm status of the service response.
- `**ecommerceStatus**`: represents the status of the ecommerce transaction.
- `**transactionDate**`: Authorization date
- `**referenceNumber**`: Unique transaction identifier
- `**dailyTransactionID**`: ID count for the transaction in the day.
- `**businessName**`: Buiness Name for the ATH Business account
- `**businessPath**`: Business Path for the ATH Business account
- `**industry**`: Industry of the business
- `**total**`: Total amount of the transaction
- `**tax**`: Tax to be charged in the transaction.
- `**subtotal**`: Subtotal amount of the transaction.
- `**fee**`: Fee to be charged in the transaction.
- `**netAmount**`: Net amount of the transaction
- `**totalRefundedAmount**`:  amount to be refunded from the original transaction.
- `**metadata1**`: variable that can be filled with additional transaction information. For example store ID, location,etc. Max length 40 characters.
- `**metadata2**`: variable that can be filled with additional transaction information. For example store ID, location,etc. Max length 40 characters.
- `**items**`: Items paid in the transaction.



Completed transaction (/Payment +/Confirmed & /Authorize) Response: Status `COMPLETED`



```json
{
  "status": "success",
  "data": {
    "ecommerceStatus": "COMPLETED",
    "ecommerceId": "730e2c49-9387-11ed-8f43-c31784ccfc6c",
    "referenceNumber": "215070443-402894c185ab1be40185acfe61c2000b",
    "businessCustomerId": "402894d56e713892016e7f2963de0010",
    "transactionDate": "2023-01-13 16:17:06",
    "dailyTransactionId": "0006",
    "businessName": "I Love Puerto Rico",
    "businessPath": "ilovepr",
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
        "metadata": "ATH Movil es lo mejor"
      }
    ],
    "isNonProfit": false
  }
}

```



Transaction Pending to be confirmed by the ATH Móvil customer (/payment) Response: Status `OPEN`
```json
{

   "status": "success",

    "data": {

        "ecommerceStatus": "OPEN",

        "ecommerceId": "39906664-e44e-11ed-b127-a519df48811e",

        "referenceNumber": "",

        "businessCustomerId": "402894d56e713892016e7f2963de0010",

        "transactionDate": "",

        "dailyTransactionId": "",

        "businessName": "I Love Puerto Rico",

        "businessPath": "ilovepr",

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

                "metadata": "ATH Movil es lo mejor",

                "formattedPrice": "",

                "sku": ""

            }

        ],

        "isNonProfit": false

    }
```


Transaction confirmed by the ATH Móvil customer but pending to be processed by the merchant (/payment+/confirm) Response: Status `CONFIRM`

```json
{
  "status": "success",
  "data": {
    "ecommerceStatus": "CONFIRM",
    "ecommerceId": "39906664-e44e-11ed-b127-a519df48811e",
    "referenceNumber": "",
    "businessCustomerId": "402894d56e713892016e7f2963de0010",
    "transactionDate": "",
    "dailyTransactionId": "",
    "businessName": "I Love Puerto Rico",
    "businessPath": "ilovepr",
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
        "metadata": "ATH Movil es lo mejor",
        "formattedPrice": "",
        "sku": ""
      }
    ],
    "isNonProfit": false
  }
}
```


Transaction Expired or Canceled Response: Status`CANCEL`

```json
{
  "status": "success",
  "data": {
    "ecommerceStatus": "CANCEL",
    "ecommerceId": "29bc7846-e44f-11ed-b127-839ef0792c17",
    "referenceNumber": "",
    "businessCustomerId": "402894d56e713892016e7f2963de0010",
    "transactionDate": "",
    "dailyTransactionId": "",
    "businessName": "I Love Puerto Rico",
    "businessPath": "ilovepr",
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
        "metadata": "ATH Movil es lo mejor",
        "formattedPrice": "",
        "sku": ""
      }
    ],
    "isNonProfit": false
  }
}
```
## Refund Payment
This is a Web Service that allows to refund a completed ecommerce transaction.

- Business Transaction 
  - Endpoint: “https://payments.athmovil.com/api/business-transaction/ecommerce/refund”
  - Object Request: {"amount": 0, "message": "string", "privateToken": "string", "publicToken": "string", "referenceNumber": "string"}
  - Type: REST
  - Format: application/json
  - Method: POST
  - Header:
    - Accept:  application/json
    - Content-Type: application/json
    - Authorization; Bearer Token
    - Host

**Request:**
```json
{
  "publicToken": "hdb932832klnasKJGDW90291",
  "privateToken": "JHEFEWP2048FNDFLKJWB2",
  "referenceNumber": "fdew98ffw9fbfewkjb", // transactionId
  "amount": "1.00",
  "message": "MSG Test" // Optional
}

```

**Response**
```json
{
  "status": "success",
  "data": {
    "refund": {
      "transactionType": "REFUND",
      "status": "COMPLETED",
      "refundedAmount": 1.00,
      "date": "123412341234", // timestamp
      "referenceNumber": "402894d56b240610016b2e6c78a6003a", // Refund transactionId
      "dailyTransactionID": "0107",
      "name": "Valeria Herrero",
      "phoneNumber": "(787) 123-4567",
      "email": "valher@gmail.com"
    },
    "originalTransaction": {
      "transactionType": "PAYMENT",
      "status": "COMPLETED",
      "date": "123412341234", // timestamp
      "referenceNumber": "402894d56b240610016b2e6c78a6003a", // Original Payment transactionId
      "dailyTransactionID": "0106",
      "name": "Valeria Herrero",
      "phoneNumber": "(787) 123-4567",
      "email": "valher@gmail.com",
      "message": "",
      "total": 1.00,
      "tax": 0.00,
      "subtotal": 0.00,
      "fee": 0.00,
      "netAmount": 0.00,
      "totalRefundedAmount": 1.00,
      "metadata1": "metadata1 test",
      "metadata2": "metadata2 test",
      "items": []
    }
  }
}

```

## Cancel Payment

This is a Web Service to cancel the ecommerce transaction.

- Business Transaction 
  - Endpoint: https://payments.athmovil.com/api/business-transaction/ecommerce/business/cancel”
  - Object Request: {“ecommerceId”, “publicToken “
  - Type: REST
  - Format: <a name="_hlk118195240"></a>application/json
  - Method: POST
  - Header:
    - Accept:  application/json
    - Content-Type: application/json
    - Authorization; Bearer Token
    - Host

**Request:**

```json
{
  "ecommerceId": "177a50fd-39fb-11ed-8b3d-230262020527",
  "publicToken": "3adc528b182e50b41acff658136bd974c89604c3"
}
```

**Response:**

```json
{
  "status": "success",
  "data": "Payment Cancelled."
}
```

## Error Messages

The error messages for the PB will follow a standard response structure of status, message, error code and data. Below are some examples of the responses and a list of all available error scenarios.

- Request without a token authentication.


```json

{
  "status": "error",
  "message": "No authorization header present.",
  "errorcode": "token.invalid.header",
  "data": null
}
```

- Request with an expired token.

```json
{
  "status": "error",
  "message": "The Token has expired on Fri Oct 28 15:21:00 AST 2022.",
  "errorcode": "token.expired",
  "data": null
}
```
- When the Object request it’s empty.
```json
{

"status": "error",

"message": "Required request body is missing",

"errorcode": 'BTRA\_0006',

"data": null

}
```
## Errors on the modal

If the user closes the phonePaymentATHM.html or waitingPaymentATHM.html screen the modal will display an error message on the main payment button screen.

<img width="468" alt="image" src="https://github.com/evertec/athmovil-javascript-api/assets/99409598/408f477b-d8ea-45b1-b8dd-80f5e62e4fac">


If you try to make another payment from another website or mobile app using ATHM's payment button (with the same ATH card as the business), you should see a error message on the main payment button screen.

<img width="468" alt="image" src="https://github.com/evertec/athmovil-javascript-api/assets/99409598/efeb53fd-da00-4a86-8a1c-13a125aafd96">


When you press ATHM's payment button and the isn’t any problem, this transaction is created with an expiration time (timeout property in ATHM\_Checkout object).

If the customer takes too much time in confirming the transaction from the ATH Móvil’s app then the user will see this error message on the main payment button screen.

<img width="468" alt="image" src="https://github.com/evertec/athmovil-javascript-api/assets/99409598/7244b42d-e1d7-49b7-98f0-7908c372761f">


**Additional Error Codes**:

|Error code|Description|
| :- | :- |
|error.code.es.BTRA\_9998|=Communication Error|
|error.code.en.BTRA\_9998|=Communication Error|
|error.code.es.BTRA\_9999|=Internal Error|
|error.code.en.BTRA\_9999|=Internal Error|
|error.code.es.BTRA\_0401|=Invalid authorization token|
|error.code.en.BTRA\_0401|=Invalid authorization token|
|error.code.es.BTRA\_0402|=Authorization token expired|
|error.code.en.BTRA\_0402|=Authorization token expired|
|error.code.es.BTRA\_0403|=Invalid authorization token|
|error.code.en.BTRA\_0403|=Invalid authorization token|
|error.code.es.BTRA\_0001|=El monto de la transferencia está por debajo del mínimo permitido|
|error.code.en.BTRA\_0001|=The transfer amount is under Minimum allowed|
|error.code.es.BTRA\_0002|=Estatus del cliente es Pendiente Recuperar Verificación de Acceso|
|error.code.en.BTRA\_0002|=Customer status is Pending Regain Access Verification|
|error.code.es.BTRA\_0003|=Número de tarjeta del cliente es el mismo que el del negocio|
|error.code.en.BTRA\_0003|=Card number from the customer is the same than the business|
|error.code.es.BTRA\_0004|=El monto está por encima de los límites|
|error.code.en.BTRA\_0004|=Amount is Over the limits|
|error.code.es.BTRA\_0005|=Transacción fallida. Estatus de error|
|error.code.en.BTRA\_0005|=Transaction failed. Status error|
|error.code.es.BTRA\_0006|=Formato inválido|
|error.code.en.BTRA\_0006|=Invalid format|
|error.code.es.BTRA\_0007|=TransactionId no existe|
|error.code.en.BTRA\_0007|=TransactionId does not exist|
|error.code.es.BTRA\_0008|=TransactionId no pertenece al negocio|
|error.code.en.BTRA\_0008|=TransactionId does not belong to the business|
|error.code.es.BTRA\_0009|=El negocio no está activo|
|error.code.en.BTRA\_0009|=Business is not active|
|error.code.es.BTRA\_0010|=El negocio no está activo|
|error.code.en.BTRA\_0010|=Business status is not Active|
|error.code.es.BTRA\_0011|=Número de la tarjeta no pertenece al negocio|
|error.code.en.BTRA\_0011|=Card number does not belong to the business|
|error.code.es.BTRA\_0012|=Números de tarjetas origen y destino son los mismos|
|error.code.en.BTRA\_0012|=Source and Target card numbers are the same|
|error.code.es.BTRA\_0013|=El monto es cero|
|error.code.en.BTRA\_0013|=Amount is Zero|
|error.code.es.BTRA\_0014|=TransactionId no existe para el negocio actual|
|error.code.en.BTRA\_0014|=TransactionId does not exist for the current business|
|error.code.es.BTRA\_0015|=Communication Error|
|error.code.en.BTRA\_0015|=Communication Error|
|error.code.es.BTRA\_0016|=Communication Error|
|error.code.en.BTRA\_0016|=Communication Error|
|error.code.es.BTRA\_0017|=Invalid authorization token|
|error.code.en.BTRA\_0017|=Invalid authorization token|
|error.code.es.BTRA\_0018|=no debe estar vacío|
|error.code.en.BTRA\_0018|=must not be blank|
|error.code.es.BTRA\_0019|=no debe ser nulo|
|error.code.en.BTRA\_0019|=must not be null|
|error.code.es.BTRA\_0020|=valor numérico fuera de los límites (<4 dígitos>. <2 dígitos> esperados)|
|error.code.en.BTRA\_0020|=numeric value out of bounds (<4 digits>.<2 digits> expected)|
|error.code.es.BTRA\_0021|=TransactionType no es válido|
|error.code.en.BTRA\_0021|=TransactionType is not valid|
|error.code.es.BTRA\_0022|=Reporte sin transacciones|
|error.code.en.BTRA\_0022|=Not record for report|
|error.code.es.BTRA\_0023|=Error con la fecha Desde|
|error.code.en.BTRA\_0023|=Error with the From date|
|error.code.es.BTRA\_0024|=Error con la fecha Hasta|
|error.code.en.BTRA\_0024|=Error with the To date|
|error.code.es.BTRA\_0025|=La fecha Desde debe ser anterior a la fecha Hasta|
|error.code.en.BTRA\_0025|=The From date must be before the To date|
|error.code.es.BTRA\_0026|=La diferencia entre fechas debe ser inferior a %s días|
|error.code.en.BTRA\_0026|=The difference between dates must be lower than %s days|
|error.code.es.BTRA\_0027|=La organización no es una organización sin fines de lucro|
|error.code.en.BTRA\_0027|=The organization is not a Non Profit organization|
|error.code.es.BTRA\_0028|=es menor de 0.01|
|error.code.en.BTRA\_0028|=is less than 0.01|
|error.code.es.BTRA\_0029|=El parámetro requerido '%s' no está presente|
|error.code.en.BTRA\_0029|=Required String parameter '%s' is not present|
|error.code.es.BTRA\_0030|=El refund fallo con un estatus de error|
|error.code.en.BTRA\_0030|=The refund failed with a status error|
|error.code.es.BTRA\_0031|=El ecommerceId no existe|
|error.code.en.BTRA\_0031|=EcommerceId does not exist|
|error.code.es.BTRA\_0032|=El estatus del e-commerce no esta confirmado|
|error.code.en.BTRA\_0032|=The status of the e-commerce is not confirm|
|error.code.es.BTRA\_0033|=Error al crear el registro del BusinessEcommerce|
|error.code.en.BTRA\_0033|=Error creating BusinessEcommerce recordb|
|error.code.es.BTRA\_0034|=El BusinessEcommerce ya existe en dynamoDB|
|error.code.en.BTRA\_0034|=BusinessEcommerce exists in the dynamoDB|
|error.code.es.BTRA\_0035|=El estatus del registro de Transacción e-commerce no es el espereado|
|error.code.en.BTRA\_0035|=The e-commerce transaction has not the expected status|
|error.code.es.BTRA\_0036|=El e-commerce que tratas de confirmar, ya esta asignado a otro customer|
|error.code.en.BTRA\_0036|=The e-commerce you are trying to confirm is already assigned to another customer|
|error.code.es.BTRA\_0037|=No se puede confirmar una transacción con status Cancelado o Fallido|
|error.code.en.BTRA\_0037|=Cannot confirm a transaction with status Canceled or Failed|
|error.code.es.BTRA\_0038|=El campo metadata excede el maximo de caracteres soportado (40)|
|error.code.en.BTRA\_0038|=The metadata field exceeds the maximum supported characters (40)|
|error.code.es.BTRA\_0039|=Tiempo valido para ejecutar la transacción ha expirado|
|error.code.en.BTRA\_0039|=Valid time to execute the transaction has expired|
|error.code.es.BTRA\_0040|=El mensaje no puede superar los 50 caracteres|
|error.code.en.BTRA\_0040|=The message can not exceed 50 characters|
|error.code.es.BTRA\_0041|=La transaccion e-commerce ya tiene asignado un numero telefonico|
|error.code.en.BTRA\_0041|=The e-commerce transaction already has a phone number assigned|
|error.code.es.BTRA\_0042|=El monto de la propina está por encima de los límites|
|error.code.en.BTRA\_0042|=Tip amount is over limits|
|error.code.es.BTRA\_0043|=El comercio no es dueño del pago|
|error.code.en.BTRA\_0043|=The business ins't owner of payment|
|error.code.es.BTRA\_0044|=El monto está por encima de los límites del Institución Financiera|
|error.code.en.BTRA\_0044|=Amount is Over limits of the Financial Institution|
|error.code.es.BTRA\_0045|=El monto está por encima de los límites de la tarjeta|
|error.code.en.BTRA\_0045|=Amount is Over the limits of the card|
|error.code.es.BTRA\_0046|=No hay transacciones para el rango de fechas indicado|
|error.code.en.BTRA\_0046|=There are no transactions for the indicated date range|
|error.code.es.BTRA\_0047|=La fecha desde no puede ser futura|
|error.code.en.BTRA\_0047|=Date from cannot be future|
|error.code.es.BTRA\_0048|=La fecha hasta no puede ser futura|
|error.code.en.BTRA\_0048|=Date to cannot be future|
|error.code.es.BTRA\_0049|=El customerId no existe|
|error.code.en.BTRA\_0049|=customerId does not exist|
|error.code.es.BTRA\_0050|=La configuracion para recibir propinas esta deshabilitada para el negocio|
|error.code.en.BTRA\_0050|=Tip configuration is disabled for the business|
|error.code.es.BTRA\_0051|=El monto de la propina no debe exceder el valor del monto de la transaccion|
|error.code.en.BTRA\_0051|=The amount of the tip must not exceed the value of the amount of the transaction|
|error.code.es.BTRA\_0052|=No puede llenar ambos campos (tipAmount y percentage), solo use uno|
|error.code.en.BTRA\_0052|=Cannot fill both fields (tipAmount and percentage), just fill one|
|error.code.es.BTRA\_0053|=El ReferenceNumber no existe|
|error.code.en.BTRA\_0053|=ReferenceNumber not found|



## User Flow

<img width="468" alt="image" src="https://github.com/evertec/athmovil-javascript-api/assets/99409598/c149814c-f6df-4faa-832b-51ce56bfcbd3">



















## Services
The following services can be used to search for transactions, perform refunds and request information of multiple payments received in a given time frame.

### Search
* Method:` POST`
* Headers: `Content-Type` -	`application/json`
* Endpoint: `https://www.athmovil.com/api/v4/searchTransaction`
* Body Example:
```json
{
    "publicToken": "hdb932832klnasKJGDW90291",
    "privateToken": "JHEFEWP2048FNDFLKJWB2",
    "referenceNumber": "a387643827-fdew98ffw9fbfewkjb",
    "dailyTransactionID": "1234",
    "name": "Valeria Herrero",
    "phoneNumber": "(787) 123-4567",
    "email": "valher@gmail.com",
    "total": "1.00",
    "metadata1": "metadata1 test",
    "metadata2": "metadata2 test"
}
```
  * *Only `publicToken`, `privateToken`any other field is required to search for a transaction.*
  * *The value of phoneNumber needs to be formatted as in the provided example.*
  * *To find results the provided field value must be an exact match with the field value of at least one transaction.*
  * *Multiple fields can be used simultaneously on the request.*

* Reponse Example:
```json
{
    "transactionType": "ECOMMERCE",
    "status": "COMPLETED",
    "date": "2019-06-06 16:12:02.0",
    "referenceNumber": "402894d56b240610016b2e6c78a6003a",
    "dailyTransactionID": 1,
    "name": "Valeria Herrero",
    "phoneNumber": "(787) 123-4567",
    "email": "valher@gmail.com",
    "message": "",
    "total": 1.00,
    "tax": 1.00,
    "subtotal": 1.00,
    "fee": 0.06,
    "netAmount": 0.94,
    "totalRefundedAmount": 0.00,
    "metadata1": "metadata1 test",
    "metadata2": "metadata2 test",
    "items": [
      {
        "name": "First Item",
        "description": "This is a description.",
        "quantity": 1,
        "price": 1.00,
        "tax": 1.00,
        "metadata": "metadata test"
      },
      {
        "name": "Second Item",
        "description": "This is another description.",
        "quantity": 1,
        "price": 1.00,
        "tax": 1.00,
        "metadata":"metadata test"
      }
    ]
}
```
  * *If more than one transaction matches the provided fields all matching payments will be sent on the response as a list.*

----

### Refund
* Method:` POST`
* Headers: `Content-Type` -	`application/json`
* Endpoint: `https://www.athmovil.com/api/v4/refundTransaction`
* Body Example:
```json
{
    "publicToken": "hdb932832klnasKJGDW90291",
    "privateToken": "JHEFEWP2048FNDFLKJWB2",
    "referenceNumber": "fdew98ffw9fbfewkjb"
    "amount":"1.00"
}
```

----

### Transaction Report
* Method:` GET`
* Headers: `Content-Type` -	`application/json`
* Endpoint: `https://www.athmovil.com/transactions/v4/transactionReport`
* Body Example:
```json
{
    "publicToken": "hdb932832klnasKJGDW90291",
    "privateToken": "JHEFEWP2048FNDFLKJWB2",
    "fromDate": "2019-01-01 16:12:02",
    "toDate":"2019-06-06 16:12:02"
}
```
* Response Example:
```json
[
    {
        "transactionType": "ECOMMERCE",
        "status": "COMPLETED",
        "date": "2021-07-08 00:36:39.0",
        "referenceNumber": "402894d56b240610016b2e6c78a6003a",
        "dailyTransactionID": 35,
        "name": "Valeria Herrero",
        "phoneNumber": "(787) 123-4567",
        "email": "valher@gmail.com",
        "message": "",
        "total": 1.00,
        "tax": 0.00,
        "subtotal": 0.00,
        "fee": 0.00,
        "netAmount": 1.00,
        "totalRefundAmount": 1.00,
        "metadata1": "metadata1 test",
        "metadata2": "metadata2 test",
        "items": [
            {
                "name": "Item",
                "description": "Description",
                "quantity": 1,
                "price": 1.00,
                "tax": 0.00,
                "metadata": "Metadata"
            },
            {
                "name": "item",
                "description": "desc",
                "quantity": 1,
                "price": 1.00,
                "tax": 0.00,
                "metadata": "Metadata"
                }
            ]
        },
        {
            "transactionType": "ECOMMERCE",
            "status": "COMPLETED",
            "date": "2021-07-08 00:35:43.0",
            "referenceNumber": "402894d56b240610016b2e6c78a6003a ",
            "dailyTransactionID": 33,
            "name": "Valeria Herrero",
            "phoneNumber": "(787) 123-4567",
            "email": "valher@gmail.com",
            "message": "",
            "total": 5.00,
            "tax": 1.00,
            "subtotal": 2.00,
            "fee": 0.00,
            "netAmount": 5.00,
            "totalRefundAmount": 5.00,
            "metadata1": "metadata1 test",
            "metadata2": "metadata1 test",
            "items": [
                {
                    "name": "Nombre de arreglo",
                    "description": "Prueba de items",
                    "quantity": 3,
                    "price": 2.00,
                    "tax": 1.00,
                    "metadata": "prueba metadata"
                }
            ]
        }
    ]
```
----
## Legal

The use of this API and any related documentation is governed by and must be used in accordance with the Terms and Conditions of Use of ATH Móvi Business ®, which may be found at: <https://athmovilbusiness.com/terminos>.

## Reporting

Withing the reports of ATH Business this transaction will be registered as an ECOMMERCE type transaction. Transactions performed through the “Pay a Business” functionality within the ATHM app will register as PAYMENT type transactions.




Reporting example

<img width="468" alt="image" src="https://github.com/evertec/athmovil-javascript-api/assets/99409598/9875dfc5-ee7b-484e-864b-0f6fa2c84dcc">



## Other information** 

Additionally, if you only want to receive payments through the payment button, we recommend disabling the business from the list of business available in the ATH Móvil’s app. 

This way you guarantee you are receiving payments only through the payment button.

This change can be performed within the app via the settings.

<img width="468" alt="image" src="https://github.com/evertec/athmovil-javascript-api/assets/99409598/806314f3-2a98-48ae-9617-f649f3b8babf">


CONFIDENTIAL INFORMATION 

COPYRIGHT © 2021 EVERTEC GROUP, LLC. ALL RIGHTS RESERVED.

