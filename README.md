**Overview**  
<b>This is a rest api implementation using JAX-RS Jersey framework. No Tomcat needed as it is standalone app and has a database as well.</b>  
**Note: Requires MySQL Instance.**  
**It is possible to insert seed data.**  
**Adjust hibernate cfg file content according to your MySQL db instance.**  

1. Go to src/main/resources/hibernate.cfg.xml and adjust the entry values for the DB, userName, password, hibernate.hbm2ddl.auto  
For the very first run, make hibernate.hbm2ddl.auto as create and then change it once schema is created.  

2. There is also a possibility of inserting seed data in the database. This is done by running the main method in CustomerDAO class.  
NOTE: Be careful, the main method will create 56 customer records in the database.  
The actual customer creation is done by createMultipleCustomers() method in CustomerDAO class.  

3. To start the project, go to Main.java in (org.abhishek.customerapi) and run main method.  
You should then see something like this:  
Jersey app started with endpoints available at http://localhost:8080/  
Now the api is running, and you could use postman to try the endpoints it offers.  

4. Project developed using JDK 8, HK2 injection, Jersey Grizzly (no need of Tomcat), mapstruct for entity to dto conversion and vice versa, Hibernate to save data to database, **MySQL 8** database instance.  


**API End Points**  
Entry to the api is via the endpoint: http://localhost:8080/customer-api/v2  

Available end points (mostly happy paths have been implemented and a few negative paths as well):  
**Branch java8refactoring has the urls:**  
a) /register: (POST) http://localhost:8080/customer-api/v2/customer/register  
b) /allCustomers: (GET) http://localhost:8080/customer-api/v2/customer/allCustomers?page=2  
Paginated customer list from the Database. It will return first 10 records as page 1. Provide query parameter "page=2" to get the page 2 result set.  
c) /deregister: (DELETE) http://localhost:8080/customer-api/v2/customer/deregister/johndoe@ema.com  
d) /customer?emailAddress: (GET)http://localhost:8080/customer-api/v2/customer?emailAddress=johndoe%40ema5.com  
e) /dateOfBirthGreaterThan: (GET) http://localhost:8080/customer-api/v2/customer/dateOfBirthGreaterThan/2000-01-01
f) /update: (PUT): http://localhost:8080/customer-api/v2/customer/update


**Minor Info**  
HTTP Status code is also sent in response.  
Also one header is sent in some cases (just for trial)  

**Example JSON**  
POST request:  
{
    "customerName": "John Doe",
    "emailAddress": "johndoe4@gmail.com",
    "phoneNumber": "0046739182157",
    "dateOfBirth": "2012-10-20"
}

GET Response:  
{
    "active": true,
    "customerName": "John Doe II",
    "dateOfBirth": "2010-06-12",
    "emailAddress": "johndoe3@gmail.com",
    "phoneNumber": "001800500400"
}
    
PUT request:
{
    "emailAddress": "johndoe@ema.com",
    "dateOfBirth": "2010-06-12",
    "phoneNumber": "0046777555666"
}

**Rainy Day Scenarios**
1. If a customer doesn't exist while searching, removing(de-registering) then return proper exception. (Works in postman and rest client in the test class)
2. If there are no customers matching the dateOfBirth search then return proper exception (Works in postman)  

**Requirements**

REQ-2022-11-12-001: Saving the customer profile data.

Requirement Added on: 2022-11-12

Requirement Details:
In this iteration, we will save customer's email address, customer name, phone, timestampcreated, active(true by default), phone number in database.

Implementation Method Name:

Test Cases:

Test Methods:

Tech Note:

Status: To do/Done
Requirement Finished On:

*****************************
REQ-2022-11-12-002: Fetch customer using email address

Requirement Added on: 2022-11-12

Business Requirement: We need to fetch customer using email address.
Properties to be shown to the client are email address, customer name, active, phonenumber.
We are not showing timestampcreated to the client.

Implementation Method Name:

Test Cases:

Test Methods:

Tech Note:
Negative scenraios handled:
a) If there is no customer recored in DB matching the email then DAO will return empty optional and it will reach controller/resource and resource will show message in the response.  

BUG:
BUG_01: (To implement)When customer doesn't has a phone number and/or date of birth then these fields are not returned in the postman response. How about rest client?

Status: To do/Done
Requirement Finished On:
**************************************
REQ-2022-11-12-003: Customer will have a field for date of birth. It is optional data for customer profile.
Customer can update profile and update either phone number or date of birth or both in a single transaction.

Requirement Added on: 2022-11-12

Business Requirement: This date of birth will later be used to give discounts to the customer.

