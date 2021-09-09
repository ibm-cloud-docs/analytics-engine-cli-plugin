---

copyright:
  years: 2015, 2021
lastupdated: "2021-09-09"

subcollection: analytics-engine-cli-plugin

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}

# {{site.data.keyword.iae_full_notm}} CLI plug-in for serverless instances
{: #CLI_analytics_engine}

The {{site.data.keyword.iae_full_notm}} v3 CLI provides command line options to interact with `Standard Serverless Spark` instances. You can manage instances and Spark applications with the CLI. For more information about serverless Analytics Engine instances, see [Architecture and concepts in serverless instances](/docs/AnalyticsEngine?topic=AnalyticsEngine-serverless-architecture-concepts).

To run {{site.data.keyword.iae_full_notm}} v3 CLI commands, use `ibmcloud analytics-engine-v3` or `ibmcloud ae-v3`.
{: tip}

## Prerequisites

Download and install the {{site.data.keyword.Bluemix_notm}} CLI on your local system. See [{{site.data.keyword.Bluemix_notm}} CLI](/docs/cli?topic=cloud-cli-install-ibmcloud-cli) to install the CLI.

## Install the {{site.data.keyword.iae_full_notm}} v3 CLI

Install the {{site.data.keyword.iae_full_notm}} v3 CLI by running the following command:
```
ibmcloud plugin install analytics-engine-v3
```
{: codeblock}

The following help CLI command shows that the v3 CLI supports CLI commands for:
- Instance management
- Spark application management

```
$ ibmcloud ae-v3 --help
NAME:
  analytics-engine-v3 - Create and manage serverless Spark instances and run applications.
USAGE:
  ibmcloud analytics-engine-v3 [command] [options]
ALIASES:
  analytics-engine-v3, ae-v3
COMMANDS:
  instance    Commands for Instance resource
  spark-app   Commands for SparkApp resource
```

## Instance management commands

Currently, you can only retrieve the instance details through the instance management command.

### instance show
{: #instance-show}

Use this command to return the details related to the configuration and state of a provisioned serverless instance.
```
$ ibmcloud ae-v3 instance show --help
NAME:
  show - Retrieve the details of a single instance.
USAGE:
  ibmcloud analytics-engine-v3 instance show --id ID
OPTIONS:
  -i, --id string   Identifier of the instance to retrieve.
```

The `ibmcloud` CLI provides the following global options to customize your output:

```
GLOBAL OPTIONS:
  -h, --help                Show help
  -j, --jmes-query string   Provide a JMESPath query to customize output.
      --output string       Choose an output format - can be 'json', 'yaml', or 'table'. (default "table")
  -q, --quiet               Suppresses verbose messages.
```

**Example**:

Example of using the show command.

```
$ ibmcloud analytics-engine-v3 instance show --id 970ec9c1-56ca-4517-aa1c-318994f34415 --output json
{
  "default_runtime": {
    "spark_version": "spark"
  },
  "id": "970ec9c1-56ca-4517-aa1c-318994f34415",
  "instance_home": {
    "bucket": "do-not-delete-ae-bucket-970ec9c1-56ca-4517-aa1c-318994f34415",
    "endpoint": "https://s3.direct.us-south.cloud-object-storage.appdomain.cloud",
    "hmac_access_key": "7b****************************75",
    "hmac_secret_key": "26********************************************bf",
    "provider": "ibm-cos",
    "region": "us-south",
    "type": "objectstore"
  },
  "state": "active",
  "state_change_time": "2021-09-06T11:46:41.000Z"
}
```

## Spark application commands

With the Spark application CLI commands, you can perform the following operations:

-	[Submit a Spark application](#spark-app-submit)
-	[List all applications submitted](#spark-app-list)
-	[Retrieve the details of a Spark application](#spark-app-show)
- [Show the status of a submitted application](#spark-app-status)
-	[Stop a submitted Spark application](#spark-app-status)

### spark-app submit
{: #spark-app-submit}

Use this command to submit a Spark application in an  {{site.data.keyword.iae_full_notm}} serverless instance.

```
$ ibmcloud ae-v3 spark-app submit --help
NAME:
  submit - Creates a Spark application based on the requested specification and returns the details of the submitted application.
USAGE:
  ibmcloud analytics-engine-v3 spark-app submit --instance-id INSTANCE-ID [--app APP] [--arg ARG] [--conf CONF] [--env ENV]
OPTIONS:
      --app string           Application name.
      --arg strings          Application arguments.
      --conf string          Application configurations to override
      --env string           Application environment configurations to override
      --instance-id string   The identifier of the instance where the Spark application is submitted.
```

**Command options**:

--instance-id
:   The serverless instance ID.

--app
:   File path including file name to the Spark application.

--arg
:   File path including file name of the input file to the application.

**Example**:

The following example shows you how to submit a Spark application.

```
$ ibmcloud ae-v3 spark-app submit --instance-id  d2189b-b729-61f-adc0-987001da52e3 --app "/opt/ibm/spark/examples/src/main/python/wordcount.py" --arg "/opt/ibm/spark/examples/src/main/resources/people.txt"
...
id               d0f582f7-cbf4-4c64-bf7f-72efd21c9f32   
state            accepted
```

### spark-app list
{: #spark-app-list}

Use this command to list all submitted Spark applications in an  {{site.data.keyword.iae_full_notm}} serverless instance.

```
ibmcloud ae-v3 spark-app list --help
NAME:
 list - Returns a list of all applications submitted in the specified instance.
USAGE:
 ibmcloud analytics-engine-v3 spark-app list --instance-id INSTANCE-ID    
OPTIONS:
     --instance-id string   Identifier of the instance where the applications run.
```

**Command options**:

--instance-id
:   The serverless instance ID.

**Example**:

The following example lists the submitted Spark applications.
```
$ ibmcloud analytics-engine-v3 spark-app list --instance-id 970ec9c1-56ca-4517-aa1c-318994f34415
...
finish_time                id                                     spark_application_id      start_time                 state    
2021-09-07T08:24:41+0000   fd61a66f-d68e-4401-888b-0782f778b56d   app-20210907082430-0000   2021-09-07T08:24:30+0000   finished   
2021-09-08T07:33:13+0000   89f5f9f0-83b5-4276-835e-bb164ab3c61c   app-20210908073303-0000   2021-09-08T07:33:03+0000   finished  
2021-09-08T09:44:07+0000   8cb6162c-5907-4d6e-ad56-6020b7e2629e   app-20210908094356-0000   2021-09-08T09:43:56+0000   finished   
```

### spark-app show
{: #spark-app-show}

Use this command to show the details of a submitted Spark application in an {{site.data.keyword.iae_full_notm}} serverless instance.

```
$ ibmcloud ae-v3 spark-app show --help
NAME:
  show - Returns the details related to the configuration and state of a submitted Spark application.
USAGE:
  ibmcloud analytics-engine-v3 spark-app show --instance-id INSTANCE-ID --app-id APP-ID
```

**Command options**:

--instance-id
:   The serverless instance ID.

--app-id
:   The application ID allocated to the application by the  {{site.data.keyword.iae_full_notm}} service.

**Example**:

The following example shows the details of a submitted Spark application.
```
ibmcloud ae-v3 spark-app show --instance-id 970ec9c1-56ca-4517-aa1c-318994f34415 --app-id e0ee970a-8333-417b-886c-2b15874f597b
...

application_details   <Nested Object>   
id                    e0ee970a-8333-417b-886c-2b15874f597b   
state                 finished   
start_time            2021-09-08T16:33:45.000Z   
finish_time           2021-09-08T16:33:56.000Z   
```

### spark.app status
{: #spark-app-status}

Use this command to show the state of a submitted Spark application in an {{site.data.keyword.iae_full_notm}} serverless instance.

```
ibmcloud ae-v3 spark-app status --help
NAME:
  status - Returns the status of the application identified by the app_id identifier.
USAGE:
  ibmcloud analytics-engine-v3 spark-app status --instance-id INSTANCE-ID --app-id APP-ID
OPTIONS:
      --app-id string        Identifier of the application for which details are requested.
      --instance-id string   Identifier of the instance to which the applications belongs.
```

**Command options**:

--instance-id
:   The serverless instance ID.

--app-id
:   The application ID allocated to the application by the  {{site.data.keyword.iae_full_notm}} service.

**Example**:

The following example shows how to get the status of a submitted Spark application.
```
$ ibmcloud ae-v3 spark-app status --instance-id 970ec9c1-56ca-4517-aa1c-318994f34415 --app-id ea2b10a9-ba61-4a4a-a448-d780d1218304
...

id            ea2b10a9-ba61-4a4a-a448-d780d1218304   
state         finished   
start_time    2021-09-07T07:33:36+0000   
finish_time   2021-09-07T07:34:03+0000   
```

### spark-app stop
{: #spark-app-status}

Use this command to show the stop a submitted Spark application in an {{site.data.keyword.iae_full_notm}} serverless instance.

```
ibmcloud ae-v3 spark-app stop --help
NAME:
  stop - Stops a running application identified by the app_id identifier. This is an idempotent operation. Performs no action if the requested application is already stopped or completed.
USAGE:
  ibmcloud analytics-engine-v3 spark-app stop --instance-id INSTANCE-ID --app-id APP-ID

```

**Command options**:

--instance-id
:   The serverless instance ID.

--app-id
:   The application ID allocated to the application by the  {{site.data.keyword.iae_full_notm}} service.

**Example**:

The following example shows how to stop a Spark application.
```
$ ibmcloud ae-v3 spark-app stop --instance-id 970ec9c1-56ca-4517-aa1c-318994f34415 --app-id e0ee970a-8333-417b-886c-2b15874f597b
...
OK
```