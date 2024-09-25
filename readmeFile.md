D:\etisalad_project_details
Readmi file

1.rem open a shell - zookeeper is at localhost:2181
   bin\windows\zookeeper-server-start.bat config\zookeeper.properties

2.rem open another shell - kafka is at localhost:9092
   bin\windows\kafka-server-start.bat config\server.properties
   
   
if any issue with state store then please delete the kafka-stream folder under %tmp%
   
   
Kafka Producers: Each source system will have a dedicated Kafka producer that sends GPS transaction data to a corresponding Kafka topic (gps-transactions-src01, gps-transactions-src02, gps-transactions-src03, gps-transactions-src04)   

if by any change kafka unable to start then change the log path
log.dirs=/tmp/kafka-logs-2

3.docker-compose up kafka-cluster

4.http://127.0.0.1:3030/
   
5.rem create input topic
bin\windows\kafka-topics.bat --create --bootstrap-server 0.0.0.0:9092 --replication-factor 1 --partitions 1 --topic gps-transactions-src01
bin\windows\kafka-topics.bat --create --bootstrap-server 0.0.0.0:9092 --replication-factor 1 --partitions 1 --topic gps-transactions-src02
bin\windows\kafka-topics.bat --create --bootstrap-server 0.0.0.0:9092 --replication-factor 1 --partitions 1 --topic gps-transactions-src03
bin\windows\kafka-topics.bat --create --bootstrap-server 0.0.0.0:9092 --replication-factor 1 --partitions 1 --topic gps-transactions-src04

bin\windows\kafka-topics.bat --create --bootstrap-server 0.0.0.0:9092 --replication-factor 1 --partitions 4 --topic gps-transactions-output


rem verify the data has been written
bin\windows\kafka-console-consumer.bat --bootstrap-server localhost:9092 --topic gps-transactions-src01 --from-beginning

bin\windows\kafka-console-consumer.bat --bootstrap-server localhost:9092 --topic gps-transactions-src02 --from-beginning

bin\windows\kafka-console-consumer.bat --bootstrap-server localhost:9092 --topic gps-transactions-src03 --from-beginning

bin\windows\kafka-console-consumer.bat --bootstrap-server localhost:9092 --topic gps-transactions-src04 --from-beginning

bin\windows\kafka-console-consumer.bat --bootstrap-server localhost:9092 --topic gps-transactions-output --from-beginning


project name

etisalad-spring-boot-kafka-producer

//postman collection

{
    "CustomerID": "{{CustomerID}}",
    "CarID": "{{CarID}}",
    "OfficeID": "{{OfficeID}}",
    "AgentID": "{{AgentID}}",
    "TRXN Timestamp": "{{TRXNTimestamp}}",
    "CarDrivingStatus": "{{randomDrivingStatus}}",
    "CurrentLongitude": "{{CurrentLongitude}}",
    "CurrentLatitude": "{{CurrentLatitude}}",
    "CurrentArea": "{{randomCity}}",
    "KM": "{{$randomInt}}"
}


// Function to format the current date and time as yyyy-MM-dd hh:mm:ss AM/PM
function formatTimestamp(date) {
    const year = date.getFullYear();
    const month = ("0" + (date.getMonth() + 1)).slice(-2); // Months are zero-based
    const day = ("0" + date.getDate()).slice(-2);

    let hours = date.getHours();
    const minutes = ("0" + date.getMinutes()).slice(-2);
    const seconds = ("0" + date.getSeconds()).slice(-2);

    const ampm = hours >= 12 ? 'PM' : 'AM';
    hours = hours % 12;
    hours = hours ? hours : 12; // the hour '0' should be '12'

    const formattedTime = year + "-" + month + "-" + day + " " + hours + ":" + minutes + ":" + seconds + " " + ampm;
    return formattedTime;
}

// Function to generate a random number between min and max
function getRandomInRange(min, max) {
    return Math.random() * (max - min) + min;
}
// Generate the current date and time
const currentDate = new Date();
const trxntimestamp = formatTimestamp(currentDate);

// Store the formatted timestamp in a Postman variable
pm.variables.set("TRXNTimestamp", trxntimestamp);

let randomLatitude = getRandomInRange(-90, 90).toFixed(6);

// Generate random longitude between -180 and 180
let randomLongitude = getRandomInRange(-180, 180).toFixed(6);


// Set environment or global variables
pm.environment.set('CurrentLongitude', randomLongitude);
pm.environment.set('CurrentLatitude', randomLatitude);
const min = 1; // Starting value
const max = 99999; // Maximum value

