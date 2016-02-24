# MongoDB-users-endpoints



**Requirement:**

MongoDB 3.0

**Techoligies:**

mongo-java-driver: Java library for MongoDB.

jersey-container-jdk-http: framework provides standard ways for building RESTful web services

httpserver allows to build http server in runnable jar file


**Installetion:**

Extract all files from zip file

Launch Eclipse

File->Import->Maven->Existing Maven Projects and select directory where files have been extracted

src->org.mongousers->UserServer ->Run As-> Java Application
press Enter to stop server

Build Runnible Jar:

File->Export->Java->Runnable JAR file: 
Select Launch configuration: UserServer
Enter Export destination: MongoUsers.jar


**Start in command line:**

java -cp MongoUsers.jar org.mongousers.UserServer [parameter:value] [parameter:value]...

List of parameters and default values:

		host -           host URL, default http://localhost/.
		port - 	         port, default 9998.
		dbhost -         host of MongoDB, default http://localhost.
		dbport -         port of MongoDB, default 27017.
		dbuser -         MongoDB user name.
		dbpassword -     MongoDB user password.
		dbname -         MongoDB database name, default "test".
		dbcollection -	 MongoDB collection, default "users".
		
Example: start application locally on port 9997 with MongoDB is running locally on port 27018, 
using db "test" and collection "users":

java -cp MongoUsers.jar org.mongousers.UserServer -port:9997 -dbport:27018 dbname:test dbcollection:users
		

**JUnit tests:**

java -cp MongoUsers.jar tests.testMain [host:<server-host>][port:<server-port>]

import users.json to MongoDB using mongoimport, where db and collection will be used in parameters of server:

mongoimport /db:test /collection:users < users.json

Start MongoUsers:

java -cp MongoUsers.jar org.mongousers.UserServer -dbname:test -dbcollection:users

Run JUnit tests:

java -cp MongoUsers.jar tests.testMain


**Versioning.**

Versioning of endpoints are handled with JAX-RS annotation @Consumes:

@Consumes({MediaType.APPLICATION_JSON,"application/mongo-users-v1+json"})

Client sends a version in http header:
connection.setRequestProperty("Content-Type", "application/mongo-users-v1+json; charset=UTF-8");

Server returns HTTP error 415, if server version and client version are different.

JUnit test testFiles() sends wrong version to server and fails, if server does not return an error.


**EndPoints definition.**

User Login:

POST
UserService/login

Payload example: {"userid":"test","password":"test123!"}

Response: {"error":"yes"|"no"}

Get users:

GET
UserService/users
Query params: 
page_size - max number of results
page_number - current page number
groupby - group field
filter - query condition

Sample: 
UserService/users?page_number=3&page_size=50&groupby=address.zipcode&filter={"fistname":"John","profession":"engineer"}

Response:
{
"last":"yes",
"result":[{_id:94063,...}]
}

Status:

GET
UserService/status

Response: {"error":"yes"|"no"}

Files:

GET
UserService/files{dir}

Response example:
[{"db":"directory"},{"readme.txt":"file"}...]


**Using endpoints locally:**

http://localhost:9998/UserService/users?page_size=3&groupby=profession&page_number=1

http://localhost:9998/UserService/status

http://localhost:9998/UserService/files/c:

