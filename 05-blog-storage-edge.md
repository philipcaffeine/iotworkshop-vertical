---
page_type: blob storage container configuration note
description: blog storage container on IoT Edge 1.2
products:
- Blob storage, IoT Edge 1.2            

languages:
- Shell and python
---


## Lab objectives: 


## Pre-requisites
* Azure account: 
    Bring your own Azure account to keep all your dev works. 
* Install VS Code:
    https://code.visualstudio.com/download


----------------------------------------------------------------------------

## Time series data for Anomaly detection

tutorial link: 

Deploy storage module to iot edge
https://docs.microsoft.com/en-us/azure/iot-edge/how-to-deploy-blob?view=iotedge-2020-11


1. In the IoT Edge Modules section of the page, click the Add dropdown and select IoT Edge Module to display the Add IoT Edge Module page.

On the Module Settings tab, provide a name for the module and then specify the container image URI:

Examples:

IoT Edge Module Name: azureblobstorageoniotedge
Image URI: mcr.microsoft.com/azure-blob-storage:latest


2. Don't select Add until you've specified values on the Module Settings, Container Create Options, and Module Twin Settings tabs as described in this procedure.

* container create options

{
  "Env":[
    "LOCAL_STORAGE_ACCOUNT_NAME=<your storage account name>",
    "LOCAL_STORAGE_ACCOUNT_KEY=<your storage account key>"
  ],
  "HostConfig":{
    "Binds":[
        "<storage mount>"
    ],
    "PortBindings":{
      "11002/tcp":[{"HostPort":"11002"}]
    }
  }
}

use bind mount: /srv/containerdata:/blobroot. Make sure to follow the steps to grant directory access to the container user

/var/iotedgeblob:/blobroot


# On the Module Twin Settings tab, copy the following JSON and paste it into the box.


  {
    "deviceAutoDeleteProperties": {
      "deleteOn": <true, false>,
      "deleteAfterMinutes": <timeToLiveInMinutes>,
      "retainWhileUploading": <true,false>
    },
    "deviceToCloudUploadProperties": {
      "uploadOn": <true, false>,
      "uploadOrder": "<NewestFirst, OldestFirst>",
      "cloudStorageConnectionString": "DefaultEndpointsProtocol=https;AccountName=<your Azure Storage Account Name>;AccountKey=<your Azure Storage Account Key>; EndpointSuffix=<your end point suffix>",
      "storageContainersForUpload": {
        "<source container name1>": {
          "target": "<target container name1>"
        }
      },
      "deleteAfterUpload": <true,false>
    }
  }


  {
      "Env": [
          "LOCAL_STORAGE_ACCOUNT_NAME=philiotbasestg",
          "LOCAL_STORAGE_ACCOUNT_KEY=6KfJXg/aa0x3Ve6Tel+A3L5a97cmpuAOBggp6Ma/BY+vqvGehOHhgXmwumk12s54+M/xGhWm7hbX31QAxxcLgw=="
      ],
      "HostConfig": {
          "Binds": [
              "/var/iotedgeblob:/blobroot"
          ],
          "PortBindings": {
              "11002/tcp": [
                  {
                      "HostPort": "11002"
                  }
              ]
          }
      }
  }



  {
      "deviceAutoDeleteProperties": {
          "deleteOn": true,
          "deleteAfterMinutes": 15,
          "retainWhileUploading": true
      },
      "deviceToCloudUploadProperties": {
          "uploadOn": true,
          "uploadOrder": "NewestFirst",
          "cloudStorageConnectionString": "xxxxxxxxxxxxxxxxxxx",
          "storageContainersForUpload": {
              "blobroot": {
                  "target": "edgeblob"
              }
          },
          "deleteAfterUpload": false
      }
  }