// Custom random values for CarDrivingStatus
const drivingStatuses = ["Stopped", "Idle", "Moving"];
pm.variables.set("randomDrivingStatus", drivingStatuses[Math.floor(Math.random() * drivingStatuses.length)]);
const customerID = Math.floor(Math.random() * (max - min + 1)) + min;

// Store the random CustomerID in a Postman variable
pm.variables.set("CustomerID", customerID);

// Define the range for CarID


// Generate a random CarID within the range
const CarID = Math.floor(Math.random() * (max - min + 1)) + min;

pm.variables.set("CarID", CarID);

//OfficeID

const OfficeID = Math.floor(Math.random() * (max - min + 1)) + min;

// Store the random CustomerID in a Postman variable
pm.variables.set("OfficeID", OfficeID);

//AgentID


const AgentID = Math.floor(Math.random() * (max - min + 1)) + min;

// Store the random CustomerID in a Postman variable
pm.variables.set("AgentID", AgentID);

// Custom random cities
const cities = ["Dubai", "New York", "Paris", "Tokyo", "Berlin"];
pm.variables.set("randomCity", cities[Math.floor(Math.random() * cities.length)]);

postman
localhost:8080/custom-message

sourceSystem

gps-transactions-src01





-------------------------------------------------
{
    "CustomerID": "{{CustomerID}}",
    "CarID": "{{CarID}}",
    "OfficeID": "{{OfficeID}}",
    "AgentID": "{{AgentID}}",
    "TRXN Timestamp": "{{TRXNTimestamp}}",
    "CarDrivingStatus": "{{randomDrivingStatus}}",
    "CurrentLongitude": "{{CurrentLongitude}}",
    "CurrentLatitude": "{{CurrentLatitude}}",
    "CurrentArea": "{{randomCity}}",
    "KM": "{{$randomInt}}"
}

// Function to format the current date and time as yyyy-MM-dd hh:mm:ss AM/PM
function formatTimestamp(date) {
    const year = date.getFullYear();
    const month = ("0" + (date.getMonth() + 1)).slice(-2); // Months are zero-based
    const day = ("0" + date.getDate()).slice(-2);

    let hours = date.getHours();
    const minutes = ("0" + date.getMinutes()).slice(-2);
    const seconds = ("0" + date.getSeconds()).slice(-2);

    const ampm = hours >= 12 ? 'PM' : 'AM';
    hours = hours % 12;
    hours = hours ? hours : 12; // the hour '0' should be '12'

    const formattedTime = year + "-" + month + "-" + day + " " + hours + ":" + minutes + ":" + seconds + " " + ampm;
    return formattedTime;
}

// Function to generate a random number between min and max
function getRandomInRange(min, max) {
    return Math.random() * (max - min) + min;
}
// Generate the current date and time
const currentDate = new Date();
const trxntimestamp = formatTimestamp(currentDate);

// Store the formatted timestamp in a Postman variable
pm.variables.set("TRXNTimestamp", trxntimestamp);

let randomLatitude = getRandomInRange(-90, 90).toFixed(6);

// Generate random longitude between -180 and 180
let randomLongitude = getRandomInRange(-180, 180).toFixed(6);


// Set environment or global variables
pm.environment.set('CurrentLongitude', randomLongitude);
pm.environment.set('CurrentLatitude', randomLatitude);


// Custom random values for CarDrivingStatus
const drivingStatuses = ["Stopped", "Idle", "Moving"];
pm.variables.set("randomDrivingStatus", drivingStatuses[Math.floor(Math.random() * drivingStatuses.length)]);

const min = 1; // Starting value
const max = 99999; // Maximum value

const customerID = Math.floor(Math.random() * (max - min + 1)) + min;

// Store the random CustomerID in a Postman variable
pm.variables.set("CustomerID", customerID);


// Generate a random CarID within the range
const CarID = Math.floor(Math.random() * (max - min + 1)) + min;
// Store the random CustomerID in a Postman variable
pm.variables.set("CarID", CarID);

//OfficeID

const OfficeID = Math.floor(Math.random() * (max - min + 1)) + min;

// Store the random CustomerID in a Postman variable
pm.variables.set("OfficeID", OfficeID);

//AgentID


const AgentID = Math.floor(Math.random() * (max - min + 1)) + min;

// Store the random CustomerID in a Postman variable
pm.variables.set("AgentID", AgentID);

// Custom random cities
const cities = ["Dubai", "New York", "Paris", "Tokyo", "Berlin"];
pm.variables.set("randomCity", cities[Math.floor(Math.random() * cities.length)]);


---------------------------------

