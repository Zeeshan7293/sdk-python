# Authorize.Net Python SDK 

[![Build Status](https://travis-ci.org/AuthorizeNet/sdk-python.png?branch=master)]
(https://travis-ci.org/AuthorizeNet/sdk-python)

`pip install authorizenet`

*** The Python SDK is still in limited Beta testing, please [contact us](http://developer.authorize.net/support/contact_us/)  for more information ***

## Prerequisites

We'll be ensuring this SDK is compatible with Python 2.6+, 3.2+ and PyPy but for the beta release we're primarily testing against Python 2.7 so that's the current prerequisite.


## Installation
To install AuthorizeNet

`pip install authorizenet`


## Registration & Configuration

Get a sandbox account at https://developer.authorize.net/sandbox  
Set your API credentials:  

````python
	merchantAuth = apicontractsv1.merchantAuthenticationType()
	merchantAuth.name = 'YOUR_API_LOGIN_ID'
	merchantAuth.transactionKey = 'YOUR_TRANSACTION_KEY'
````

For reporting tests, go to https://sandbox.authorize.net/ under Account tab->Transaction Details API and enable it.


## Usage
See our sample code repository at https://github.com/AuthorizeNet/sample-code-python (*** NOTE during beta the sample code repo is a work in progress ***)

For the simplest "Hello World" example, paste this into a file called charge-credit-card.py and run:

````python
from authorizenet import apicontractsv1
from authorizenet.apicontrollers import *
from decimal import *

merchantAuth = apicontractsv1.merchantAuthenticationType()
merchantAuth.name = '5KP3u95bQpv'
merchantAuth.transactionKey = '4Ktq966gC55GAX7S'

creditCard = apicontractsv1.creditCardType()
creditCard.cardNumber = "4111111111111111"
creditCard.expirationDate = "2020-12"

payment = apicontractsv1.paymentType()
payment.creditCard = creditCard

transactionrequest = apicontractsv1.transactionRequestType()
transactionrequest.transactionType = "authCaptureTransaction"
transactionrequest.amount = Decimal ('1.55')
transactionrequest.payment = payment


createtransactionrequest = apicontractsv1.createTransactionRequest()
createtransactionrequest.merchantAuthentication = merchantAuth
createtransactionrequest.refId = "MerchantID-0001"

createtransactionrequest.transactionRequest = transactionrequest
createtransactioncontroller = createTransactionController(createtransactionrequest)
createtransactioncontroller.execute()

response = createtransactioncontroller.getresponse()

if (response.messages.resultCode=="Ok"):
	print "Transaction ID : %s" % response.transactionResponse.transId
else:
	print "response code: %s" % response.messages.resultCode

````

## Building and Testing Source Code

Requirements
--------------------------------------
- python 2.7
- pyxb 1.2.4


Run the following to get pyxb and nosetests:
- pip install pyxb
- pip install nosetests
- pip install Magicmock

Testing
--------------------------------------
- Tests available are: unit tests, mock tests, sample code
- use nosetests to run all unittests 
`
>nosetests
`

