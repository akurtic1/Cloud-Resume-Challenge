# 🚀 Cloud Resume Challenge

The official link of the Cloud Resume Challenge: https://cloudresumechallenge.dev/docs/the-challenge/azure/
The Cloud Resume Challenge is hosted on this domain: https://www.cloudresumeadmir.xyz/

## 📋 Table of Contents

- [Introduction](#introduction)
- [Resources Used](#features)
- [Summary](#summary)

## 🌟 Introduction

This project is intended to be used for a securly uploading files to Azure Blob Storage.
![Diagram of the architecture tasks](./media/diagram-sharesafely.png)

## ✨ Features

These are the resources I have used. Some of them I interacted for the first time.

+ HTML
+ CSS
+ Azure Storage
+ HTTPS
+ JavaScript
+ Azure CosmosDB
+ Azure Functions (API, HTTP Trigger)
+ Python
+ IaC
+ CI/CD (Backend & Frontend)


## ☁️ Step 1 - HTML & CSS

In this simple task, I developed a simple HTML & CSS resume to work with on this project. This resume is not
intended to be fancy styled because I was focused on the tasks that are related to Azure & Cloud.
+ HTML Link: 
+ CSS Link: 

## ☁️ Step 2 - Static Website

In this task, I have deployed a static website on the Azure Blob Storage. With the enabled static website, I
received the link and tested if the static website is available. You can see on the screenshot below that
the link is working:
![Screenshot of the Static Website](./media/diagram-sharesafely.png)

## ☁️ Step 3 - HTTPS

In this task, I have deployed Azure Front Door and configured the properties.
I used the following Microsoft documentation to do this task: https://learn.microsoft.com/en-us/azure/frontdoor/integrate-storage-account
![Screenshot of the Azure Front Door](./media/diagram-sharesafely.png)

## ☁️ Step 4 - Configuring Azure DNS

So, in this task I have signed up a custom domain using GoDaddy and configured it with the Azure DNS & Azure Front Door.
In the screenshot below, you can see the overview of the configuration for the Azure Front Door.
![Azure Front Door](./media/diagram-sharesafely.png)

You can check the custom domain by the following link: [www.cloudresumeadmir.xyz](https://www.cloudresumeadmir.xyz/)
This is the custom domain that I'm using and the endpoint goes like this: resumechallenge-g3gkgjaecrezbqdr.z01.azurefd.net

Also, I have configured in the Azure Front Door Manager that only HTTPS protocol is allowed.

## ☁️ Step 5 - Configuring the Page Visitor Counter

In this task, I have deployed Azure Functions App and Azure Data Storage Service.
I edited the HTML File with the following script: 
        <script>
            async function fetchVisitorCount() {
                try {
                    const response = await fetch('https://countervisit.azurewebsites.net/api/VisitorCounter');
                    const data = await response.json();
                    document.getElementById('count').textContent = data.count;
                } catch (error) {
                    console.error('Error fetching visitor count:', error);
                }
            }
          
            document.addEventListener('DOMContentLoaded', fetchVisitorCount);
          </script>

You can see on the short gif below that the Visitor Counter is working.
![Testing the Vistor Counter Function](./media/diagram-sharesafely.png)

The function didn't work at first so I had to configure the properties in the Azure Function to make it work.
One of the reasons why my function didn't work was because the function wasn't set to "anonymous".

## ☁️ Step 6 - Database (CosmosDB)

In this task, I  deployed CosmosDB and used TableAPI to retreive and update its coint in database.
I also used serverless capacity mode for less payment, becasue I don't store or retreive that much data.
On the gif below, you will see that visitor counter is retreived by the Azure CosmosDB.
![CosmosDB Table API Counter](./media/diagram-sharesafely.png)

You can see the following example of the function code below:

    const connectionString = process.env["CosmosDBConnectionString"];
    const tableName = "visitorcounter";
    const rowKey = "count";

    const tableClient = TableClient.fromConnectionString(connectionString, tableName);

    try {
        // Ensure the table (container) exists
        try {
            await tableClient.createTable();
        } catch (error) {
            if (error.statusCode !== 409) { // 409 means the table already exists
                throw error;
            }
        }

        let entity;
        try {
            entity = await tableClient.getEntity("visitor", rowKey); // Use a constant value for partition key
            entity = {
                partitionKey: entity.partitionKey,
                rowKey: entity.rowKey,
                count: entity.count + 1
            };
            await tableClient.updateEntity(entity, "Merge");
        } catch (error) {
            if (error.statusCode === 404) {
                entity = { partitionKey: "visitor", rowKey, count: 1 }; // Use a constant value for partition key
                await tableClient.createEntity(entity);
            } else {
                throw error;
            }
        }

## ☁️ Step 7 - Python

In this task, for the first time I encountered with Python and I used some of the following commands to set up the environment.
I had an issiue with the python because "Python -m" didn't work so I figured it out that I have to use "py" instead of "python".

First, I used the command to create the Virtual Environment "py -m venv myenv"
After previous step, I installed Azure SDK packages:
+ pip install azure-functions
+ pip install azure-cosmos

After working on Python Function, I couldn't deploye the function to Azure because python functions are
only support for Linux. After that I have deployed the new Azure Function App with version for Python.
You can see on the screenshot below, that the deployment was successful:
I used the following command to deploy thej function: func azure functionapp publish functionpython1113 --force

![Testing the Vistor Counter Function](./media/diagram-sharesafely.png)

Also, after this task, I have reseached about the python tests and how they are performed.

## ☁️ Step 8 - Infrastructure as Code

So in this task, I have developed an ARM template that will deploy Data Storage Account, Azure Function & Azure CosmosDB.
![Preview the ARM Template](./media/diagram-sharesafely.png)

## ☁️ Step 9 - Source Control 

In this task, I simply created a repository for my backend code.
The repository is available at: https://github.com/akurtic1/visitor-counter-backend/tree/main
This is the first time I commit the repository and push the code and also
I encountered with the some commands for the first time.

## ☁️ Step 10 - CI/CD (Back end)

In this task, I set up GitHub Actions such that when you push an update to your ARM template. If the tests pass, the ARM application should get packaged and deployed to Azure.
You can preview the YAML file and template file on the link below:
![YAML file](./media/diagram-sharesafely.png)
![ARM Template](./media/diagram-sharesafely.png)

You can also preview the screenshot below of the deployment:
![Deployment - Azure Resources](./media/diagram-sharesafely.png)

This template deploys resources such as: Function App, Azure CosmosDB, Storage Account...

## ☁️ Step 11 - CI/CD (Front End)

So in this task, I set up a GitHub actions where when we deploy some files they will be automatically added
into the Azure Storage Account which I had to set up in the .yaml file.
You can preview the YAML file on the link below:
![YAML file](./media/diagram-sharesafely.png)


