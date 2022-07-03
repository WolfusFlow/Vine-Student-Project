
# What have to be Done
Project for working with information about Wines.

#### Business functions:
Client can Authorize on App.
If valid -> Get Data about wines on Page.
Client have to be able to Add Wine(s), Edit Wine Data, Delete Wines.
Client can add Wines to wishlist table for future taste.


## Backend
__Programming language is *any*__ </br>
Have to be a docker-compose containing __API Service__, __Worker__(for backend operations), __Broker__(message broker), __PostgreSQL__(database)</br>
Message broker can be chosen from one of theese -> __RabbitMQ__, __Kafka__, __NATS__.</br>

#### Backend logic:
**API Service:**
Implement API for client side and do business logic
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
- Information. Shows information about service("This is Wine Backend Service")
- Login. For Authorization. (log/pass have to be in environment variables)
- Add/Create wine(s). Have to support single addition or multiple of wines.
- Get Wine. Get Particular Wine depends on some condition or return not found.
- Get Wines. Get All Wines or Wines that have a common attribute(s).
- Update Wine. Update information of a particular Wine.
- Delete Wines. Delete wines that will be in an incoming list.
- Add Grape Sort.
- Get All GrapeSorts.
- Get Grape Sort. (By Name)
- Update Grape Sort. (By Name)
- Delete Grape Sort.


## Database
**Database Wines:**</br>
Table **Already_have_wine**:
- ID (Int)
- Name (String)
- Wine_Type (Enum(White, Rose, Red, Other))
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

Main Window is just a table with all the wines that client tasted or have.
Wines have to have attributes:
- Name
- Wine Type(White, Rose, Red, Other)
- Year
- Country
- Grape Sort (Drop down list with grape sorts)
- Tasted (Yes/No)
- Favourite(star)
- Amount that client have(How many bottles do client have)
- "How it was" (Do client like it?) (Very well, Good, Average, Not Good Enought, Terrible)

Additional window Containing Window with list of wines to test in future:
- Name
- Year
- Country
- Grape Sort (Drop down list with grape sorts)
- Additional info

Switching between "Tasted"(Wines that client have) and "Want to Taste" wines have to be on top of the application.

If client edit table - send request to backend only when he clicks update button, **not on every change**

Button Add - Form for adding Wine. User can Add multiple Wines. When done -> send a single request on backend

On each wine have to be an icon for editing Wine. Edit information about Wine and send info on backend

Button For Deleting Wines - After it was clicked client may choose some bottles and press "Confirm Delete" After That the request will go to backend

Refresh button -> to get data againg from backend as we just entered app
