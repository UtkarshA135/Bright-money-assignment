# Bright-money-assignment
A Django server integrated with Plaid API to fetch bank transaction details and serve bulk async requests with celery

# Problem Statement
Create a project in django rest framework and celery with following APIs exposed:
1) User signup, login , logout APIs
2) Token exchange API : An authenticated user can submit a plaid public token that he gets
post link integration.
a) This public token is exchanged for access token on the backend.
b) This initiates an async job on the backend for fetching account and item metadata
for the access token.

3) Expose a webhook for handling plaid transaction updates and fetch the transactions on
receival of a webhook.
4) Expose an api endpoint for fetching all transaction and account data each for a user.
5) Do appropriate plaid error handling

## Make and enter a virtaul env  

```sh
$ virtualenv venv
```
```sh
$ source venv/bin/activate
```  

## Install all dependencies
```sh
$ pip3 install -r requirements.txt
```
  
## Install RabbitMq (for Async Message Queue)

### Add a repository
```sh
$ - wget -O- https://dl.bintray.com/rabbitmq/Keys/rabbitmq-release-signing-key.asc | sudo apt-key add -
```
```sh
$ - wget -O- https://www.rabbitmq.com/rabbitmq-release-signing-key.asc | sudo apt-key add -
```
```sh
$ - sudo apt-get update
```

### Install
```sh
$ sudo apt-get install rabbitmq-server
```

## Setup Models  

```sh
$ python3 manage.py makemigrations
```
```sh
$ python3 manage.py migrate 
```
```sh
$ python3 manage.py runserver
```
  
## Start Celery worker  

```sh
$ celery -A Plaid_Manager_API worker -l info
```
## API END-PONTS  
  
@http://localhost:8000/  
  
### Users API [ all POST req ]: (When hitting API with Postman make sure to include Authorization in Headers)  
  
-  users/register/ -- User-Register-API  
-  users/login  -- User-Login-API  
-  users/logout/  -- User-Logout-API  
  
### Token_Exchange API  
  
[POST req]  
  
-  token_exchange/link_token/ ------- Get-Plaid-Link_Token 
-  token_exchange/get_access_token/ - Plaid-Link-Public_token (exchange public token with access_token)  
  
[GET req]  
  
-  token_exchange/home/  ------------------ Get-Public_token 
-  token_exchange/get_items/	--------------  Get-All-Items  
-  token_exchange/get_accounts/ ---------- Get-All-Accounts 
-  token_exchange/get_transactions/ -------  Get-All-Transactions  
-  token_exchange/transaction_webhook/ - Transaction-Webhook  

## Model Details:

### BankItemModel
  
-	bank_item_id
-	access_token
-	request_id
-	user  
  
### AccountModel
  
-	account_id
-	bank_item
-	balance_available
-	balance_current

### TransactionModel
  
-	transaction_id
-	account
-	amount
-	date
-	name
-	pending
  
### APILogModel

-	request_id
-	api_type
-	date_log