{
    "CustomerID": "{{CustomerID}}",
    "CarID": "{{CarID}}",
    "OfficeID": "{{OfficeID}}",
    "AgentID": "{{AgentID}}",
    "TRXN Timestamp": "{{TRXNTimestamp}}",
    "CarDrivingStatus": "{{randomDrivingStatus}}",
    "CurrentLongitude": "{{CurrentLongitude}}",
    "CurrentLatitude": "{{CurrentLatitude}}",
    "CurrentArea": "{{randomCity}}",
    "KM": "{{$randomInt}}"
}


// Function to format the current date and time as yyyy-MM-dd hh:mm:ss AM/PM
function formatTimestamp(date) {
    const year = date.getFullYear();
    const month = ("0" + (date.getMonth() + 1)).slice(-2); // Months are zero-based
    const day = ("0" + date.getDate()).slice(-2);

    let hours = date.getHours();
    const minutes = ("0" + date.getMinutes()).slice(-2);
    const seconds = ("0" + date.getSeconds()).slice(-2);

    const ampm = hours >= 12 ? 'PM' : 'AM';
    hours = hours % 12;
    hours = hours ? hours : 12; // the hour '0' should be '12'

    const formattedTime = year + "-" + month + "-" + day + " " + hours + ":" + minutes + ":" + seconds + " " + ampm;
    return formattedTime;
}

// Function to generate a random number between min and max
function getRandomInRange(min, max) {
    return Math.random() * (max - min) + min;
}
// Generate the current date and time
const currentDate = new Date();
const trxntimestamp = formatTimestamp(currentDate);

// Store the formatted timestamp in a Postman variable
pm.variables.set("TRXNTimestamp", trxntimestamp);

let randomLatitude = getRandomInRange(-90, 90).toFixed(6);

// Generate random longitude between -180 and 180
let randomLongitude = getRandomInRange(-180, 180).toFixed(6);


// Set environment or global variables
pm.environment.set('CurrentLongitude', randomLongitude);
pm.environment.set('CurrentLatitude', randomLatitude);

const min = 1; // Starting value
const max = 99999; // Maximum value

// Custom random values for CarDrivingStatus
const drivingStatuses = ["Stopped", "Idle", "Moving"];
pm.variables.set("randomDrivingStatus", drivingStatuses[Math.floor(Math.random() * drivingStatuses.length)]);
const customerID = Math.floor(Math.random() * (max - min + 1)) + min;

// Store the random CustomerID in a Postman variable
pm.variables.set("CustomerID", customerID);


// Generate a random CarID within the range
const CarID = Math.floor(Math.random() * (max - min + 1)) + min;
// Store the random CustomerID in a Postman variable
pm.variables.set("CarID", CarID);

//OfficeID

const OfficeID = Math.floor(Math.random() * (max - min + 1)) + min;

// Store the random CustomerID in a Postman variable
pm.variables.set("OfficeID", OfficeID);

//AgentID


const AgentID = Math.floor(Math.random() * (max - min + 1)) + min;

// Store the random CustomerID in a Postman variable
pm.variables.set("AgentID", AgentID);

// Custom random cities
const cities = ["Dubai", "New York", "Paris", "Tokyo", "Berlin"];
pm.variables.set("randomCity", cities[Math.floor(Math.random() * cities.length)]);

----------------------------------------

{
    "CustomerID": "{{CustomerID}}",
    "CarID": "{{CarID}}",
    "OfficeID": "{{OfficeID}}",
    "AgentID": "{{AgentID}}",
    "TRXN Timestamp": "{{TRXNTimestamp}}",
    "CarDrivingStatus": "{{randomDrivingStatus}}",
    "CurrentLongitude": "{{CurrentLongitude}}",
    "CurrentLatitude": "{{CurrentLatitude}}",
    "CurrentArea": "{{randomCity}}",
    "KM": "{{$randomInt}}"
}


// Function to format the current date and time as yyyy-MM-dd hh:mm:ss AM/PM
function formatTimestamp(date) {
    const year = date.getFullYear();
    const month = ("0" + (date.getMonth() + 1)).slice(-2); // Months are zero-based
    const day = ("0" + date.getDate()).slice(-2);

    let hours = date.getHours();
    const minutes = ("0" + date.getMinutes()).slice(-2);
    const seconds = ("0" + date.getSeconds()).slice(-2);

    const ampm = hours >= 12 ? 'PM' : 'AM';
    hours = hours % 12;
    hours = hours ? hours : 12; // the hour '0' should be '12'

    const formattedTime = year + "-" + month + "-" + day + " " + hours + ":" + minutes + ":" + seconds + " " + ampm;
    return formattedTime;
}

// Function to generate a random number between min and max
function getRandomInRange(min, max) {
    return Math.random() * (max - min) + min;
}
// Generate the current date and time
const currentDate = new Date();
const trxntimestamp = formatTimestamp(currentDate);

