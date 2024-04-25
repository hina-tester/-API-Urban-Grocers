This Postman prject is based on given documents
Requirements: Shipping Price Calculations 1
🚢
Requirements: Shipping Price Calculations
Each delivery service has its own endpoint.
A request to the delivery service URL returns the following parameters:
"name": Service name
"isItPossibleToDeliver": Possibility of delivery (true/false)
"hostDeliveryCost": Shipping price for us
"toBeDeliveredTime": Delivery time range
"clientDeliveryCost": Shipping price for the client
name — always the same when the same delivery service is addressed. Values are taken from the parameters
table in the requirements.
toBeDeliveredTime — the time range when the service will make the delivery. This is always the same when the
same delivery service is addressed. Parameter values are in the table in the requirements.
In the request itself, deliveryTime is the time when the user expects their delivery, in hours.
isItPossibleToDeliver — true when the time in deliveryTime is within the range of the delivery service's working
hours, and false when it's not.
hostDeliveryCost — the price of internal delivery is calculated according to the table below. In all other cases,
hostDeliveryCost will be equal to 7 .
Name Operating Hours
Number of
products in the
order (pcs)
Order weight Delivery time
Shipping price
($) Data format
Speedy 08-22 1-10 0-3 kg 30-35 minutes 4 JSON
Speedy 08-22 11-15 3.1-7 kg 30-35 minutes 7 JSON
Fast Delivery 07-21 1-7 0-2.5 kg 25-30 minutes 3 XML
Fast Delivery 07-21 8-14 2.6-6 kg 25-30 minutes 6 XML
Food Service 06-20 1-12 0-3.5 kg 25-30 minutes 5 SOAP
Food Service 06-20 13-20 3.6-9 kg 25-30 minutes 7 SOAP
Order and Go 08-22 1-9 0-3 kg 20-25 minutes 3 JSON
Order and Go 08-22 10-15 3.1-6 kg 20-25 minutes 5 JSON
The cost is calculated using the variables passed in productsCount (number of products in the order) and productsWeight
(order weight).
Let’s take the "Speedy" delivery service as an example:
IF productsCount is less than or equal to 10 pcs.
AND productsWeight is less than or equal to 3 kg
THEN hostDeliveryCost will be equal to 4 .
Requirements: Shipping Price Calculations 2
clientDeliveryCost — the shipping price for the client is calculated according to the table.
The cost is calculated using the variables passed in productsCount (number of products in the order) and
productsWeight (order weight).
Shipping price for the client can be equal to 0 and 9.
It will be 9 if at least one of the following conditions is met:
The maximum number of products is exceeded.
The maximum weight is exceeded.
Please refer to the hostDeliveryCost table above to see the limits for weight and quantity. Anything that is above the
value listed in the table qualifies as exceeding the maximum limit.
For example, for the "Speedy" delivery service:
IF productsWeight is greater than 7 kg (maximum value in the table)
OR productsCount is greater than 15 pcs. (maximum value in the table)
THEN clientDeliveryCost will be equal to 9 .
In all other cases, clientDeliveryCost will be equal to 0 .
The "Order price less than $7" condition applies to the product order endpoint, not the delivery service endpoint.
******************************************************************************************************************************
Task description
	Analyze these two requirements: “Working with kits” and “Working with deliveries”. They can be found below under the “Requirements to be tested heading”.
 If you need a reminder on API requirement analysis, refer to the lessons on designing API test cases (Part 1, Part 2) and API Validation.
	Design tests cases to cover the features you received for testing. The test cases should be added to the Google Sheets template for this project.
 To make sure you fill out the template according to QA best practices, review the following lesson: Designing API Test Cases Part 3.
	Run your test cases through Postman and create bug reports in Jira for any bugs you find.
	Write a test report. What can you tell the team about the status of the part of the product you tested? (This task won’t affect the result of the final project but it will help you practice.)
Your Server
As usual, you’ll need a server for testing. You may also want to run it when writing tests, but, in reality, you shouldn’t need to.

ASK ME FOR THE SERVER LINK
Requirements to be tested
Let's quickly go through the requirements you'll need to test and give some guidance on designing test cases for them.
💻 Requirement 1: Working with kits
The user should be able to add existing products to a kit using the endpoint POST URL + /api/v1/kits/:id/products unless the number of products in the kit exceeds the limit of 30.
When adding non-existent product IDs, 400 Bad Request should be returned.
When adding products to a non-existing kit ID, 404 Not Found should be returned.
In cases when the request body does not contain the right structure, 400 Bad Request should be returned.
💻 Requirement 2: Working with deliveries
The “Fast Delivery” service should be available if the Shipping Price Calculation Requirements are met.
The endpoint is /fast-delivery/v3.1.1/calculate-delivery.xml. See the “Couriers” part of apiDoc for information about the request structure.
Tips and Hints
When designing tests for Requirement 1, make sure you cover:
	The ability to add products to a kit
	Positive and negative cases of the :idpath variable in the URL (ID of the kit where you want to add products)
	Positive and negative cases of the “id” body parameter (id of a product that is added)
	Positive and negative cases of the “quantity” body parameter
	Positive and negative cases for the “productList” body parameter
💡 Access apiDoc to find out which parameters you need to pass and view an example of the request body.
☝ To test both positive and negative cases for the kit ID and the product ID, you might want to know the number of products and kits the database and their IDs.
You can access the database file with Urban Groceries products at: URL + /api/db/resources/product_model.csv
You can access the database file with Urban Groceries products at: URL + /api/db/resources/kit_model.csv

Before you start testing, you should have written 14-20 test cases for Requirement 1.
When designing tests for Requirement 2, make sure you cover:
	Positive and negative cases of the “deliveryTime” parameter
	Positive and negative cases of the “produtcsWeight” parameter
	Positive and negative cases of the “produtcsCount” parameter

💡 In the Shipping Price Calculation Requirements, you will find the requirement for every parameter that will help you identify the input values for positive and negative tests.
Before you start testing, you should have 19-28 test cases for Requirement 2.


