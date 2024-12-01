# Go Rest API Testing
This project provides a professional overview of the Go REST API, describing its purpose, architecture, and endpoints. The Go REST API is a web service designed to handle CRUD (Create, Read, Update, Delete) operations efficiently.

_**Key Features**_
- Scalability: Designed to handle high traffic efficiently.
- Performance: Makes use of Go's concurrency model to handle requests more quickly.
- Security: Implements authentication and authorization mechanisms.
- implicity: Follows REST principles, ensuring easy integration.
- Tests for POST, GET, PUT, PATCH, and DELETE requests
- Collection of tests
- Environment set up for easy switching between environments
- Pre-request script for data setup
- Test scripts for all assertions and validations

### **Technology used:**
- Postman
- Newman

### **Prerequisite:**
- Node Js
- Newman
- Newman Html Report Library

### **Installation**

1. Postman: If you haven't already, [download and install Postman.](https://www.postman.com/downloads/)
2. Clone the repository:
 ```console 
  git clone https://github.com/mdnazrulislam7255/GoRestAPI-Testing.git
```
3. Import the Postman collection:
    - Open Postman.
    - Click on the Import button.
    - Select the file from the repository.
4. Import the Postman environment:
    - In Postman, click on the gear icon in the top right corner.
    - Select **Import** and choose the file.
5. Newman and Report Installation Process:
    - Newman Install Command:
     ```console 
      npm install -g newman
    ```
    - Newman Html Report Install Command:
     ```console 
      npm install -g newman-reporter-htmlextra
    ```
### **Usage**
1. Select Environment:
    -   In Postman, select the appropriate environment (e.g., Development, Production) from the top-right dropdown.
3. Run Collection:
    -   Select the imported collection from the Collections sidebar.
    -   Click on the Runner button to open the collection runner.
    -   Select the desired environment.
    -   Click Start Test to run the collection.
8. View Results:
    -   Once the tests are complete, view the results in the Runner tab.
    -   Detailed test results can be viewed for each request.

## Base URL
The base URL of Go Rest API is: https://gorest.co.in
## API Endpoints
_**Authentication**_
**Token : Bearer token**                                                                                                                                                                 
**Token value: _fa169e1336c27a986a48660afc597ede875dbe07171318b8532f68e64f12baf5_**
### Screenshot: ![image](https://github.com/user-attachments/assets/a0d3aee3-b14f-4de5-93f2-f07ec4d282fc)


## **Testing**

## Test Case Scenarios:

## _**Create New User**_
### Request URL: https://gorest.co.in/public/v2/users
### Request Method: POST
### Authorization : Inherit from parent
### Pre-request Script:
``` console

var name= pm.variables.replaceIn("{{$randomFullName}}");
pm.environment.set("name_env",name);

var email= pm.variables.replaceIn("{{$randomEmail}}");
pm.environment.set("email_env", email);

let genders = ["male", "female"];
let statuses = ["active", "inactive"];
let randomGender = genders[Math.floor(Math.random() * genders.length)];
let randomStatus = statuses[Math.floor(Math.random() * statuses.length)];
pm.environment.set("gender_env", randomGender);
pm.environment.set("status_env", randomStatus);

console.log(`Generated Name: ${name}`);
console.log(`Generated Email: ${email}`);
console.log(`Generated Gender: ${randomGender}`);
console.log(`Generated Status: ${randomStatus}`);
```
### Post-response script:
``` console
var json_data= pm.response.json();
pm.environment.set("id",json_data.id);
var userID = pm.environment.get("id");
if (userID === json_data.id) {
    pm.test("A new user is created successfully", function () {
        pm.response.to.have.status(201);
    });
}
else{
     pm.test(" Request is failed", function () {
        pm.response.to.have.status(404);
    });
}
```
### Request body:
``` console
{
    "name": "{{name_env}}",
    "gender": "{{gender_env}}",
    "email": "{{email_env}}",
    "status": "{{status_env}}"
}
```
### Response body:
``` console
{
    "id": 7555501,
    "name": "Anthony Kreiger",
    "email": "Troy_Mayert67@gmail.com",
    "gender": "male",
    "status": "active"
}
```
## _**Get New User by ID**_
### Request URL: https://gorest.co.in/public/v2/users/{{id}}
### Request Method: GET
### Authorization : Inherit from parent
### Post-response Script:
``` console
var statusCode = pm.response.code;
console.log(statusCode);
switch (statusCode) {
    case 200:
        var json_data = pm.response.json();

        pm.test("Check Status code 200 OK", function () {
            pm.response.to.have.status(200);
        });

        pm.test("Check Name", function () {
            pm.expect(json_data.name).to.equal(pm.environment.get("name_env"));
        });
        pm.test("Check Email", function () {
            pm.expect(json_data.email).to.equal(pm.environment.get("email_env"));
        });
        pm.test("Check Gender", function () {
            pm.expect(json_data.gender).to.equal(pm.environment.get("gender_env"));
        });

        pm.test("Check Status", function () {
            pm.expect(json_data.status).to.equal(pm.environment.get("status_env"));
        });
        break;

    case 404:
        pm.test("Something went wrong/Not found");
        break;

    case 500:
        pm.test("Something went wrong");
        break;

    default:

}

pm.test("Response time is less than 800ms", function () {
    pm.expect(pm.response.responseTime).to.be.below(800);
});

pm.test("Content-Type is present", function () {
    pm.expect(pm.response.headers.get('Content-Type')).to.eql('application/json; charset=utf-8')
});
//Data types
pm.test("Test the data type of email is a string", () => {
    pm.expect(json_data.email).to.be.a("string");
});
pm.test("Test the data type of status is a string", () => {
    pm.expect(json_data.status).to.be.a("string");
});
// Get the 'name' from the response
let name = pm.response.json().name;
pm.test("Name should be a valid string with no numbers or special characters", function () {
    pm.expect(typeof name).to.eql("string");
    const namePattern = /^[A-Za-z\s]+$/;
    pm.expect(namePattern.test(name)).to.be.true;
    console.log(`Validated Name: ${name}`);
});
// Get the 'status' from the response
let status = pm.response.json().status;
pm.test("Status should be a valid string with no numbers or special characters", function () {
    pm.expect(typeof status).to.eql("string");
    const namePattern = /^[A-Za-z\s]+$/;
    pm.expect(namePattern.test(name)).to.be.true;
    console.log(`Validated Name: ${status}`);
});
let gender = pm.response.json().gender;
pm.test("Gender should be a valid string with no numbers or special characters", function () {
    pm.expect(typeof gender).to.eql("string");
    const namePattern = /^[A-Za-z\s]+$/;
    pm.expect(namePattern.test(name)).to.be.true;
    console.log(`Validated Name: ${gender}`);
});
```
### Request body: none
### Response body: 
``` console
{
    "id": 7555533,
    "name": "Loren Anderson",
    "email": "Jarrell57@yahoo.com",
    "gender": "female",
    "status": "active"
}
```
## _**Update User by ID**_
### Request URL: https://gorest.co.in/public/v2/users/{{id}}
### Request Method: PUT
### Authorization : Inherit from parent
### Pre-request Script:
``` console
// Extracting the values from environment variables or request data
let name = pm.variables.replaceIn("{{name_env}}");
let gender = pm.variables.replaceIn("{{gender_env}}");

// Regex patterns to ensure no special characters or numbers
const namePattern = /^[A-Za-z\s]+$/;
const genderPattern = /^[A-Za-z]+$/;
// Validation logic
let isNameValid = namePattern.test(name);
let isGenderValid = genderPattern.test(gender);

// If any of the fields are invalid, throw an error and stop the request
if (!isNameValid || !isGenderValid) {
    pm.environment.set("updateAllowed", "false");
    pm.error("Validation failed. Name or Gender,contains invalid characters.");
} else {
    pm.environment.set("updateAllowed", "true");
    pm.environment.set("name_env", name);
    pm.environment.set("gender_env", gender);
}
```
### Post-response script:
``` console
pm.test("Check if data update is allowed", function () {
    let updateAllowed = pm.environment.get("updateAllowed");
    pm.expect(updateAllowed).to.eql("true", "Data update not allowed due to validation failure.");
});
```
### Request body:
``` console
{
    "name": " {{name_env}}",
    "email": "{{email_env}}",
    "gender": "{{gender_env}}",
    "status": "active"
}
```
### Response body:
``` console
{
    "email": "Jarrell57@yahoo.com",
    "name": " Loren Anderson",
    "gender": "female",
    "status": "active",
    "id": 7555533
}
```
## _**Update User Partially by ID**_
### Request URL: https://gorest.co.in/public/v2/users/{{id}}
### Request Method: PATCH
### Authorization : Inherit from parent
### Pre-request Script: none
### Request body:
``` console
{
    "name": "bully anny",
    "status": "inactive"
}
```
### Response body:
``` console
{
    "name": "bully anny",
    "status": "inactive",
    "id": 7555533,
    "email": "Jarrell57@yahoo.com",
    "gender": "female"
}
```
## _**Delete User by ID**_
### Request URL: https://gorest.co.in/public/v2/users/{{id}}
### Request Method: DELETE
### Authorization : Inherit from parent
### Pre-request Script:
``` console
let userId = pm.environment.get("id");

if (!userId) {
    console.error("No user ID found.");
    postman.setNextRequest(null);
}
```
### Post-response script:
``` console

pm.test("Status code is 204 (No Content)", function () {
    pm.response.to.have.status(204);
});
pm.test("Unset user_id after deletion", function () {
    pm.environment.unset("id");

    pm.expect(pm.environment.get("id")).to.be.undefined;

    console.log("User ID has been unset after deletion.");
});
```
## Run Command:  
- Run Command for Console: 
```console 
newman run Go_Rest_API.postman_collection.json -e go_rest_env.postman_environment.json 
```
- Run Command for Report: 
```console 
newman run Go_Rest_API.postman_collection.json -e go_rest_env.postman_environment.json -r cli,htmlextra 
```
- Run Command for Report with three iteration:
``` console
newman run Go_Rest_API.postman_collection.json -e go_rest_env.postman_environment.json -r cli,htmlextra -n 3
```
## Newman Report
![image](https://github.com/user-attachments/assets/973d8251-4ee2-4d18-b3b0-72662f993f39)
![image](https://github.com/user-attachments/assets/38c57a64-f8e4-48dd-8bc6-5175960ef79a)


## Contact
For questions or support, contact the development team at:
Email: **nazrul15-7255@diu.edu.bd**