// Store the formatted timestamp in a Postman variable
pm.variables.set("TRXNTimestamp", trxntimestamp);

let randomLatitude = getRandomInRange(-90, 90).toFixed(6);

// Generate random longitude between -180 and 180
let randomLongitude = getRandomInRange(-180, 180).toFixed(6);


// Set environment or global variables
pm.environment.set('CurrentLongitude', randomLongitude);
pm.environment.set('CurrentLatitude', randomLatitude);


// Custom random values for CarDrivingStatus
const drivingStatuses = ["Stopped", "Idle", "Moving"];
pm.variables.set("randomDrivingStatus", drivingStatuses[Math.floor(Math.random() * drivingStatuses.length)]);

const min = 1; // Starting value
const max = 99999; // Maximum value


const customerID = Math.floor(Math.random() * (max - min + 1)) + min;

// Store the random CustomerID in a Postman variable
pm.variables.set("CustomerID", customerID);

//CarID
const CarID = Math.floor(Math.random() * (max - min + 1)) + min;

// Store the random CustomerID in a Postman variable
pm.variables.set("CarID", CarID);

//OfficeID

const OfficeID = Math.floor(Math.random() * (max - min + 1)) + min;

// Store the random CustomerID in a Postman variable
pm.variables.set("OfficeID", OfficeID);

//AgentID


const AgentID = Math.floor(Math.random() * (max - min + 1)) + min;

// Store the random CustomerID in a Postman variable
pm.variables.set("AgentID", AgentID);

// Custom random cities
const cities = ["Dubai", "New York", "Paris", "Tokyo", "Berlin"];
pm.variables.set("randomCity", cities[Math.floor(Math.random() * cities.length)]);



---------------------------------------------------------------------


	










api calls

localhost:9090/api/sales-transactions

{
  "agentId": {{AgentID}},
  "officeId": {{OfficeID}},
  "carId": {{CarID}},
  "customerId": {{CustomerID}},
  "amount": {{amount}},  // You can define this variable or set it manually
  "createdAt": "{{currentDateTime}}",
  "updatedAt": "{{currentDateTime}}"
}

// Function to format the current date and time as yyyy-MM-dd hh:mm:ss AM/PM
function formatTimestamp(date) {
    const year = date.getFullYear();
    const month = ("0" + (date.getMonth() + 1)).slice(-2); // Months are zero-based
    const day = ("0" + date.getDate()).slice(-2);

    let hours = date.getHours();
    const minutes = ("0" + date.getMinutes()).slice(-2);
    const seconds = ("0" + date.getSeconds()).slice(-2);

    const ampm = hours >= 12 ? 'PM' : 'AM';
    hours = hours % 12;
    hours = hours ? hours : 12; // the hour '0' should be '12'

    const formattedTime = year + "-" + month + "-" + day + " " + hours + ":" + minutes + ":" + seconds + " " + ampm;
    return formattedTime;
}

// Function to generate a random number between min and max
function getRandomInRange(min, max) {
    return Math.random() * (max - min) + min;
}
// Generate the current date and time
const currentDate = new Date();
const trxntimestamp = formatTimestamp(currentDate);

// Store the formatted timestamp in a Postman variable
pm.variables.set("currentDateTime", trxntimestamp);

const min = 1; // Starting value
const max = 99999; // Maximum value

const customerID = Math.floor(Math.random() * (max - min + 1)) + min;

// Store the random CustomerID in a Postman variable
pm.variables.set("CustomerID", customerID);

//CarID
const CarID = Math.floor(Math.random() * (max - min + 1)) + min;

// Store the random CustomerID in a Postman variable
pm.variables.set("CarID", CarID);

//OfficeID

const OfficeID = Math.floor(Math.random() * (max - min + 1)) + min;

// Store the random CustomerID in a Postman variable
pm.variables.set("OfficeID", OfficeID);

//AgentID
const AgentID = Math.floor(Math.random() * (max - min + 1)) + min;
// Store the random CustomerID in a Postman variable
pm.variables.set("AgentID", AgentID);

// Generate a random decimal between 0 and 10000, rounded to 2 decimal places
let randomAmount = (Math.random() * 10000).toFixed(2);

// Set the random decimal as an environment variable in Postman
pm.environment.set("amount", randomAmount);


----------------------------
localhost:9090/api/car-data

{
    "carMake": "{{randomCarMake}}",
    "carModel": "{{randomCarModel}}",
    "plateNo": "{{randomPlateNo}}",
    "registrationDate": "{{randomDate}}",
    "registrationExpiryDate": "{{randomExpiryDate}}"
}