Implementation Method Name:

Test Cases:

Test Methods:

Tech Note:

Status: To do/Done
Requirement Finished On:
**************************************
REQ-2022-11-15-004: Instead of just sending back the DTO class, we want to send Response with Http Status, Http header, and corresponding entity. Implement Response. 

Requirement Added on: 2022-11-15

Business Requirement:

Implementation Method Name:

Test Cases:

Test Methods: 2 test methods. 1st to check positive case when customer exists and 2nd for the case when customer doesn't exist in DB
test_search_a_customer_using_existing_emailAddress_should_return_response_object_And_Customer
test_search_a_customer_using_nonexisting_emailAddress_should_return_response_object_with_ErrorMessageObjectInstance()


Tech Note: Currently implemented as an extra method for the fetchCustomerByEmailReturnResponse() in class CustomerResource

Status: To do/Done
Requirement Finished On:
**************************************
REQ-2022-11-15-005: When api receives a request for all customers, then it should default to pagination.

Requirement Added on: 2022-11-15

Business Requirement: 1st 10 customers should be returned with a link in the response to fetch next set of customers

Implementation Method Name:

Test Cases:

Test Methods:

Tech Note: Using criteria api to fetch the paginated customer list

Status: To do/Done
Requirement Finished On:
**************************************
REQ-2022-11-16-006: Customer registration. A customer could register using email address. If the customer already exists with provided email address, then system will just return the matching customer.

Requirement Added on: 2022-11-16

Business Requirement: Create new customer or return an existing customer using email address.

Implementation Method Name:

Test Cases:

Test Methods:

Tech Note: What HTTP status code should be returned when a customer is newly created? 201-CREATED
What HTTP status code should be returned when a customer is already existing? TBD

Status: To do/Done
Requirement Finished On:
**************************************
REQ-2022-11-16-007: When de-registering a customer, if the customer does not exist then return an exception to the client stating that the customer was not found.

Requirement Added on: 2022-11-16

Business Requirement: Detailed Requirement

Implementation Method Name:

Test Cases:

Test Methods:

Tech Note: From DAO class throwing DataNotFoundException and it shows correctly in the response of Postman

Status: To do/Done
Requirement Finished On:
**************************************
REQ-2022-11-21-008: It should be possible to search customers whose date of birth is greater than a certain date 

Requirement Added on: 2022-11-21

Business Requirement: Detailed Requirement

Implementation Method Name:

Test Cases:

Test Methods:

Tech Note: the input date has to be in format yyyy-mm-dd

Status: Done
Requirement Finished On:
**************************************
REQ-2022-11-21-009: Return the location of the created resource/entity when a customer is registered

Requirement Added on: 2022-11-21

Business Requirement: Returning the location of created resource using uriInfo

Implementation Method Name:

Test Cases:

Test Methods:

Tech Note: 
URI createdUri = uriInfo.getBaseUriBuilder()
			.path(CustomerResource.class)
			.path(CustomerResource.class, "fetchCustomerByEmailReturnResponse")
			.queryParam("emailAddress", createdCustomerDTO.getEmailAddress()).build();

Status: To do/Done
Requirement Finished On:
**************************************
REQ-2022-11-15-005: Brief requirement one-liner

Requirement Added on: YYYY-MM-DD

Business Requirement: Detailed Requirement

Implementation Method Name:

Test Cases:

Test Methods:

Tech Note: 

Status: To do/Done
Requirement Finished On:
**************************************
REQ-2022-11-15-005: Brief requirement one-liner

Requirement Added on: YYYY-MM-DD

Business Requirement: Detailed Requirement

Implementation Method Name:

Test Cases:

Test Methods:

Tech Note: 

Status: To do/Done
Requirement Finished On:
**************************************
REQ-2022-11-15-005: Brief requirement one-liner

Requirement Added on: YYYY-MM-DD

Business Requirement: Detailed Requirement

Implementation Method Name:

Test Cases:

Test Methods:

Tech Note: 

Status: To do/Done
Requirement Finished On:
**************************************
REQ-2022-11-15-005: Brief requirement one-liner

Requirement Added on: YYYY-MM-DD

Business Requirement: Detailed Requirement

Implementation Method Name:

Test Cases:

Test Methods:

Tech Note: 

Status: To do/Done
Requirement Finished On:
**************************************
REQ-2022-11-15-005: Brief requirement one-liner

Requirement Added on: YYYY-MM-DD

Business Requirement: Detailed Requirement

Implementation Method Name:

Test Cases:

Test Methods:

Tech Note: 

Status: To do/Done
Requirement Finished On:
**************************************
