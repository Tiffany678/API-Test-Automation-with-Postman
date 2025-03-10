**API Testing Using Postman**
**By Tiffany Yang**

Postman is a powerful tool for API testing and regression automation, enabling developers and testers to verify HTTP requests and ensure data processing meets specific requirements. In this article, we will explore how to use Postman to test API requests for an e-commerce bookstore. The process will simulate user interactions by sending HTTP requests to the server, covering key actions such as logging in, browsing products, completing the checkout process, and retrieving order details. These tests will demonstrate how to create GET and POST requests effectively.

**Content Breakdown:**

1. Setup

   - Collection Setup
     - Credentials Management
     - Collection variables
   - Environments Setup

2. API Automation Testing:

   - Login Validation
   - Retrieving Product Listing
   - Check Process
   - Fetching Order Details

3. Running a Collection in CLI Using Newman
4. Monitoring API Performance with Postman Monitor
5. Conclusion

# Setup

Before writing our test case, we will first create a collection and environments. Within the collection and environment, we will define a set of variables and use them throughout the test cases. This approach helps organize and group API requests across different environments, such as development and quality assurance (QA), making the testing process more efficient and scalable..

**Collection Setup**

In Postman, variables created within a collection are available throughout that collection, effectively providing project-level scope. Postman uses key-value pairs to define variables, where each variable name represents a key and can be referenced using {{key}}.
<img height="180" src="https://github.com/Tiffany678/API-Test-Automation-with-Postman/blob/main/img/Collection_Creation.png" alt="Collection Creation" width="500"/>

**Next**, we will create variables such as userName and password in the Variables tab and reuse them for authentication and the test scripts.

On the authentication tab, the testing website (a bookstore) uses Basic Auth. We will:

Step 1: Select Basic Auth as the authentication type.

Step 2: Create and assign value to the userName and password variables.
<img height="180" src="https://github.com/Tiffany678/API-Test-Automation-with-Postman/blob/main/img/Collection_Variable.png" alt="Collection Variable" width="500"/>

<img height="180" src="https://github.com/Tiffany678/API-Test-Automation-with-Postman/blob/main/img/Collection_Authorization.png" alt="Collection Authorization" width="500"/>

# **API Automation Testing**

API testing is essential for verifying the functionality, reliability, and performance of software applications. It ensures that key aspects such as data validation, error handling, and authentication within an API work as expected under various scenarios.

Below are the API endpoints that need to be tested for our bookstore application. For each endpoint, we will create a request and write test scripts to validate that the API responses behave as intended.
<img height="180" src="https://github.com/Tiffany678/API-Test-Automation-with-Postman/blob/main/img/SwaggerAPI.png" alt="Swagger API" width="500"/>