// Function to format the current date and time as yyyy-MM-dd hh:mm:ss AM/PM
function formatTimestamp(date) {
    const year = date.getFullYear();
    const month = ("0" + (date.getMonth() + 1)).slice(-2); // Months are zero-based
    const day = ("0" + date.getDate()).slice(-2);

    let hours = date.getHours();
    const minutes = ("0" + date.getMinutes()).slice(-2);
    const seconds = ("0" + date.getSeconds()).slice(-2);

    const ampm = hours >= 12 ? 'PM' : 'AM';
    hours = hours % 12;
    hours = hours ? hours : 12; // the hour '0' should be '12'

    const formattedTime = year + "-" + month + "-" + day;
    return formattedTime;
}

// Function to generate a random number between min and max
function getRandomInRange(min, max) {
    return Math.random() * (max - min) + min;
}
// Generate the current date and time
const currentDate = new Date();

const nextYearDate = new Date(currentDate);
nextYearDate.setFullYear(currentDate.getFullYear() + 1);
const randomExpiryDate = formatTimestamp(nextYearDate);

// Store the formatted timestamp in a Postman variable
pm.variables.set("randomExpiryDate", randomExpiryDate);
const registrationDate = formatTimestamp(currentDate);

// Store the formatted timestamp in a Postman variable
pm.variables.set("randomDate", registrationDate);

const carModels = [
    "Sedan",
    "SUV",
    "Coupe",
    "Convertible",
    "Hatchback",
    "Minivan",
    "Pickup Truck",
    "Wagon"
];

// Generate a random index to select a car model
const randomCarIndex = Math.floor(Math.random() * carModels.length);
const randomCarModel = carModels[randomCarIndex];

// Set the random car model as an environment variable
pm.environment.set("randomCarModel", randomCarModel);

const carMakes = [
    "Toyota",
    "Ford",
    "Honda",
    "Chevrolet",
    "BMW",
    "Audi",
    "Mercedes",
    "Nissan"
];


const randomcarMakesIndex = Math.floor(Math.random() * carMakes.length);
const randomCarMake = carMakes[randomcarMakesIndex];
// Set the random car make as an environment variable
pm.environment.set("randomCarMake", randomCarMake);

// Function to generate a random alphanumeric string of a specified length
function generateRandomPlateNumber(length) {
    const characters = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789';
    let plateNumber = '';
    for (let i = 0; i < length; i++) {
        const randomIndex = Math.floor(Math.random() * characters.length);
        plateNumber += characters[randomIndex];
    }
    return plateNumber;
}

// Generate a random plate number with a length of 7 characters (e.g., "ABC1234")
const randomPlateNo = generateRandomPlateNumber(7);

// Set the random plate number as an environment variable
pm.environment.set("randomPlateNo", randomPlateNo);








----------------------------------------------
select * from sales_transactions;

localhost:9090/api/car-data


CREATE TABLE cardata (
    car_id INT AUTO_INCREMENT PRIMARY KEY,
    car_make VARCHAR(100) NOT NULL,
    car_model VARCHAR(100) NOT NULL,
    plate_no VARCHAR(20) NOT NULL,
    registration_date VARCHAR(20) NOT NULL,
    registration_expiry_date VARCHAR(20) NOT NULL
);




localhost:9090/api/customers


{
  "customerId": "{{customerId}}",
  "mobileNo": "{{mobileNo}}",
  "name": "{{name}}",
  "gender": "{{gender}}",
  "age": {{age}},
  "nationality": "{{nationality}}",
  "passportNo": "{{passportNo}}",
  "idNo": "{{idNo}}",
  "homeAddress": "{{homeAddress}}",
  "leaseStartDate": "{{leaseStartDate}}",
  "leasePeriod": {{leasePeriod}}
}




// Function to generate random phone numbers
function randomPhoneNumber() {
    return Math.floor(Math.random() * (9999999999 - 1000000000 + 1)) + 1000000000;
}

// Function to generate random names
function randomName() {
    const names = ["John Doe", "Jane Smith", "Alice Johnson", "Bob Brown"];
    return names[Math.floor(Math.random() * names.length)];
}

// Function to generate a random gender
function randomGender() {
    const genders = ["M", "F", "O"];
    return genders[Math.floor(Math.random() * genders.length)];
}

// Function to generate a random nationality
function randomNationality() {
    const nationalities = ["American","UAE", "British", "Canadian", "Australian"];
    return nationalities[Math.floor(Math.random() * nationalities.length)];
}

// Function to generate a random passport number
function randomPassportNo() {
    return 'P' + Math.floor(Math.random() * 10000000).toString().padStart(7, '0');
}

