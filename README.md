## Introduction

- Server endpoint of VPN Manager System
- Provides secured REST-based Web APIs to perform CRUD operations
- It's APIs are consumed by VPN Manager Client and VPN Manager Interceptor

# VPN Manager Server

![](https://img.shields.io/badge/version-1.0.0-blue) ![](https://img.shields.io/badge/java-1.7-green) ![](https://img.shields.io/badge/spring%20boot-2.1.6-orange) ![](https://img.shields.io/badge/spring%20security-2.1.6-yellow) ![](https://img.shields.io/badge/hibernate-4.3.1-blue)

## Architecture

![](https://github.com/satish4linux/Pigeon-API/blob/master/Untitled%20Diagram.png)

## Authorization

- Each API exposed for consumption is secured by **Basic Authentication**
- The Consumer system needs to provide it's valid *credentials* to consume the API.
- If the consumer is authenticated, it depends upon consumer's *role* whether it is authorized to consume that API.
### How To Use

- The HTTP Request must contain an **Authorization** header as below:
  ```
  Authorization : Basic dXNlcm5hbWU6cGFzc3dvcmQ=
  ```
  where the later part after *Basic* is **Base64 encode** of `username:password`

## Web Services

### customization/create

### customization/get/{id}

### customization/getAll

### customization/delete/{appId}

### customization/update

### server/create

### server/get/{id}

### server/getAll

### server/delete/{serverId}

### server/update
