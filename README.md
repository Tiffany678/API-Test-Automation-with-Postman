**API Testing Using Postman**

**By Tiffany Yang**

Postman is a powerful tool for API testing and regression automation, enabling developers and testers to verify HTTP requests and ensure data processing meets specific requirements. In this article, we will explore how to use Postman to test API requests for an e-commerce bookstore. The process will simulate user interactions by sending HTTP requests to the server, covering key actions such as logging in, browsing products, completing the checkout process, and retrieving order details. These tests will demonstrate how to create GET and POST requests effectively.

**Content Breakdown:**

1. [Test Case Template For API Testing](#Test-Case-Template-For-API-Testing)

2. [Setup](#setup)

   - Collection Setup
     - Credentials Management
     - Collection variables
   - Environments Setup

3. [API Automation Testing](#api-automation-testing)

   - Login Validation
   - Retrieving Product Listing
   - Checkout Verification
   - Fetching Order Details

4. [Running a Collection in CLI Using Newman](#running-a-collection-in-cli-using-newman)
5. [Monitoring API Performance with Postman Monitor](#monitoring-api-performance-with-postman-Monitor)
6. [Conclusion](#conclusion)

# Test Case Template For API Testing

Providing standardized templates helps maintain consistency and ensures that all necessary details are included in each test case. A well-structured template should include sections for:
• Test Case ID – A unique identifier for tracking
• Description – A brief overview of the test case
• Preconditions – Any conditions that must be met before execution
• Test Steps – A step-by-step guide to performing the test
• Expected Results – The anticipated outcome if the test passes
• Actual Results – The observed outcome after execution
• Test Case Design Strategy – The objection for the test case

<img style="width:600px; height:450px;" src="https://github.com/Tiffany678/API-Test-Automation-with-Postman/blob/main/img/API_TestCase.png"

# Setup

Before writing our test case, we will first create a collection and environments. Within the collection and environment, we will define a set of variables and use them throughout the test cases. This approach helps organize and group API requests across different environments, such as development and quality assurance (QA), making the testing process more efficient and scalable.

**Collection Setup**

In Postman, variables created within a collection are available throughout that collection, effectively providing project-level scope. Postman uses key-value pairs to define variables, where each variable name represents a key and can be referenced using {{key}}.

<img style="width:600px; height:450px;" src="https://github.com/Tiffany678/API-Test-Automation-with-Postman/blob/main/img/Collection_Creation.png" alt="Collection Creation" />

**Next**, we will create variables such as userName and password in the Variables tab and reuse them for authentication and the test scripts.

On the authentication tab, the testing website (a bookstore) uses Basic Auth. We will:

Step 1: Select Basic Auth as the authentication type.
Step 2: Create and assign value to the userName and password variables.

<img style="width:600px; height:450px;" src="https://github.com/Tiffany678/API-Test-Automation-with-Postman/blob/main/img/Collection_Variable.png" alt="Collection Variable" />
<img style="width:600px; height:450px;" src="https://github.com/Tiffany678/API-Test-Automation-with-Postman/blob/main/img/Collection_Authorization.png" alt="Collection Authorization" />

**Environment Setup**

To support testing across different phases, we will create separate environments for Development and QA. This will allow us to run the same test cases under different conditions, ensuring comprehensive API validation across different stages of the software development lifecycle.

<img style="width:600px; height:450px;" src="https://github.com/Tiffany678/API-Test-Automation-with-Postman/blob/main/img/Environments_Dev.png" alt="Collection Authorization" />
<img style="width:600px; height:450px;" src="https://github.com/Tiffany678/API-Test-Automation-with-Postman/blob/main/img/Environments_QA.png" alt="Collection Authorization" />

# **API Automation Testing**

API testing is essential for verifying the functionality, reliability, and performance of software applications. It ensures that key aspects such as data validation, error handling, and authentication within an API work as expected under various scenarios.

Below are the API endpoints that need to be tested for our bookstore application. For each endpoint, we will create a request and write test scripts to validate that the API responses behave as intended.
<img style="width:600px; height:450px;" src="https://github.com/Tiffany678/API-Test-Automation-with-Postman/blob/main/img/SwaggerAPI.png" alt="Swagger API"/>

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
   <img style="width:600px; height:450px;" src="https://github.com/Tiffany678/API-Test-Automation-with-Postman/blob/main/img/Request_GetUser.png" alt="Request GetUser"/>

**Request: Get Products**

1. Enter the request URL, <https://bookstore.qacurry.com/json/get_products/>
2. Select GET method
3. Add test scripts

```sh
// Retrieve data in json format
const jsonData = pm.response.json();

// Check if the status code is 200 (OK)
pm.test("Get Products - Success", function () {
   pm.response.to.have.status(200);
});

// Check if the responsTime is less than 0.2 seconds
pm.test("Response time is less than 200ms", function () {
    pm.expect(pm.response.responseTime).to.be.below(200);
});

// Check if the response has at least one product
pm.test("Get Products - Success", function () {
   pm.expect(jsonData).to.be.an("array");
   pm.expect(jsonData).to.have.lengthOf.at.least(1);
});
```

4. Click the send button to execute the request.
5. Once verified, click the Save button to store the request in your collection.

<img style="width:600px; height:450px;"
src="https://github.com/Tiffany678/API-Test-Automation-with-Postman/blob/main/img/Request_GetProducts.png" alt="Request Get Products"/>

**Request: Post an Order**

1. Insert the url, <https://bookstore.qacurry.com/json/post_order/>
2. Select the POST method.
3. Add test scripts

```sh
// Create a new post
pm.test("Order placed successfully", async ()=> {
    const jsonData = await pm.response.json()

    // Check the status Code
    pm.response.to.have.status(200);

   // Store Order_id variable to collection
   pm.collectionVariables.set("Order_id", jsonData[0]["Order ID"]);

   // Print in the console and get the order id
   console.log("Stored Order ID in Collection Variable:", pm.collectionVariables.get("Order_id"));

  // check response body
  pm.expect(jsonData[0]).to.have.property("Order ID"); // Use exact key
});
```

4. Navigate to the Body tab and insert the required JSON data.

```sh
[
 {
   "quantity": 1,
   "product_id": 1
 },
 {
   "quantity": 2,
   "product_id": 2
 }
]
```

5. Click the Send button to execute the request and review the response.
6. Once verified, click the Save button to store the request in your collection for future use.

<img style="width:600px; height:450px;"
src="https://github.com/Tiffany678/API-Test-Automation-with-Postman/blob/main/img/Request_PostOrder_Body.png" alt="Request Post Order Body"/>
<img style="width:600px; height:450px;"
src="https://github.com/Tiffany678/API-Test-Automation-with-Postman/blob/main/img/Request_PostOrder_Script.png" alt="Request Post Order"/>

**Request: Get the New Order Detail**

1. Insert the URL, https://bookstore.qacurry.com/json/get_order/?order_id={{Order_id}}
2. Select GET method
3. Add test scripts

```sh
pm.response.to.have.status(200);
var jsonData = pm.response.json();
pm.test("Get Order Details - Success", function () {
   pm.expect(jsonData[0]).to.have.property("order_id");
});
pm.test("Get Order Details - Success", function () {
     pm.expect(jsonData).to.be.an("array");
});
pm.test("Get Order Details - Success", function () {
pm.expect(jsonData[0].order_id).to.eql(String(pm.collectionVariables.get("Order_id")));
});
```

4. Click the send button to execute the request.
5. Once verified, click the Save button to store the request in your collection.
   <img style="width:600px; height:450px;"
   src="https://github.com/Tiffany678/API-Test-Automation-with-Postman/blob/main/img/Request_GetOrderDetail.png" alt="Request_GetOrderDetail"/>

**Alternative: Running All Requests at Once**

Instead of testing each request individually, Postman allows you to run all HTTP requests in a collection simultaneously.

Step 1: Run the Collection

1. Select the collection in Postman.
2. Click the Run button to open the Collection Runner.

Step 2: Configure the Run Settings

1. Choose the desired execution options based on your testing needs.
2. (Optional) Upload a mock data file to simulate different API request scenarios.
3. Click Run Collection to execute all requests at once.
   <img style="width:600px; height:450px;"
   src="https://github.com/Tiffany678/API-Test-Automation-with-Postman/blob/main/img/RunCollection_Run1.png" alt="RunCollection"/>
   <img style="width:600px; height:450px;"
   src="https://github.com/Tiffany678/API-Test-Automation-with-Postman/blob/main/img/RunCollection_Run2.png" alt="RunCollection"/>
   <img style="width:600px; height:450px;"
   src="https://github.com/Tiffany678/API-Test-Automation-with-Postman/blob/main/img/RunCollection_Result.png" alt="RunCollection"/>

# **Running a Collection in CLI Using Newman**

Newman is a command-line tool that enables you to execute and test Postman collections automatically. It supports seamless integration with CI/CD pipelines, allowing for automated API testing as part of the development workflow. Additionally, Newman provides custom report generation, providing insights into test execution results.

**Steps to Generate Automated Test Reports in CLI and Export as an HTML File**

**Step 1: Install Newman and HTML Report Globally**

1. Open a terminal or command prompt and install Newman:

```sh
npm install -g newman
```

1. Verify the installation:

```sh
Newman –version
```

1. Install the HTML reporter for generating reports:

```sh
npm install -g newman-reporter-html
```

**Step 2**: **Generate Test Report**

1. In Postman, go to the Collections panel in the left sidebar.
2. Click the three dots next to the “BookStore” collection and select Export
3. Open a terminal or command prompt
4. Run collection and display results in CLI:

```sh
newman run BookStore.json
```

5. Generate an HTML report in the current directory:

```sh
newman run BookStore.json -r html --reporter-html-export cli_Report.html
```

<img style="width:600px; height:450px;"
src="https://github.com/Tiffany678/API-Test-Automation-with-Postman/blob/main/img/Newman_Explore.png" alt="Newman Explore"/>
<img style="width:600px; height:450px;"
src="https://github.com/Tiffany678/API-Test-Automation-with-Postman/blob/main/img/Newman_CLI.png" alt="Newman CLI"/>

# **Monitoring API Performance with Postman Monitor**

Postman Monitor enables continuous analysis and observation of API performance at regular intervals in both the development and production environments. It helps identify potential issues such as high response times, errors, outages, and automatically sends notifications to developers, allowing for quick issue resolution.

**Step 1: Create a New Monitor**

1. In Postman, navigate to the Monitor section on the left sidebar.
2. Click Create Monitor, enter a meaningful name for the monitor, and select the collection you want to track.

**Step 2: Configure and Activate the Monitor**

1. Set the monitoring schedule by defining the interval at which API tests should run.
2. Configure environment variables if needed.
3. Click Create Monitor to finalize and activate the monitoring process.
   <img style="width:600px; height:450px;"
   src="https://github.com/Tiffany678/API-Test-Automation-with-Postman/blob/main/img/Monitor_Creation.png" alt="Monitor Creation"/>
   <img style="width:600px; height:450px;"
   src="https://github.com/Tiffany678/API-Test-Automation-with-Postman/blob/main/img/Monitor_Result.png" alt="Monitor Result"/>

# **Conclusion**

Postman is a powerful and versatile tool for automating API testing. It offers a wide range of features that simplify the process of creating, executing, and managing automated API tests. By automating API testing with Postman, developers and testers can ensure more comprehensive test coverage while significantly reducing time and effort spent on manual testing.