// Function to generate a random ID number
function randomIdNo() {
    return 'ID' + Math.floor(Math.random() * 1000000000).toString().padStart(9, '0');
}

// Function to generate a random home address
function randomHomeAddress() {
    return Math.floor(Math.random() * 1000) + " Main Street, Some City, Some Country";
}

// Function to generate a random lease start date
function randomLeaseStartDate() {
    const startDate = new Date();
    startDate.setFullYear(startDate.getFullYear() - Math.floor(Math.random() * 5)); // Up to 5 years ago
    return startDate.toISOString().split('T')[0]; // YYYY-MM-DD format
}

// Function to generate a random lease period
function randomLeasePeriod() {
    return Math.floor(Math.random() * 60) + 1; // Lease period between 1 and 60 months
}

// Set the environment variables with dynamic values
pm.environment.set("customerId", Math.floor(Math.random() * 1000)); // Random customer ID
pm.environment.set("mobileNo", randomPhoneNumber());
pm.environment.set("name", randomName());
pm.environment.set("gender", randomGender());
pm.environment.set("age", Math.floor(Math.random() * 100)); // Random age between 0 and 99
pm.environment.set("nationality", randomNationality());
pm.environment.set("passportNo", randomPassportNo());
pm.environment.set("idNo", randomIdNo());
pm.environment.set("homeAddress", randomHomeAddress());
pm.environment.set("leaseStartDate", randomLeaseStartDate());
pm.environment.set("leasePeriod", randomLeasePeriod());
------------------------------------------------------------------------
localhost:9090/api/customers
method type post
{
  "customerId": "{{customerId}}",
  "mobileNo": "{{mobileNo}}",
  "name": "{{name}}",
  "gender": "{{gender}}",
  "age": {{age}},
  "nationality": "{{nationality}}",
  "passportNo": "{{passportNo}}",
  "idNo": "{{idNo}}",
  "homeAddress": "{{homeAddress}}",
  "leaseStartDate": "{{leaseStartDate}}",
  "leasePeriod": {{leasePeriod}}
}


// Function to generate random phone numbers
function randomPhoneNumber() {
    return Math.floor(Math.random() * (9999999999 - 1000000000 + 1)) + 1000000000;
}

// Function to generate random names
function randomName() {
    const names = ["John Doe", "Jane Smith", "Alice Johnson", "Bob Brown"];
    return names[Math.floor(Math.random() * names.length)];
}

// Function to generate a random gender
function randomGender() {
    const genders = ["M", "F", "O"];
    return genders[Math.floor(Math.random() * genders.length)];
}

// Function to generate a random nationality
function randomNationality() {
    const nationalities = ["American","UAE", "British", "Canadian", "Australian"];
    return nationalities[Math.floor(Math.random() * nationalities.length)];
}

// Function to generate a random passport number
function randomPassportNo() {
    return 'P' + Math.floor(Math.random() * 10000000).toString().padStart(7, '0');
}

// Function to generate a random ID number
function randomIdNo() {
    return 'ID' + Math.floor(Math.random() * 1000000000).toString().padStart(9, '0');
}

// Function to generate a random home address
function randomHomeAddress() {
    return Math.floor(Math.random() * 1000) + " Main Street, Some City, Some Country";
}

// Function to generate a random lease start date
function randomLeaseStartDate() {
    const startDate = new Date();
    startDate.setFullYear(startDate.getFullYear() - Math.floor(Math.random() * 5)); // Up to 5 years ago
    return startDate.toISOString().split('T')[0]; // YYYY-MM-DD format
}

// Function to generate a random lease period
function randomLeasePeriod() {
    return Math.floor(Math.random() * 60) + 1; // Lease period between 1 and 60 months
}

// Set the environment variables with dynamic values
pm.environment.set("customerId", Math.floor(Math.random() * 1000)); // Random customer ID
pm.environment.set("mobileNo", randomPhoneNumber());
pm.environment.set("name", randomName());
pm.environment.set("gender", randomGender());
pm.environment.set("age", Math.floor(Math.random() * 100)); // Random age between 0 and 99
pm.environment.set("nationality", randomNationality());
pm.environment.set("passportNo", randomPassportNo());
pm.environment.set("idNo", randomIdNo());
pm.environment.set("homeAddress", randomHomeAddress());
pm.environment.set("leaseStartDate", randomLeaseStartDate());
pm.environment.set("leasePeriod", randomLeasePeriod());





--------------------------------------------------------------------
localhost:9090/api/customers

method type get


-------------------------------------------------------------------

localhost:9090/api/office-branches

