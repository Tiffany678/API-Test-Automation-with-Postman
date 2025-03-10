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

<img style="width:500px; height:250px;" src="https://github.com/Tiffany678/API-Test-Automation-with-Postman/blob/main/img/Collection_Creation.png" alt="Collection Creation" />

**Next**, we will create variables such as userName and password in the Variables tab and reuse them for authentication and the test scripts.

On the authentication tab, the testing website (a bookstore) uses Basic Auth. We will:

Step 1: Select Basic Auth as the authentication type.

Step 2: Create and assign value to the userName and password variables.
<img style="width:500px; height:250px;" src="https://github.com/Tiffany678/API-Test-Automation-with-Postman/blob/main/img/Collection_Variable.png" alt="Collection Variable" />

<img style="width:500px; height:250px;" src="https://github.com/Tiffany678/API-Test-Automation-with-Postman/blob/main/img/Collection_Authorization.png" alt="Collection Authorization" />

# **API Automation Testing**

API testing is essential for verifying the functionality, reliability, and performance of software applications. It ensures that key aspects such as data validation, error handling, and authentication within an API work as expected under various scenarios.

Below are the API endpoints that need to be tested for our bookstore application. For each endpoint, we will create a request and write test scripts to validate that the API responses behave as intended.

<img style="width:500px; height:250px;" src="https://github.com/Tiffany678/API-Test-Automation-with-Postman/blob/main/img/SwaggerAPI.png" alt="Swagger API"/>

**Create test requests for each API endpoint.**

**Request: Login Verification**

1. Enter the request URL, <https://bookstore.qacurry.com/json/get_login/>
2. Select GET method
3. Add test scripts

```sh
// Check if the status code is 200 (OK)
pm.test("Login successful", function () {
 pm.response.to.have.status(200);
});

// Check if the response has the one user
pm.test("Response has the login user", function(){
  const jsonData = pm.response.json()
  pm.expect(jsonData).to.be.an("array").and.to.have.lengthOf(1)
})

// Ensuring the user is not null
pm.test("Response has the login user", function(){
  const jsonData = pm.response.json()
  pm.expect(jsonData[0]).to.have.property("id")
})

```

4. Click the send button to execute the request.
5. Once verified, click the Save button to store the request in your collection.

<img style="width:500px; height:250px;" src="https://github.com/Tiffany678/API-Test-Automation-with-Postman/blob/main/img/Request_GetUser.png" alt="Request GetUser"/>

**Request: Get Products**

1. Enter the request URL, <https://bookstore.qacurry.com/json/get_products/>
2. Select GET method
3. Add test scripts

```sh
pm.test("Get Products - Success", function () {
  pm.response.to.have.status(200);
  var jsonData = pm.response.json();
  pm.expect(jsonData).to.be.an("array");
  pm.expect(jsonData).to.have.lengthOf.at.least(1);
});
```

4. Click the send button to execute the request.
5. Once verified, click the Save button to store the request in your collection.

<img style="width:500px; height:250px;"
src="https://github.com/Tiffany678/API-Test-Automation-with-Postman/blob/main/img/Request_GetProducts.png" alt="Request Get Products"/>

Request: Post an Order

1. Insert the url, <https://bookstore.qacurry.com/json/post_order/> and select Post method
2. Select the POST method.
3. Add test scripts (as shown in the image below or refer to the text file in this repository).
4. Navigate to the Body tab and insert the required JSON data.
5. Click the Send button to execute the request and review the response.
6. Once verified, click the Save button to store the request in your collection for future use.
