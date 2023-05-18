---
title: "Create App Registrations for API and Client Apps Using Azure Portal"
description: "meta description"
image: "images/post/02/page.png"
date: 2023-04-16T16:56:47+06:00
draft: false
author: "Yuru Zuo"
tags: ["Azure", "Azure AD", "OAuth", "Postman", "App Registration", "API", "Client App"]
categories: ["Azure"]
---

# How to Create App Registrations for API and Client Apps Using Azure Portal

Creating app registrations for API and client apps in Azure Active Directory (Azure AD) is the first step in enabling authentication and authorization for your apps. This blog post will guide you through the process of creating app registrations for both your API app and client app using Azure Portal.

## **Creating App Registration for API App**

1. Navigate to Azure Portal and go to Azure AD.
2. Select "App Registrations" and click "New registration."
3. Name your app registration and click "Register."
4. Once the app registration is created, go to "Expose an API."
5. Set the ID URI and add a scope (e.g., user read, etc.).

## **Creating App Registration for Client App (Postman)**

1. Navigate to Azure Portal and go to Azure AD.
2. Select "App Registrations" and click "New registration."
3. Name your app registration and click "Register."
4. Once the app registration is created, go to "Authentication" and add a platform.
5. Select "Web" and add an access token with a redirect URL.
6. Go to "Certificate & Secret" and create a new client secret with a name and period. Save the value somewhere safe (e.g., zvc8Q~k47ySVRWFxxxxxxxxxxxxxxxKi).

## *etting up OAuth 2.0 in Postman

1. Open Postman and set up OAuth 2.0 as shown in Picture 1 below.
![postman](./images/app-reg-1.png)
2. Set the **`tenant_id`**, and **`client_secret`** from the client app.
3. Set the **`client_id`** from the API app.
4. Copy and paste the **`Scope`** from the API app.
5. Request a token, which would show up a dialog saying it requires admin approval.

## **Configuring API Permissions and Client App**

1. Uncheck "Access token" in the client app.
2. In the client app, add API permissions in the scope with delegate permission.
3. If Request admin approval is stilled required
    1. Add the client app ID and scope to the API app.
    2. Go to "Expose an API" and add a client application.

## Check the access token

Once you got that token, you can validate the token from [jwt.ms](https://jwt.ms/).

## Source code

Final code can be found [here](https://dev.azure.com/zoeylearning/lime/_git/LimeScooter)