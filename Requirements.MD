
# What have to be Done
Project for working with information about Vines.

#### Business functions:
Client have to Authorize on App.
If valid -> Get Data about vines on Page.
Client have to be able to Add Vine(s), Edit Vine Data, Delete Vines.
Client can see in separate table Vines that he want to try in future.


## Backend
__Programming language is *any*__
Have to be a docker-compose containing __API Service__, __Worker__(for backend operations), __Broker__(message broker), __PostgreSQL__(database)</br>
Message broker can be chosen from one of theese -> __RabbitMQ__, __Kafka__, __NATS__.</br>

#### Backend logic:
**API Service:**
Realise API for client side and do business logic
- REST or gRPC API
- Methods for realisation - see **API Description** segment
- Have to be an Authorization(Basic Auth)
- If Get Data Methods -> Send Request to Database and get data
- If Create, Update or Delete -> Return information that Request was accepted and send information in "Message Broker"

-------------
- If REST API - having "Swagger" is a plus
- OAuth2.0 as Authorization is a plus

**Worker:**
- Listen for broker queue and read message if there are any
- Do Operations of Create in Database or Delete. If Delete - Before Delete there have to be a pause for 30 seconds. If Update - pause for 20 Seconds. If Create - 10.
- Worker have 2 threads in which it can do separate actions like Create, Update and Delete

All important stemps have to be logged.


## API Description
Description of API
- HealthCheck. Returns "Status": "UP"
- Information. Shows information about service("This is Vine Backend Service")
- Login. For Authorization. (log/pass have to be in env variables)
- Add/Create vine(s). Have to support single addition or multiple of vines.
- Get Vine. Get Particular Vine depends on some condition or return not found.
- Get Vines. Get All Vines or Vines that have a common attribute(s).
- Update Vine. Update information of a particular Vine.
- Delete Vines. Delete vines that will be in an incoming list.
- Add Grape Sort.
- Get All GrapeSorts.
- Get Grape Sort. (By Name)
- Update Grape Sort. (By Name)
- Delete Grape Sort.


## Database
**Database Vines:**
Table **Already_have_vine**:
- ID (Int)
- Name (String)
- Vine_Type (Enum(White, Rose, Red, Other))
- Year (String or DateTime)
- Country (String)
- Grape_Sort (Grape_sort_name relation)
- Tasted (Bool)
- Favourite (Bool)
- Amount (Int)
- How_Good (Enum(Very well, Good, Average, Not Good Enought, Terrible))

Table **Want_to_taste**:
- ID (Int)
- Name (String)
- Year (String or DateTime)
- Country (String)
- Grape_Sort (Grape_sort_name relation)
- Additional info (Text Area)

Table **Grape_sort**:
- ID (Int)
- Name (String | Unique)
- Countries (List[String])
- Regions (List[String])

## Client side
Mobile application 

At first - Authorize client.

If invalid Auth -> Show message "Wrong Credentials". Else -> Show Main Page. (Send request to backend to get Data)

Main Window is just a table with all the vines that client tasted or have.
Vines have to have attributes:
- Name
- Vine Type(White, Rose, Red, Other)
- Year
- Country
- Grape Sort (Drop down list with grape sorts)
- Tasted (Yes/No)
- Favourite(star)
- Amount that client have(How many bottles do client have)
- "How it was" (Do client like it?) (Very well, Good, Average, Not Good Enought, Terrible)

Additional window Containing Window with list of vines to test in future:
- Name
- Year
- Country
- Grape Sort (Drop down list with grape sorts)
- Additional info

Switching between "Tasted"(Vines that client have) and "Want to Taste" vines have to be on top of the application.

If client edit table - send request to backend only when he clicks update button, **not on every change**

Button Add - Form for adding Vine. User can Add multiple Vines. When done -> send a single request on backend

On each vine have to be an icon for editing Vine. Edit information about Vine and send info on backend

Button For Deleting Vines - After it was clicked client may choose some bottles and press "Confirm Delete" After That the request will go to backend

Refresh button -> to get data againg from backend as we just entered app
