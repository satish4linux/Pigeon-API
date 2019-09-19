## Introduction

- Server endpoint of VPN Manager System
- Provides secured REST-based Web APIs to perform CRUD operations
- It's APIs are consumed by VPN Manager Client and VPN Manager Interceptor

# VPN Manager Server

![](https://img.shields.io/badge/version-1.0.0-blue) ![](https://img.shields.io/badge/java-1.7-green) ![](https://img.shields.io/badge/spring%20boot-2.1.6-orange) ![](https://img.shields.io/badge/spring%20security-2.1.6-yellow) ![](https://img.shields.io/badge/hibernate-4.3.1-blue)

## Architecture

![VPN Manager Architecture](https://github.com/satish4linux/Pigeon-API/blob/master/Untitled%20Diagram.png)

## Authorization

- Each API exposed for consumption is secured by **Basic Authentication**
- The Client needs to provide it's valid *credentials* to consume the API.
- If the client is authenticated, it depends upon consumer's *role* whether it is authorized to consume that API.
### How To Use

- The HTTP Request must contain an **Authorization** header as below:
  ```
  Authorization : Basic dXNlcm5hbWU6cGFzc3dvcmQ=
  ```
  where the later part after *Basic* is **Base64 encode** of `username:password`

## Web Services

### customization/create

- Request Method - POST
- Consumes - JSON
```
{
  "appName":"Madeena VPN",
  "appContext":"madeenavpn",
  "primaryIPAddress":"182.12.21.12",
  "secondaryIPAddress":"123.23.23.21",
  "httpPort":8084,
  "sshPort":22,
  "isServerCreationFlag":true
}
```
- Produces - JSON
```
{
  "errorCode": <error code>,
  "errorCause": <error message to be displayed>,
  "Result": null
}
```
- Logic - If the request data is valid and the customization doesn't already exist, it creates an instance of the customization. If the *isServerCreationFlag* flag is ON, it makes an HTTP GET Request to the *VPN Manager Interceptor* to get the list of server instances mapped against this customization. The received server instances are added to the customization instance and is persisted into the database.
### customization/get/{id}

- Request Method - GET
- Produces - JSON
```
{
  "errorCode": <error code>,
  "errorCause": <error message to be displayed>,
  "Result": <customization instance>
}
```
- Logic - If the request data is valid and customization exists, it returns the instance of the requested customization.

### customization/getAll

- Request Method - GET
- Produces - JSON
```
{
  "errorCode": <error code>,
  "errorCause": <error message to be displayed>,
  "Result": <list of customization instances>
}
```
- Logic - It returns list of all the customizations instance present in the database.

### customization/delete/{appId}

- Request Method - GET
- Produces - JSON
```
{
  "errorCode": <error code>,
  "errorCause": <error message to be displayed>,
  "Result": null
}
```
-Logic - If the request data is valid and the customization exists, it removes the requested customization from the database.

### customization/update

- Request Method - POST
- Consumes - JSON
```
{
  "appName":"Madeena VPN",
  "appContext":"madeenavpn",
  "primaryIPAddress":"32.12.54.12",
  "secondaryIPAddress":"",
  "httpPort":8404,
  "sshPort":22,
  "servers":    [
    {
      "serverKeyId":21,
      "serverName":"Etisalat",
      "primaryIPAddress":"86.23.12.76",
      "secondaryIPAddress":"",
      "port":821,
      "sshPort":22,
      "serverProvider":"Azure"
    },
    {
      "serverKeyId":22,
      "serverName":"Wi-Fi",
      "primaryIPAddress":"86.23.12.76",
      "secondaryIPAddress":"",
      "port":821,
      "sshPort":22,
      "serverProvider":"Azure"
    }
  ]
}
```
- Produces - JSON
```
{
  "errorCode": <error code>,
  "errorCause": <error message to be displayed>,
  "Result": null
}
```
- Logic - If the request data is valid and customization exists, it updates the requested customization. Then it makes an HTTP POST Request to *VPN Manager Interceptor* and sends the updated customization instance in body so that the server mapping of that customization application is updated.

### server/create

- Request Method - POST
- Consumes - JSON
```
{
  "serverName":"Etisalat",
  "serverKeyId":"321",
  "serverProvider":"Microsoft Azure",
  "primaryIPAddress":"182.21.23.12",
  "secondaryIPAddress":"",
  "port":821,
  "sshPort":22
}
```
- Produces - JSON
```
{
  "errorCode": <error code>,
  "errorCause": <error message to be displayed>,
  "Result": null
}
```
- Logic - If the request data is valid and the srever doesn't already exist, it creates an instance of the server. It makes a ping connection with the JAR apps deployed over the concerned OpenVPN Server to check it's status (ONLINE/OFFLINE). Then this instance is perssited in the database.

### server/get/{id}

- Request Method - GET
- Produces - JSON
```
{
  "errorCode": <error code>,
  "errorCause": <error message to be displayed>,
  "Result": <server instance>
}
```
- Logic - If the request data is valid and server exists, it returns the instance of the requested server.

### server/getAll

- Request Method - GET
- Produces - JSON
```
{
  "errorCode": <error code>,
  "errorCause": <error message to be displayed>,
  "Result": <list of server instances>
}
```
- Logic - It returns list of all the servers instance present in the database.

### server/delete/{serverId}

- Request Method - GET
- Produces - JSON
```
{
  "errorCode": <error code>,
  "errorCause": <error message to be displayed>,
  "Result": null
}
```
-Logic - If the request data is valid and the server exists, it removes the requested server from the database. Then it makes an HTTP POST Request to the *VPN Manager Interceptor* and sends the list of customizations mapped to this server, so that it's mapping can be removed from these customization applications.

### server/update

- Request Method - POST
- Consumes - JSON
```
{
  "serverName":"Etisalat",
  "serverKeyId":21,
  "primaryIPAddress":"85.12.43.56",
  "secondaryIPAddress":"",
  "port":321,
  "sshPort":22,
  "serverProvider":"Azure"
}
```
- Produces - JSON
```
{
  "errorCode": <error code>,
  "errorCause": <error message to be displayed>,
  "Result": null
}
```
- Logic - If the request data is valid and server exists, it updates the requested server. Then it makes an HTTP POST Request to *VPN Manager Interceptor* and sends the updated server instance in body so that all the customization applications mapped to this server are updated.
