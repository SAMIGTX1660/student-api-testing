# 🧪 RESTful Student API — Postman Automation Testing

> **API Testing • Postman • Newman • JavaScript Assertions • Test Automation**

A REST API testing project built with **Postman** to automate functional and basic non-functional testing of the [TestingWorld Student API](https://thetestingworldapi.com/).

The project demonstrates how API test cases can be organized into a reusable Postman collection, validated with JavaScript assertions, executed from the command line using **Newman**, and integrated into an automated testing workflow.

---

## 📌 Project Overview

This project focuses on testing a Student REST API through a sequence of automated API requests.

The collection validates:

* HTTP status codes
* Response time
* Response size
* Response body fields
* Required fields
* Returned student ID
* Data consistency between requests
* Date format
* Dynamic test data
* Request chaining using Postman environment variables

The Postman collection is exported using the **Postman Collection v2.1 schema** and contains seven automated requests.

---

## 🎯 Objectives

The main objectives of this project are to:

1. Automate REST API functional testing.
2. Validate HTTP response status codes.
3. Verify response payloads using JavaScript assertions.
4. Validate response performance.
5. Validate response size.
6. Generate dynamic test data.
7. Pass data between dependent API requests.
8. Execute Postman tests through Newman.
9. Generate an automated HTML test report.
10. Identify API defects or unexpected API behavior through assertions.

---

## 🛠️ Technologies & Tools

| Technology / Tool | Purpose                                       |
| ----------------- | --------------------------------------------- |
| **Postman**       | API development and test automation           |
| **JavaScript**    | Writing test and pre-request scripts          |
| **Newman**        | Command-line execution of Postman collections |
| **REST API**      | System under test                             |
| **JSON**          | Request and response data                     |
| **Node.js / npm** | Newman installation and execution             |
| **HTML Report**   | Test execution reporting                      |

---

## 🔗 API Under Test

**Base URL**

```text
https://thetestingworldapi.com/api
```

The base URL is stored as the `base_url` environment variable rather than being hard-coded throughout the collection.

---

# 📂 Project Structure

```text
Restful_Student_API_Testing/
│
├── API testing.postman_collection.json
├── Automated.postman_environment.json
├── newman/
│   └── newman-run-report.html
│
└── README.md
```

### Main Files

**`API testing.postman_collection.json`**

Contains the API requests, pre-request scripts, test scripts, assertions, request bodies, and request chaining logic.

**`Automated.postman_environment.json`**

Contains the Postman environment variables used throughout the automated workflow, including `base_url`, `student_id`, student information, technical-skill variables, and address-related variables.

**Newman Report**

Contains the results of command-line execution, including passed/failed assertions, response times, response sizes, and failure details.

---

# 🔄 API Test Workflow

The collection follows a logical student-management workflow:

```text
┌─────────────────────┐
│  Create Student     │
│       POST          │
└──────────┬──────────┘
           │
           │ student_id
           ▼
┌─────────────────────┐
│ Get Final Student   │
│       GET           │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ Get Specific Student│
│       GET           │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│  Update Student     │
│       PUT           │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ Create Technical    │
│      Skills         │
│       POST          │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ Create Student      │
│      Address        │
│       POST          │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ Get All Students    │
│       GET           │
└─────────────────────┘
```

---

# 🧪 Test Cases

## 1. Create Student

**Method:** `POST`

```text
/studentsDetails
```

Creates a new student using dynamically generated information.

The request uses:

* First name
* Middle name
* Last name
* Date of birth

The request body is populated using environment variables.

### Assertions

* Response time < 1000 ms
* Status code = `201`
* Response size < 1000 bytes
* Student ID is not null
* First name is not empty
* Middle name is not empty
* Last name is not empty
* Date of birth is not empty
* Returned values match generated test data
* Date of birth follows `DD-MM-YYYY`

The generated student ID is stored in the `student_id` environment variable for subsequent requests.

---

## 2. Final Student Information

**Method:** `GET`

```text
/FinalStudentDetails/{{student_id}}
```

Retrieves the complete student information using the ID generated during student creation.

The endpoint dynamically receives the `student_id` from the Postman environment.

### Assertions

The response is checked for:

* Required student fields
* Correct student ID
* Correct first name
* Correct middle name
* Correct last name
* Correct date of birth
* Correct date format

---

## 3. Get Specific Student

**Method:** `GET`

```text
/studentsDetails/{{student_id}}
```

Retrieves a specific student using the dynamically generated student ID.

### Assertions

* Required fields are present
* Student ID is correct
* First name is correct
* Middle name is correct
* Last name is correct
* Date of birth is correct
* Date format is valid

---

## 4. Update Student

**Method:** `PUT`

```text
/studentsDetails/{{student_id}}
```

Updates the student information using dynamically generated values.

The request body includes the student ID and updated student information.

### Assertions

* Response time < 1000 ms
* Status code = `200`
* Response size < 1000 bytes

---

## 5. Create Technical Skills

**Method:** `POST`

```text
/technicalskills
```

Creates technical-skill information associated with a student.

The pre-request script generates random years of experience and a student-related ID.

### Assertions

* Response time < 1000 ms
* Status code = `201`
* Response size < 1000 bytes

---

## 6. Create Student Address

**Method:** `POST`

```text
/addresses/{{student_id}}
```

Creates address information for the student.

The request dynamically generates:

* City
* State
* Country
* Phone numbers
* Standard code
* Home phone
* Mobile phone

The request body uses these generated environment variables.

### Assertions

* Response time < 1000 ms
* Status code = `201`
* Response size < 1000 bytes

---

## 7. Get All Students

**Method:** `GET`

```text
/studentsDetails
```

Retrieves the available student records from the API.

### Assertions

* Response time < 1000 ms
* Status code = `200`
* Response size < 1000 bytes

---

# ⚙️ Dynamic Test Data

One of the important features of this project is the use of **dynamic test data**.

The collection uses Postman's dynamic variables such as:

```text
{{$randomFirstName}}
{{$randomLastName}}
{{$randomCity}}
{{$randomState}}
{{$randomCountry}}
{{$randomInt}}
{{$randomPhoneNumber}}
```

The generated values are stored in environment variables before requests execute.

For example:

```javascript
var firstname = pm.variables.replaceIn('{{$randomFirstName}}');
pm.environment.set("fname", firstname);
```

A dynamic date is also generated using Moment:

```javascript
var check = require('moment');
var checkin = check().format('DD-MM-YYYY');
pm.environment.set("checkin", checkin);
```

This allows the tests to avoid relying entirely on fixed input data.

---

# 🔗 Request Chaining

The collection demonstrates **data-driven request chaining**.

The student creation request extracts the generated student ID from the response:

```javascript
var responsebody = pm.response.json();

pm.environment.set("student_id", responsebody.id);
```

The ID can then be reused in subsequent requests:

```text
{{student_id}}
```

This creates a dependency between API requests and allows the collection to behave more like a real end-to-end workflow rather than a collection of isolated API calls.

---

# 🧠 Automated Assertions

The project uses Postman's JavaScript-based assertion framework.

Example:

```javascript
pm.test("Check if status is 200", function () {
    pm.response.to.have.status(200);
});
```

Response-time validation:

```javascript
pm.test("Check if time less than 1000ms", function () {
    pm.expect(pm.response.responseTime).to.be.below(1000);
});
```

Response-size validation:

```javascript
pm.test("Response size is below 1000 bytes", function () {
    pm.expect(pm.response.responseSize).to.be.below(1000);
});
```

The tests therefore cover more than simply checking whether an API request returned a response.

---

# 🚀 Running the Tests

## 1. Install Node.js

Install Node.js if it is not already installed.

Verify the installation:

```bash
node --version
npm --version
```

---

## 2. Install Newman

Install Newman globally:

```bash
npm install -g newman
```

Verify:

```bash
newman --version
```

Expected version used for this project:

```text
Newman v6.2.2
```

---

## 3. Run the Postman Collection

From the project directory:

```bash
newman run "API testing.postman_collection.json" -e "Automated.postman_environment.json"
```

---

## 4. Generate an HTML Report

For a detailed HTML report:

```bash
newman run "API testing.postman_collection.json" -e "Automated.postman_environment.json" -r cli,html
```

The generated report can then be opened in a browser for detailed analysis.

---

# 📊 Latest Newman Test Results

The following results were obtained from the project's Newman execution:

| Metric                |      Result |
| --------------------- | ----------: |
| Collection            | API testing |
| Newman Version        |       6.2.2 |
| Iterations            |           1 |
| Requests              |           7 |
| Failed Requests       |           0 |
| Pre-request Scripts   |           4 |
| Test Scripts          |           7 |
| Total Assertions      |          45 |
| Passed Assertions     |          41 |
| Failed Assertions     |           4 |
| Execution Time        | 2.4 seconds |
| Total Data Received   |    ~11.8 KB |
| Average Response Time |      308 ms |

### Overall Result

```text
41 / 45 Assertions Passed

Pass Rate: 91.11%
```

> **Note:** The collection completed all seven requests without a request-level failure, but four individual assertions failed. This distinction is important: the API requests were executed successfully, while some expected behaviors did not match the actual responses.

---

# ❌ Failed Assertions

The current Newman execution produced four assertion failures.

### 1. Final Student Information — ID Validation

**Expected:**

```text
10893407
```

**Actual:**

```text
undefined
```

The test expected the returned student ID to match the environment's `student_id`, but the response structure did not provide the expected value at the tested location.

---

### 2. Create Technical Skills — Status Code

**Expected:**

```text
201 Created
```

**Actual:**

```text
400 Bad Request
```

This indicates that the API rejected the submitted technical-skills request instead of creating the resource.

---

### 3. Create Student Address — Status Code

**Expected:**

```text
201 Created
```

**Actual:**

```text
200 OK
```

The request completed successfully from an HTTP perspective, but the API returned `200` instead of the status code expected by the automated test.

---

### 4. Get All Students — Response Size

**Expected:**

```text
< 1000 bytes
```

**Actual:**

```text
11487 bytes
```

The API returned approximately **11.2 KB**, exceeding the test's configured 1000-byte response-size threshold.

This does not necessarily mean that the API itself is broken. It indicates that the current performance/response-size requirement does not match the observed behavior of the endpoint.

---

# 📈 Request Performance

| Request                   | Method | Avg. Response Time | Status |
| ------------------------- | ------ | -----------------: | -----: |
| Create Student            | POST   |             454 ms |    201 |
| Final Student Information | GET    |            1129 ms |    200 |
| Get Specific Student      | GET    |              78 ms |    200 |
| Update Student            | PUT    |             128 ms |    200 |
| Create Technical Skills   | POST   |             129 ms |    400 |
| Create Student Address    | POST   |             126 ms |    200 |
| Get Student               | GET    |             113 ms |    200 |

The collection uses a **1000 ms response-time threshold** in its automated assertions.

---

# 🧩 Environment Variables

The automated environment contains variables used for request chaining and dynamic test data.

| Variable      | Purpose                              |
| ------------- | ------------------------------------ |
| `base_url`    | API base URL                         |
| `student_id`  | Dynamically generated student ID     |
| `fname`       | Generated first name                 |
| `mname`       | Generated middle name                |
| `lname`       | Generated last name                  |
| `checkin`     | Generated date of birth              |
| `yearexp`     | Random years of experience           |
| `st_id`       | Generated technical-skill student ID |
| `city`        | Generated city                       |
| `state`       | Generated state                      |
| `country`     | Generated country                    |
| `std_code`    | Generated standard code              |
| `home_phone`  | Generated home phone                 |
| `phone`       | Generated mobile phone               |
| `home_code`   | Generated house number               |
| `home_phone2` | Generated second home phone          |
| `phone2`      | Generated second mobile phone        |

These variables are defined in the exported `Automated` environment.

---

# 🏗️ Testing Approach

The project follows a basic automated API testing lifecycle:

```text
          ┌────────────────────┐
          │  Generate Dynamic  │
          │       Data         │
          └─────────┬──────────┘
                    ↓
          ┌────────────────────┐
          │ Send API Request   │
          └─────────┬──────────┘
                    ↓
          ┌────────────────────┐
          │ Validate Status    │
          │ Code               │
          └─────────┬──────────┘
                    ↓
          ┌────────────────────┐
          │ Validate Response  │
          │ Body               │
          └─────────┬──────────┘
                    ↓
          ┌────────────────────┐
          │ Validate Response  │
          │ Time & Size        │
          └─────────┬──────────┘
                    ↓
          ┌────────────────────┐
          │ Store Data for     │
          │ Next Request       │
          └─────────┬──────────┘
                    ↓
          ┌────────────────────┐
          │ Execute Through    │
          │ Newman             │
          └─────────┬──────────┘
                    ↓
          ┌────────────────────┐
          │ Generate Test      │
          │ Report             │
          └────────────────────┘
```

---

# 🔍 What This Project Demonstrates

This project demonstrates practical knowledge of:

* ✅ REST API testing
* ✅ HTTP methods: `GET`, `POST`, `PUT`
* ✅ Postman collections
* ✅ Postman environments
* ✅ Environment variables
* ✅ Dynamic variables
* ✅ Pre-request scripts
* ✅ Post-response test scripts
* ✅ JavaScript assertions
* ✅ Request chaining
* ✅ JSON request bodies
* ✅ Response validation
* ✅ Performance assertions
* ✅ Response-size validation
* ✅ Command-line API testing
* ✅ Newman
* ✅ Automated HTML reporting
* ✅ Basic API test-result analysis

---

# 📌 Current Testing Status

### Overall

**91.11% assertion pass rate**

```text
██████████████████░░  41 Passed
████████████████████  45 Total
```

### Request-Level Result

```text
7 Requests Executed
0 Requests Failed
```

### Assertion-Level Result

```text
41 Passed
4 Failed
```

The four failures are documented above and provide useful candidates for further debugging and test refinement.

---

# 🔮 Future Improvements

Possible improvements to this project include:

* Add negative test cases.
* Add authentication/authorization testing if supported by the API.
* Add schema validation using JSON Schema.
* Add boundary-value testing.
* Add invalid input testing.
* Add missing-field validation.
* Add duplicate-record testing.
* Improve technical-skills request validation.
* Investigate the `FinalStudentDetails` response structure.
* Reconsider the 1000-byte threshold for the Get-All-Students endpoint.
* Parameterize test data using external CSV/JSON files.
* Add CI/CD execution using GitHub Actions.
* Publish Newman reports as CI artifacts.
* Add test badges to the repository.
* Increase API coverage with additional endpoints.
* Separate smoke, regression, and negative test suites.

---

# ▶️ Quick Start

Clone the repository:

```bash
git clone <YOUR-GITHUB-REPOSITORY-URL>
```

Move into the project:

```bash
cd Restful_Student_API_Testing
```

Install Newman:

```bash
npm install -g newman
```

Run the collection:

```bash
newman run "API testing.postman_collection.json" -e "Automated.postman_environment.json"
```

Generate the HTML report:

```bash
newman run "API testing.postman_collection.json" -e "Automated.postman_environment.json" -r cli,html
```

---

# 👨‍💻 Author

**Arthur**

QA / Software Testing Project

---

## 📜 Disclaimer

This project is intended for **educational and testing purposes**. The automated tests interact with the publicly available TestingWorld Student API and are designed to demonstrate API testing and automation concepts using Postman and Newman.