{
  "officeId": {{OfficeID}},
  "mobileNo": "{{randomPhoneNumber}}",
  "area": "{{randomArea}}",
  "officeNo": "{{randomOfficeNo}}",
  "workingHours": "{{randomWorkingHours}}"
}

// Generate a random phone number

// Function to generate random phone numbers
function randomPhoneNumber() {
    return Math.floor(Math.random() * (9999999999 - 1000000000 + 1)) + 1000000000;
}

pm.environment.set("randomPhoneNumber",randomPhoneNumber());


// Generate a random area name
const areas = ["Dubai", "Abu Dhabi", "RAK", "Union"];
pm.environment.set("randomArea", areas[Math.floor(Math.random() * areas.length)]);

// Generate a random office number
pm.environment.set("randomOfficeNo", `${Math.floor(Math.random() * 1000)}`);


// Function to format time with leading zeros
function formatTime(hour, minute) {
    return `${String(hour).padStart(2, '0')}:${String(minute).padStart(2, '0')}`;
}

// Define working hours range
const startHour = 8;  // 8 AM
const endHour = 20;   // 8 PM

// Generate random start and end times within the defined range
function getRandomTimeWithinRange(startHour, endHour) {
    const hour = Math.floor(Math.random() * (endHour - startHour + 1)) + startHour;
    const minute = Math.floor(Math.random() * 60);
    return formatTime(hour, minute);
}

// Generate random working hours
const startTime = getRandomTimeWithinRange(startHour, endHour);
const endTime = getRandomTimeWithinRange(startHour, endHour);

// Ensure the end time is after the start time
const [startHourInt, startMinuteInt] = startTime.split(':').map(Number);
const [endHourInt, endMinuteInt] = endTime.split(':').map(Number);

let adjustedEndTime = endTime;

if (endHourInt < startHourInt || (endHourInt === startHourInt && endMinuteInt <= startMinuteInt)) {
    // Ensure end time is always after start time
    const newEndHour = Math.min(endHour, startHourInt + 1);
    adjustedEndTime = formatTime(newEndHour, Math.floor(Math.random() * 60));
}

pm.environment.set("randomWorkingHours", `${startTime} - ${adjustedEndTime}`);



const min = 1; // Starting value
const max = 99999; // Maximum value


const OfficeID = Math.floor(Math.random() * (max - min + 1)) + min;

// Store the random CustomerID in a Postman variable
pm.variables.set("OfficeID", OfficeID);



CREATE TABLE office_branch_data (
    OfficeID INT AUTO_INCREMENT PRIMARY KEY,     -- Unique ID for the office
    mobile_no VARCHAR(20) NOT NULL,               -- Mobile number with a length of up to 20 characters
    Area VARCHAR(100) NOT NULL,                  -- Area where the office is located
    office_no VARCHAR(20) NOT NULL,               -- Office number, assuming it's alphanumeric
    working_hours VARCHAR(50) NOT NULL            -- Working hours as a string (e.g., "9 AM - 5 PM")
);


-------------------------------------------------

localhost:9090/api/sales-agents

{
  "mobileNo": "{{randomMobileNo}}",
  "name": "{{name}}",
  "gender": "{{gender}}",
  "age": {{age}},
  "nationality": "{{randomNationality}}",
  "officeId": {{OfficeID}}
}



// Function to generate random names
function randomName() {
    const names = ["John Doe", "Jane Smith", "Alice Johnson", "Bob Brown"];
    return names[Math.floor(Math.random() * names.length)];
}
pm.environment.set("name",randomName());



// Generate a random phone number

// Function to generate random phone numbers
function randomPhoneNumber() {
    return Math.floor(Math.random() * (9999999999 - 1000000000 + 1)) + 1000000000;
}

pm.environment.set("randomMobileNo",randomPhoneNumber());



// Generate a random office number
pm.environment.set("randomOfficeNo", `${Math.floor(Math.random() * 1000)}`);


// Function to format time with leading zeros
function formatTime(hour, minute) {
    return `${String(hour).padStart(2, '0')}:${String(minute).padStart(2, '0')}`;
}

// Define working hours range
const startHour = 8;  // 8 AM
const endHour = 20;   // 8 PM

// Generate random start and end times within the defined range
function getRandomTimeWithinRange(startHour, endHour) {
    const hour = Math.floor(Math.random() * (endHour - startHour + 1)) + startHour;
    const minute = Math.floor(Math.random() * 60);
    return formatTime(hour, minute);
}

// Generate random working hours
const startTime = getRandomTimeWithinRange(startHour, endHour);
const endTime = getRandomTimeWithinRange(startHour, endHour);

