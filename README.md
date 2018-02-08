Deloitte RESTful API demo.

Developer : Suresh Pritmani Date : 8-February-2018

Project : Demo for RESTful API + RAML + Mulesoft assignment : RESTful API + JSON + RAML + Mulesoft + Anypoint Studio + Anypoint Design Center

Description : 	This sample project is developed using RAML to develop RESTful API and integrated in mule using Anypoint Studio.
				The main resource of RESTful API is Customer having various operations like Insert, Update, Delete, Retrieve and Fetch list of Customers.
				Sample Customer Object = {"CustomerId":1,"FirstName":"James", "LastName":"Russ", "City":"Sydney", "Country":"Australia", "PostCode":"2000"}
Base URL :		http://127.0.0.1:8081/api

Implementation Details :
	- RAML file has been designed specifically for "Customer" resource.
	- For easy maintenance purpose, separate JSON file has been created for the examples which are being used in RAML file.
	- This RAML file is being used in Anypoint Studio to perform integration/flow.
	- Operations covered for the resources are CREATE, FETCH, UPDATE, DELETE and RETRIEVE LIST OF CUSTOMERS.	
	- Following are the use cases which are using these operations.
				
COMMENTARY ON USE CASES

[1] CONSUME API TO FETCH LIST OF CUSTOMERS PERIODICALLY
	Understanding : A Consumer will try to fetch the list of customers frequently and the frequency for the same is 5 minutes.
	URL Path : /customers (GET method)
	Description : Mule flow - CustomerProfilePeriodicCallFlow is being used to request "/customer" URL periodically (every 5 minutes) with the help of polls connector which will send HTTP request every 5 minutes. The HTTP connector will fetch the required resource and that resource will be stored as JSON response to specific location ("C:\Temp\customer_list.json"). Currently same file will be overwritten based on new request. It can be configured to create new file on each request by using unique ID with the help of MEL example #[java.util.UUID.randomUUID()].
	Response code : 200
	Response : Refer Customer_Profile_List.json
	
	
[2] CRUD OPERATION ON RESOURCE - CUSTOMER
	Understanding : Mobile application or any other consumer can perform CRUD operation via REST API. Following are the operations which can be performed by different kind of consumers.
	
	Design Approach : As this API can be used with Mobile application, off-line feature can be provided with this functionality. Considering the scenario where representative trying to update the user profile and doesn't have internet connection than inline DB can capture the changes and once the user gets internet connection, all changes can be synchronized. Similarly (mobile app) database can be synchronized during off peak hours or can be scheduled as per users' request.
	
	(a) CREATE NEW CUSTOMER
	URL Path : /customers (POST method)
	Description : This API can be used to create new customer by passing JSON body which contains customer object in JSON format. As of now, "CustomerId" is also being passed as request body, but best approach is to derive some unique id thru some business logic and use it as "CustomerId". On successful execution of api, relavant message will be shared to consumer.
	Request : Refer New_Customer.json
	Response code : 201
	Response : Insert_Customer_Success_Message.json
	
	(b) RETRIEVE CUSTOMER
	URL Path : /customers/{CustomerId} (GET method)
	Description : To fetch existing customer, this API can be used. And as per URL, consumer has to pass customer id. On successful execution of api, respective Customer Object will be shared as JSON response to the consumer. To validate the customer id extra restriction can be performed during implementation of API.
	Response code : 200
	Response : Customer_Example.json
	
	(c) UPDATE EXISTING CUSTOMER	
	URL Path : /customers/{CustomerId} (PUT method)
	Description : To update existing customer profile, this API can be used. And as per URL, consumer has to pass customer id along with JSON body which contains customer object (in JSON format) with the details which needs to be updated. On successful execution of api, relavant message will be shared to consumer. To validate the customer id extra restriction can be performed during implementation of API.
	Request : Update_Customer_Profile.json
	Response code : 202
	Response : Update_Customer_Success_Message.json
	
	(d) DELETE CUSTOMER	
	URL Path : /customers/{CustomerId} (DELETE method)
	Description : To delete existing customer, this API can be used. And as per URL, consumer has to pass customer id. On successful execution of api, relevant message will be shared to consumer. To validate the customer id extra restriction can be performed during implementation of API.
	Response code : 202
	Response : Delete_Customer_Success_Message.json

[3] EXTENSION OF API TO SUPPORT FUTURE RESOURCES
	Understanding : Considering the fact that current application may be scalled and new resources may be introduces or could be integrated with other teams who are working on different modules, keep provision of extension.
	Description : As mentioned, current resource may be extended to interact with other resources like "Orders" and "Products", the linkage between these three resources could be hierarchical, like, Customers -> Orders -> Products, the future APIs could be as follow.
	- /customers/{CustomerId}/Orders : To get list of orders belongs to particular customer.
	- /customers/{CustomerId}/Orders/{OrderId} : To get details of particular order belongs to particular customer.
	- /orders/{OrderId}/products : To get list of products belongs to particular order.
	- /orders/{OrderId}/products/{ProductId} : To get details of particular product belongs to particular order.
	
	
Best Practices :

- RESTFUL API
	- RESTful API should be mapped with Resources only.
	- All RESTful API operations should align with HTTP methods.
	- REF LINK should be used to show linkage between resources. 

- RAML	
	- JSON schema can be used to validate the inputs passed as Request Body.
	- For RAML file, examples are stored in separate JSON file for easy maintenance purpose.
	- Security authentication can be introduced.
	- Common HTTP Status code can be managed for multiple resources, few of them are already implemented.
	- Various RAML features can be used. Like:
		- Versioning feature can be used to manage frequently changed APIs
		- Data types can be used to manage resources.
		- Include feature to modularise the code and remove code redundancy as well.