// Ensure the end time is after the start time
const [startHourInt, startMinuteInt] = startTime.split(':').map(Number);
const [endHourInt, endMinuteInt] = endTime.split(':').map(Number);

let adjustedEndTime = endTime;

if (endHourInt < startHourInt || (endHourInt === startHourInt && endMinuteInt <= startMinuteInt)) {
    // Ensure end time is always after start time
    const newEndHour = Math.min(endHour, startHourInt + 1);
    adjustedEndTime = formatTime(newEndHour, Math.floor(Math.random() * 60));
}

pm.environment.set("randomWorkingHours", `${startTime} - ${adjustedEndTime}`);

// Function to generate a random nationality
function randomNationality() {
    const nationalities = ["American","UAE", "British", "Canadian", "Australian"];
    return nationalities[Math.floor(Math.random() * nationalities.length)];
}

pm.environment.set("randomNationality", randomNationality());


const min = 1; // Starting value
const max = 99999; // Maximum value


const OfficeID = Math.floor(Math.random() * (max - min + 1)) + min;

// Store the random CustomerID in a Postman variable
pm.variables.set("OfficeID", OfficeID);
pm.environment.set("age", Math.floor(Math.random() * 100)); // Random age between 0 and 99


function randomGender() {
    const genders = ["M", "F", "O"];
    return genders[Math.floor(Math.random() * genders.length)];
}

pm.variables.set("gender", randomGender());

CREATE TABLE sales_agent_data (
    AgentID INT AUTO_INCREMENT PRIMARY KEY,
    mobile_no VARCHAR(20) NOT NULL,
    Name VARCHAR(100) NOT NULL,
    Gender CHAR(1) NOT NULL, -- Assuming 'M', 'F', or 'O'
    Age INT NOT NULL,
    Nationality VARCHAR(50) NOT NULL,
    OfficeID INT NOT NULL
);



--------------------------------------
install the flink cluster


cd C:\flink


bin\start-cluster.sh



Open a web browser and go to http://localhost:8081. This will bring up the Flink Web Dashboard where you can monitor and manage Flink jobs.



cd C:\flink


bin\stop-cluster.sh


-----------------------------------------------------------------------------

kafka-stream project 

CREATE TABLE GPSRealTimeTransactions (
    txnID INT AUTO_INCREMENT PRIMARY KEY,
    customerID VARCHAR(255),
    carID VARCHAR(255),
    officeID VARCHAR(255),
    agentID VARCHAR(255),
    trxnTimestamp VARCHAR(255),
    carDrivingStatus VARCHAR(255),
    currentLongitude VARCHAR(255),
    currentLatitude VARCHAR(255),
    currentArea VARCHAR(255),
    km VARCHAR(255)
);


-------------------------------------------------

conda install pyspark
pip install kafka-python mysql-connector-python

from anconda prompt

pyspark --packages org.apache.spark:spark-sql-kafka-0-10_2.12:3.5.2




--------------------------------

SELECT
    ROW_NUMBER() OVER () AS BatchID,
    g.trxnTimestamp,
    g.carDrivingStatus,
    g.CurrentLongitude,
    g.CurrentLatitude,
    g.CurrentArea,
    g.KM,
    st.Amount,
    c.mobile_no,
    c.Name AS CustomerName,
    c.Gender AS CustomerGender,
    c.Age AS CustomerAge,
    c.Nationality AS CustomerNationality,
    c.passport_no,
    c.IDNo,
    c.home_address,
    c.lease_start_date,
    c.lease_period,
    ca.car_make,
    ca.car_model,
    ca.plate_no,
    ca.registration_date,
    ca.registration_expiry_date,
    o.mobile_no,
    o.Area AS OfficeArea,
    o.office_no,
    o.working_hours,
    a.Name AS AgentName,
    a.Gender AS AgentGender,
    a.Age,
    a.Nationality AS AgentNationality
FROM
    gpsrealtimetransactions g
LEFT JOIN
    customer_data c ON g.CustomerID = c.CustomerID
LEFT JOIN
    cardata ca ON g.CarID = ca.car_id
LEFT JOIN
    office_branch_data o ON g.OfficeID = o.OfficeID -- Assuming a foreign key relation exists
LEFT JOIN
    sales_agent_data a ON g.agentID = a.AgentID -- Assuming a foreign key relation exists
LEFT JOIN sales_transactions st ON g.agentID=st.AgentID;

-----
INSTALL tableau  

https://www.tableau.com/support/drivers?_ga=2.56418539.1702372025.1676324272-1639065942.1673995356

in mysql workbench
server->user privilages

tableau

localhost
Shubham#1990



SELECT SUM(TOTAL_sales) from etisalad.sales_aggregate group by period_type;








