---

copyright:
  years: 2015, 2020
lastupdated: "2021-07-26"

subcollection: analytics-engine-cli-plugin

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}

# {{site.data.keyword.iae_full_notm}} CLI plug-in for classic instances
{: #CLI_analytics_engine_classic}

Use the {{site.data.keyword.Bluemix_notm}} command-line interface (CLI) to interact with the cluster for an  {{site.data.keyword.iae_full_notm}} classic service instance.

## Prerequisites
{: #CLI_analytics_engine_classic-1}

To use the {{site.data.keyword.Bluemix_notm}} CLI, download and install the following packages on your local system. Do not install the packages on the {{site.data.keyword.iae_full_notm}} cluster.

- The [Cloud Foundry CLI](https://github.com/cloudfoundry/cli/blob/master/README.md#installing-using-a-package-manager)

- The [{{site.data.keyword.Bluemix_notm}} CLI](/docs/cli?topic=cloud-cli-install-ibmcloud-cli)

## Install the {{site.data.keyword.iae_full_notm}} CLI
{: #CLI_analytics_engine_classic-2}

For details about the {{site.data.keyword.Bluemix_notm}} CLI plugin installation, see the [documentation](/docs/cli?topic=cloud-cli-getting-started).

To list the plugins in the {{site.data.keyword.Bluemix_notm}} repository:

```
ibmcloud plugin repo-plugins -r Bluemix
```
{: codeblock}

To install the {{site.data.keyword.iae_full_notm}} plugin from the {{site.data.keyword.Bluemix_notm}} repository:

```
ibmcloud plugin install -r Bluemix analytics-engine
```
{: codeblock}

Note: If the default {{site.data.keyword.Bluemix_notm}} repository is not available from your {{site.data.keyword.Bluemix_notm}} CLI, you might need to add the repository. This only needs to be done once.

```
ibmcloud plugin repo-add Bluemix https://plugins.ng.bluemix.net
```
{: codeblock}

## Get started
{: #CLI_analytics_engine_classic_3}

1. Run the [spark-endpoint](#endpoint) command to set the {{site.data.keyword.iae_full_notm}} cluster endpoint. The argument for `endpoint` is the IP or hostname of the cluster management node.
    ```
    ibmcloud ae endpoint https://iae-tmp-867-mn001.<changeme>.ae.appdomain.cloud

    ```

    where `<changeme>` is the {{site.data.keyword.Bluemix_short}} hosting location, for example `us-south`.

    When prompted, accept the default port numbers for the Ambari(9443) and Knox(8443) services.

    Response if your {{site.data.keyword.Bluemix_short}} hosting location is `us-south`:

    ```
    Registering endpoint 'https://iae-tmp-867-mn001.us-south.ae.appdomain.cloud'...
    Ambari Port Number [Optional: Press enter for default value] (9443)>
    Knox Port Number [Optional: Press enter for default value] (8443)>
    OK
    Endpoint 'https://iae-tmp-867-mn001.us-south.ae.appdomain.cloud' set.
    ```

1. Run the [spark-submit](#spark-submit) command. For example:
    ```
    $ ibmcloud ae spark-submit --className org.apache.spark.examples.SparkPi local:/usr/hdp/current/spark2-client/jars/spark-examples.jar
    ```

    Enter the {{site.data.keyword.iae_full_notm}} cluster login credentials at the prompts. To set the default username for command execution, see the [`username`](#username) command.

    Response if your {{site.data.keyword.Bluemix_short}} hosting location is `us-south`:
    ```
    User (clsadmin)>
    Password>
    Contacting endpoint 'https://iae-tmp-867-mn001.us-south.ae.appdomain.cloud:8443'...
    Job ID '17'
    Waiting for job to return application ID. Will check every 10 seconds, and stop checking after 2 minutes. Press Control C to stop waiting.
    Finished contacting endpoint 'https://iae-tmp-867-mn001.us-south.ae.appdomain.cloud:8443'
    OK
    Job ID '17'
    Application ID 'application_1491850285904_0023'
    Done
    ```

## endpoint
{: #endpoint}

Use this command to set the {{site.data.keyword.iae_full_notm}} server endpoint if your {{site.data.keyword.Bluemix_short}} hosting location is `us-south` for example.

```
$ ibmcloud ae endpoint https://chs-xxx-xxx-mn001.us-south.ae.appdomain.cloud
Registering endpoint 'https://chs-xxx-xxx-mn001.us-south.ae.appdomain.cloud'...
Ambari Port Number [Optional: Press enter for default value] (9443)>
Knox Port Number [Optional: Press enter for default value] (8443)>
OK
Endpoint 'https://chs-xxx-xxx-mn001.us-south.ae.appdomain.cloud' set.
```

**Prerequisites**: None.

**Command options**:

--unset
:   Removes all endpoint information.

ENDPOINT_URI
:   Sets endpoint to a provided value. To create the ENDPOINT URI:

    - Get the hostname. See [Getting credentials](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-retrieve-endpoints).
    - Set ENDPOINT_URI=https://hostname


**Examples**:

Examples of using the endpoint command.

```
ibmcloud ae endpoint
      Displays current endpoint configuration

ibmcloud ae endpoint ENDPOINT_URI
      Sets endpoint to a provided value

ibmcloud ae endpoint --unset
      Removes all endpoint information

ENDPOINT_URI is the URI of the endpoint without a port number, e.g. https://www.example.com
```

## file-system
{: #file-system}

Use this command to interact with HDFS on the {{site.data.keyword.iae_full_notm}} cluster.

```
ibmcloud ae file-system [--user <user>] [--password <password>] get SRC DST
  SRC is the HDFS file
  DST is the local file

ibmcloud ae file-system [--user <user>] [--password <password>] ls [DIR]
  DIR is the optional HDFS directory

ibmcloud ae file-system [--user <user>] [--password <password>] mkdir DIR
  DIR is the HDFS directory

ibmcloud ae file-system [--user <user>] [--password <password>] mv SRC DST
  SRC and DST are the HDFS file or directory

ibmcloud ae file-system [--user <user>] [--password <password>]  SRC DST
  SRC is the local file
  DST is the HDFS file

ibmcloud ae file-system [--user <user>] [--password <password>] rm [-R] FILE

  FILE is the HDFS file or directory
  -R, --R remove directories and their contents recursively
```

**Prerequisites**: None.

**Command options**:

--user
:   Removes all endpoint information.

--password
:   The password for the selected user.

**Subcommand options for file-system**:

- **get**

    Use this subcommand to copy a single file to the local file system from a remote HDFS file system.

    ```
    ibmcloud ae file-system [--user <user>] [--password <password>] get [SRC] [DST]
    ```

    **Command options**:

    -- SRC
    : The file path to the remote HDFS file system.

    -- DST
    : The file path on the local file system.

    **Example**:

    ```
    $ ibmcloud ae file-system get /user/clsadmin/run.log run.log
    User (clsadmin)>
    Password>
    Copying HDFS file '/user/clsadmin/run.log' contents to 'run.log'...
    OK
    ```
- **ls**

    Use this subcommand to list the contents of a remote destination directory.

    ```
    ibmcloud ae file-system [--user <user>] [--password <password>] ls [DIR]
    ```

    **Command options**:
    --DIR
    :   The path of the remote HDFS directory. This option is optional. If this option is  empty, the contents of the HDFS user home directory is returned.

    **Examples**:
    ```
    $ ibmcloud ae file-system ls
    User (clsadmin)>
    Password>
    Found 2 files in '/user/clsadmin'...
    drwxr-xr-x  0  clsadmin  biusers  0  Tue Apr 11 12:33:28 CDT 2017  .sparkStaging
    drwxr-xr-x  0  clsadmin  biusers  0  Tue Apr 11 12:33:11 CDT 2017  cli
    OK
    ```
    ```
    $ ibmcloud ae file-system ls /user/clsadmin/cli
    User (clsadmin)>
    Password>
    Found 3 files in '/user/clsadmin/cli'...
    drwxr-xr-x  0  clsadmin  biusers  0  Tue Apr 11 11:19:36 CDT 2017  0fdd1caf-a21f-4a18-970c-e40255e8f0ad
    drwxr-xr-x  0  clsadmin  biusers  0  Tue Apr 11 11:19:16 CDT 2017  28c3a243-59cc-4bda-a186-a5bd77f62570
    drwxr-xr-x  0  clsadmin  biusers  0  Tue Apr 11 12:33:11 CDT 2017  5b9a0a4c-21c1-4b41-b8bb-2b9ca47e1a51
    OK
    ```
- **mkdir**

    Use this subcommand to create a directory on the remote HDFS file system.

    ```
    ibmcloud ae file-system [--user <user>] [--password <password>] mkdir [DIR]
    ```

    **Command options**:

    --DIR
    :   The file path to the remote HDFS file system.


    **Example**:

    ```
    $ ibmcloud ae file-system get /user/clsadmin/run.log run.log
    User (clsadmin)>
    Password>
    Copying HDFS file '/user/clsadmin/run.log' contents to 'run.log'...
    OK
    ```

- **mv**

    Use this subcommand to move a single file from a source to a destination file path on the remote HDFS file system.

    ```
    ibmcloud ae file-system [--user <user>] [--password <password>] mv [SRC] [DST]
    ```

    **Command options**:

    SRC
    :   The source file path on the remote HDFS file system.

    DST
    :   The target file path on the remote HDFS  file system.


    **Examples**:

    ```
    $ ibmcloud ae file-system mv /user/clsadmin/run.log /user/clsadmin/run.log.bkp
    User (clsadmin)>
    Password>
    Moving HDFS path from '/user/clsadmin/run.log' to '/user/clsadmin/run.log.bkp'...
    OK
    ```

    ```
    $ ibmcloud ae file-system mv /user/clsadmin/run.log /tmp/run.log.bkp
    User (clsadmin)>
    Password>
    Moving HDFS path from '/user/clsadmin/run.log' to '/tmp/run.log.bkp'...
    OK
    ```
- **put**

    Use this subcommand to copy a single file from the local file system to the remote HDFS file system.

    ```
    ibmcloud ae file-system [--user <user>] [--password <password>] put [SRC] [DST]
    ```

    **Command options**:

    SRC
    :   The source file path on the local file system.

    DST
    :   The target file path on the remote HDFS file system.

    **Example**:

    ```
    $ ibmcloud ae file-system put sparkpi_2.10-1.0.jar /user/clsadmin/jobs/sparkpi_2.10-1.0.jar
    User (clsadmin)>
    Password>
    Copying local file 'sparkpi_2.10-1.0.jar' to HDFS location '/user/clsadmin/jobs/sparkpi_2.10-1.0.jar'...
    OK
    ```
- **rm**

    Use this subcommand to remove a file or directory from the remote HDFS.

    ```
    ibmcloud ae file-system [--user <user>]  [--password <password>] rm [-R] FILE
    ```

    **Command options**:

    FILE
    :   The file path on the remote HDFS file system.

    **Examples**:

    Removes a file:

    ```
    $ ibmcloud ae file-system rm /user/clsadmin/run.log
    User (clsadmin)>
    Password>
    Are you sure you want to remove '/user/clsadmin/run.log' from HDFS ? [y/N]> y
    Removing...
    OK
    ```

    Removes directory recursively:

    ```
    $ ibmcloud ae file-system rm -R /user/clsadmin/logs
    User (clsadmin)>
    Password>
    Are you sure you want to remove '/user/clsadmin/logs' from HDFS ? [y/N]> y
    Removing...
    OK
    ```

## kernels
{: #kernels}

Use this command to interacts with Jupyter Kernel Gateway (JKG) kernels on the  {{site.data.keyword.iae_full_notm}} cluster.

```
ibmcloud ae kernels [--user <user>] [--password <password>] create SPEC_NAME
    SPEC_NAME is the name of a kernel specification.
    Use 'ibmcloud ae kernels specs' for available kernel specification names.

ibmcloud ae kernels [--user <user>] [--password <password>] delete ID
    ID is the kernel ID that will be deleted

ibmcloud ae kernels [--user <user>] [--password <password>] get ID
    ID is the kernel ID whose information will be returned

ibmcloud ae kernels [--user <user>] [--password <password>] interrupt ID
    ID is the kernel ID that will be interrupted

ibmcloud ae kernels [--user <user>] [--password <password>] ls

ibmcloud ae kernels [--user <user>] [--password <password>] restart ID
    ID is the kernel ID that will be restarted

ibmcloud ae kernels [--user <user>] [--password <password>] specs
```

**Prerequisites**: None.

**Command options**:

--user
:   A user with the authority to get version information.

--password
:   The password for the selected user.


**Subcommand options for kernels**:

  - **create**

    Use this subcommand to create a kernel instance.

    ```
    ibmcloud ae kernels [--user <user>] [--password <password>] create SPEC_NAME
    ```

    **Command options**:

    --SPEC_NAME
    :   The name of a kernel specification.
    specs
    :   Use `ibmcloud ae kernels specs` for the available kernel specification names.


  - **delete**

    Use this subcommand to kill a kernel instance and remove the kernel ID.

    ```
    ibmcloud ae kernels [--user <user>] [--password <password>] delete ID
    ```

    **Command options**:

    ID
    :   The kernel ID that will be deleted.

  - **get**

    Use this subcommand to get information about a specific kernel instance.

    ```
    ibmcloud ae kernels [--user <user>] [--password <password>] get ID
    ```

    **Command options**:

    ID
    :   The kernel ID whose information will be returned.


  - **interrupt**

    Use this subcommand to issue an interrupt request to the kernel instance.

    ```
    ibmcloud ae kernels [--user <user>] [--password <password>] interrupt ID
    ```

    **Command options**:

    ID
    :   The kernel ID that will be interrupted.


- **ls**

    Use this subcommand to list kernel instances.

    ```
    ibmcloud ae kernels [--user <user>] [--password <password>] ls
    ```

    **Command options**:

    None.

- **restart**

    Use this subcommand to issue a restart request to the kernel instance.

    ```
    ibmcloud ae kernels [--user <user>] [--password <password>] restart ID
    ```

    **Command options**:

    ID
    :   The kernel ID of the kernel that is restarted.


- **specs**

    Use this subcommand to show the kernel specifications.

    ```
    ibmcloud ae kernels [--user <user>] [--password <password>] specs
     ```

    **Command options**:

    None.

## username
{: #username}

Use this command to set the default username for {{site.data.keyword.iae_full_notm}} commands.

```
ibmcloud ae username
Displays current username configuration

ibmcloud ae username --unset
Removes username information

ibmcloud ae username USERNAME
Sets the username to the provided value

USERNAME is the name of the user to authenticate with, e.g., clsadmin
```

**Prerequisites**: None.

**Command options**:

--user
:   A user with authority to get the version information.

--password
:   The password for the selected user.

--unset
:   Removes username.


**Examples**:

Examples of using the username command.

Set username:

```
$ ibmcloud ae username clsadmin
Registering user 'clsadmin'...
OK
Default user name 'clsadmin' set.
```

View username:

```
ibmcloud ae username
OK

Username:   clsadmin
```

Erase username:

```
$ ibmcloud ae username --unset
OK
Username has been erased.
```

## versions
{: #versions}

Use this command to get the versions of the services running in the {{site.data.keyword.iae_full_notm}} cluster.

```
ibmcloud ae versions [--user <user>] [--password <password>] [--serviceDetails]
```

**Prerequisites**: None.

**Command options**:

--user
:   A user with authority to get the version information.

--password
:   The password for the selected user.

--serviceDetails
:   Provides information about the services installed within the cluster.


**Examples**:

Examples of using the versions command.

View the IOP stack version:
```
$ ibmcloud ae versions
Current user is 'clsadmin'
Password>
OK
Combined hadoop spark cluster version: BigInsights-4.3
```
View all service versions:

```
$ ibmcloud ae versions --serviceDetails
Current user is 'clsadmin'
Password>
OK
Service Name     Service Version
Ambari Infra     0.1.0
Ambari Metrics   0.1.0
Flume            1.7.0
HBase            1.2.4
HDFS             2.7.3
...
```

## spark‐job‐cancel
{: #spark‐job‐cancel}

Use this command to cancel a Spark job submitted on the {{site.data.keyword.iae_full_notm}} cluster.

```
ibmcloud ae spark-job-cancel [--user <user>] [--password <password>] JOB-ID

JOB-ID is the identifier for the job. This value is returned from spark-submit, spark-job-status or spark-job-statuses
```

**Prerequisites**: None.

**Command options**:

--user
:   A user with authority to get the version information.

--password
:   The password for the selected user.

**Example**:

The following example shows how to cancel a Spark job if your {{site.data.keyword.Bluemix_short}} hosting location is `us-south`.

```
$ ibmcloud ae spark-job-cancel 10
User (clsadmin)>
Password>
Contacting endpoint 'https://chs-xxx-xxx-mn001.us-south.ae.appdomain.cloud:8443'...
Finished contacting endpoint 'https://chs-xxx-xxx-mn001.us-south.ae.appdomain.cloud:8443'
OK
deleted
```

## spark‐job‐status
{: #spark‐job‐status}

Use this command to retrieve the status of a Spark job submitted on the {{site.data.keyword.iae_full_notm}} cluster.

```
ibmcloud ae spark-job-status [--user <user>] [--password <password>] JOB-ID

JOB-ID is the identifier for the job. This value is returned from spark-submit, spark-job-status or spark-job-statuses.
```

**Prerequisites**: None.

**Command options**:

--user
:   A user with authority to get the version information.

--password
:   The password for the selected user.

**Example**:

To view the status of a job:

```
$ ibmcloud ae spark-job-status 3
User (clsadmin)>
Password>
Contacting endpoint 'https://169.54.195.210:8443'...
Finished contacting endpoint 'https://169.54.195.210:8443'
OK
App ID 'application_1491850285904_0004'
State 'success'
App Info 'driverLogUrl' = ''
App Info 'sparkUiUrl' = 'http://enterprise-mn001.rocmg01.wdp-chs.ibm.com:8088/proxy/application_1491850285904_0004/'
Log lines '     diagnostics: [Tue Apr 11 17:33:16 +0000 2017] Application is Activated, waiting for resources to be assigned for AM.  Details : AM Partition = <DEFAULT_PARTITION> ; Partition Resource = <memory:20480, vCores:4> ; Queue's Absolute capacity = 100.0 % ; Queue's Absolute used capacity = 0.0 % ; Queue's Absolute max capacity = 100.0 % ;
ApplicationMaster host: N/A
ApplicationMaster RPC port: -1
       queue: default
       start time: 1491931996616
       final status: UNDEFINED
       tracking URL: http://enterprise-mn001.rocmg01.wdp-chs.ibm.com:8088/proxy/application_1491850285904_0004/
       user: clsadmin
17/04/11 17:33:16 INFO ShutdownHookManager: Shutdown hook called
17/04/11 17:33:16 INFO ShutdownHookManager: Deleting directory /tmp/spark-6fbb8870-edea-47e0-9e2b-be2901b4bc2f'
```

## spark‐job‐statuses
{: #spark‐job‐statuses}

Use this command to retrieve the status of all Spark jobs submitted on the {{site.data.keyword.iae_full_notm}} cluster.

```
ibmcloud ae spark-job-statuses [--user <user>] [--password <password>] [--includeSubmissionLogs]
```

**Prerequisites**: None.

**Command options**:

--user
:   A user with authority to get the version information.

--password
:   The password for the selected user.

--includeSubmissionLogs
:   Flag to indicate if the logs should be included in the status list.

**Examples**:

To view the status of all jobs if your {{site.data.keyword.Bluemix_short}} hosting location is `us-south`:

```
$ ibmcloud ae spark-job-statuses
User (clsadmin)>
Password>
Start index to fetch jobs [Optional: Press enter for no value]>
Number of jobs [Optional: Press enter for no value]>
Contacting endpoint 'https://chs-xxx-xxx-mn001.us-south.ae.appdomain.cloud:8443'...
Finished contacting endpoint 'https://chs-xxx-xxx-mn001.us-south.ae.appdomain.cloud:8443'
OK

Start index of jobs '0'
Total jobs active '1'
Displaying '1' jobs:

ID '3'
App ID 'application_1491850285904_0004'
State 'success'
App Info 'driverLogUrl' = ''
App Info 'sparkUiUrl' = 'http://chs-xxx-xxx-mn002.us-south.ae.appdomain.cloud:8088/proxy/application_1491850285904_0004/'
```

To show the logs with the status of all jobs if the  {{site.data.keyword.Bluemix_short}} hosting location is `us-south`:

```
$ ibmcloud ae spark-job-statuses --includeSubmissionLogs
User (clsadmin)>
Password>
Start index to fetch jobs [Optional: Press enter for no value]>
Number of jobs [Optional: Press enter for no value]>
Contacting endpoint 'https://chs-xxx-xxx-mn001.us-south.ae.appdomain.cloud:8443'...
Finished contacting endpoint 'https://chs-xxx-xxx-mn001.us-south.ae.appdomain.cloud:8443'
OK

Start index of jobs '0'
Total jobs active '1'
Displaying '1' jobs:

ID '3'
App ID 'application_1491850285904_0004'
State 'success'
App Info 'driverLogUrl' = ''
App Info 'sparkUiUrl' = 'http://chs-xxx-xxx-mn002.us-south.ae.appdomain.cloud:8088/proxy/application_1491850285904_0004/'
Log lines '     diagnostics: [Tue Apr 11 17:33:16 +0000 2017] Application is Activated, waiting for resources to be assigned for AM.  Details : AM Partition = <DEFAULT_PARTITION> ; Partition Resource = <memory:20480, vCores:4> ; Queue's Absolute capacity = 100.0 % ; Queue's Absolute used capacity = 0.0 % ; Queue's Absolute max capacity = 100.0 % ;
ApplicationMaster host: N/A
ApplicationMaster RPC port: -1
queue: default
start time: 1491931996616
final status: UNDEFINED
tracking URL: http://chs-xxx-xxx-mn002.us-south.ae.appdomain.cloud:8088/proxy/application_1491850285904_0004/
user: clsadmin
17/04/11 17:33:16 INFO ShutdownHookManager: Shutdown hook called
17/04/11 17:33:16 INFO ShutdownHookManager: Deleting directory /tmp/spark-6fbb8870-edea-47e0-9e2b-be2901b4bc2f'
```

## spark‐logs
{: #spark‐logs}

Use this command to get the Spark job logs.

```
ibmcloud ae spark-logs [--user <user>] [--password <password>] [--output <folder>] [--driver] [--executor] JOB-ID

ibmcloud ae spark-logs [--user <user>] [--password <password>] [--output <folder>] (--driver | --executor) --applicationID APPLICATION-ID

JOB-ID is the identifier for the job. This value is returned from spark-submit, spark-job-status or spark-job-statuses.

APPLICATION-ID is the YARN application ID for the job. This value is returned from spark-submit, spark-job-status or spark-job-statuses.
```

**Prerequisites**: None.

**Command options**:

--user
:   A user with authority to get the version information.

--password
:   The password for the selected user.

--driver
:   Flag to indicate if the Spark driver logs should be included. If APPLICATION-ID is provided, this flag or the executor flag is required.

--executor
:   Flag to indicate if the Spark executor logs should be included. If APPLICATION-ID is provided, this flag or the driver flag is required.

--outputDir
:   Write output into this folder.

--applicationID
:   Flag to indicate that an APPLICATION-ID will be supplied rather than a JOB-ID. If this flag is not set, JOB-ID is expected.


**Examples**:

To get the submission log:

```
$ ibmcloud ae spark-logs 2
User (clsadmin)>
Password>
Retreiving logs for job id '2'...
OK
Creating output file '/workspace/wdp-ae/jobid_2_submission.log'...
OK
Writing...
OK
```

To get the driver log:

Using the `--driver` flag will retrieve the spark driver logs. The following example shows how to get the Spark logs if your {{site.data.keyword.Bluemix_short}} hosting location is `us-south`.

```
$ ibmcloud ae spark-logs --driver --outputDir driverlogs 2
User (clsadmin)>
Password>
Retreiving YARN app id for job id '2'...
OK
App id: application_1491850285904_0003
Retreiving app state in YARN...
OK
State: FINISHED
Retreiving spark driver info...
OK
Spark driver ContainerID: container_1491850285904_0003_01_000001
Spark driver Node: chs-xxx-xxx-xxxxx.us-south.ae.appdomain.cloud
Locating spark driver logs in HDFS...
OK
Downloading log file 'chs-xxx-xxx-xxxxx.us-south.ae.appdomain.cloud_45454'...
OK
Parsing raw log TFile...
Creating output file 'driverlogs/jobid_2_container_1491850285904_0003_01_000001_driver_stderr.log'...
OK
Writing...
OK
Creating output file 'driverlogs/jobid_2_container_1491850285904_0003_01_000001_driver_stdout.log'...
OK
Writing...
OK
```

To get the executor log:

Using the `--executor` flag will retrieve the spark executor logs. The following examples shows how to get the executor logs if your {{site.data.keyword.Bluemix_short}} hosting location is `us-south`.

```
$ ibmcloud ae spark-logs --executor --outputDir executorlogs 2
User (clsadmin)>
Password>
Retreiving YARN app id for job id '2'...
OK
App id: application_1491850285904_0003
Retreiving app state in YARN...
OK
State: FINISHED
Retreiving spark driver info...
OK
Spark driver ContainerID: container_1491850285904_0003_01_000001
Spark driver Node: chs-xxx-xxx-xxxxx.us-south.ae.appdomain.cloud
Locating spark driver logs in HDFS...
OK
Downloading log file 'chs-xxx-xxx-xxxxx.us-south.ae.appdomain.cloud_45454'...
OK
Parsing raw log TFile...
OK
Creating output file 'executorlogs/jobid_2_container_1491850285904_0003_01_000003_executor_stderr.log'...
OK
Writing...
OK
Creating output file 'executorlogs/jobid_2_container_1491850285904_0003_01_000003_executor_stdout.log'...
OK
Writing...
OK
Creating output file 'executorlogs/jobid_2_container_1491850285904_0003_01_000002_executor_stderr.log'...
OK
Writing...
OK
Creating output file 'executorlogs/jobid_2_container_1491850285904_0003_01_000002_executor_stdout.log'...
OK
Writing...
OK
```

To get the driver or executor logs by using the YARN application ID:

Because the job ID is short lived, you can also obtain logs that use the YARN application ID by using the `--applicationID` flag. The applicationID value is printed to the console when the job is submitted in synchronous mode (`spark-submit` without the `--asynchronous` flag).

The following examples shows how to get logs by using YARN if your {{site.data.keyword.Bluemix_short}} hosting location is `us-south`.
```
$ ibmcloud ae spark-logs --driver --applicationID application_1491850285904_0003
User (clsadmin)>
Password>
Retreiving app state in YARN...
OK
State: FINISHED
Retreiving spark driver info...
OK
Spark driver ContainerID: container_1491850285904_0003_01_000001
Spark driver Node: chs-xxx-xxx-xxxxx.us-south.ae.appdomain.cloud
Locating spark driver logs in HDFS...
OK
Downloading log file 'chs-xxx-xxx-xxxxx.us-south.ae.appdomain.cloud_45454'...
OK
Parsing raw log TFile...
Creating output file '/workspace/wdp-ae/appid_application_1491850285904_0003_container_1491850285904_0003_01_000001_driver_stderr.log'...
OK
Writing...
OK
Creating output file '/workspace/wdp-ae/appid_application_1491850285904_0003_container_1491850285904_0003_01_000001_driver_stdout.log'...
OK
Writing...
OK
```

## spark‐submit
{: #spark‐submit}

Use this command to submit a Spark job to the {{site.data.keyword.iae_full_notm}} cluster.

```
ibmcloud ae spark-submit [--user <user>] [--password <password>] [--proxyUser <user>] [--className <main-class>] [--args <arg>]... [--jars <jar>]... [--pyFiles <file>]... [--files <file>]... [--driverMemory <memory>] [--driverCores <number>] [--executorMemory <memory>] [--executorCores <number>] [--numExecutors <number>] [--archives <archive>]... [--queue <queue>] [--name <value>] [--conf <key=val>]... [--asynchronous] FILE

FILE is the file containing the application to execute
```

**Prerequisites**: None.

**Command options**:

--user
:   A user with authority to access the cluster.

--password
:   The password for the selected cluster user.

--proxyUser
:   User to impersonate when running the job. Use `clsadmin` to avoid permission issues.

--className
: Application Java/Spark main class

--args
:   Command-line arguments for the application. The argument can be repeated multiple times.

--jars
:   Jars to be used in this Job. Argument can be repeated multiple times.

--pyFiles
:   Python files to be used in this job. Argument can be repeated multiple times.

--files
:   Files to be used in this job. Argument can be repeated multiple times.

--driverMemory
:   Amount of memory to use for the driver process.

--driverCores
:   Number of cores to use for the driver process.

--executorMemory
:   Amount of memory to use per executor process.

--executorCores
:   Number of cores to use for each executor.

--numExecutors
:   Number of executors to launch for this session.

--archives
:   Archives to be used in this session. Argument can be repeated multiple times.

--queue
:   The name of the YARN queue to submit the job to.

--name
:   The name of this session.

--conf
:   Spark configuration property. Provide a single name=value pair. Argument can be repeated multiple times.

--asynchronous
:   Execute spark submit and return immediately without waiting for an application ID.

--upload
:   If set, all file related arguments will be treated as local file system references and will be uploaded to HDFS. Once uploaded, Spark submit will refer to the files in their HDFS locations. If this is set, any URI references such as `<scheme>://<host:port>/<path>` will be treated as an error.

**Examples**:

To submit a Spark job-app on the cluster:

In this example `/user/clsadmin/jobs/spark-examples_2.11-2.1.0.jar` is a file on HDFS. The example shows submitting a Spark job if your {{site.data.keyword.Bluemix_short}} hosting location is `us-south`.

```
$ ibmcloud ae spark-submit --className org.apache.spark.examples.SparkPi /user/clsadmin/jobs/spark-examples_2.11-2.1.0.jar
User (clsadmin)>
Password>
Contacting endpoint 'https://chs-xxx-xxx-mn001.us-south.ae.appdomain.cloud:8443'...
Job ID '4'
Waiting for job to return application ID. Will check every 10 seconds, and stop checking after 2 minutes. Press Control C to stop waiting.
Finished contacting endpoint 'https://chs-xxx-xxx-mn001.us-south.ae.appdomain.cloud:8443'
OK
Job ID '4'
Application ID 'application_1491850285904_0005'
Done
```

If the HDFS `host` and `port` are known, the HDFS file scheme can be used in the `FILE` argument. The following example uses the {{site.data.keyword.Bluemix_short}} hosting location `us-south`.

```
$ ibmcloud ae spark-submit --className org.apache.spark.examples.SparkPi hdfs://chs-xxx-xxx-mn001.us-south.ae.appdomain.cloud:8020/user/clsadmin/jobs/spark-examples_2.11-2.1.0.jar
```

To submit a local Spark job or app (not on the cluster):

The Spark job `spark-examples_2.11-2.1.0.jar` is located on the user's local system remote from the cluster. By using the `--upload` flag the Spark job is copied to a unique directory in the user's HDFS home directory. The example uses the {{site.data.keyword.Bluemix_short}} hosting location `us-south`.

```
$ ibmcloud ae spark-submit --className org.apache.spark.examples.SparkPi --upload spark-examples_2.11-2.1.0.jar
User (clsadmin)>
Password>
Contacting endpoint 'https://chs-xxx-xxx-mn001.us-south.ae.appdomain.cloud:8443'...
Job ID '2'
Waiting for job to return application ID. Will check every 10 seconds, and stop checking after 2 minutes. Press Control C to stop waiting.
If you would like to repeat this request without re-uploading your files again, please use:

ibmcloud ae spark-submit --user clsadmin --password <YOUR PASSWORD HERE> --className org.apache.spark.examples.SparkPi  hdfs://chs-xxx-xxx-mn001.us-south.ae.appdomain.cloud:8020/user/clsadmin/cli/0fdd1caf-a21f-4a18-970c-e40255e8f0ad/spark-examples_2.11-2.1.0.jar

Finished contacting endpoint 'https://chs-xxx-xxx-mn001.us-south.ae.appdomain.cloud:8443'
OK
Job ID '2'
Application ID 'application_1491850285904_0003'
Done
```

In this example, the Spark job is copied to `hdfs://chs-xxx-xxx-mn001.us-south.ae.appdomain.cloud:8020/user/clsadmin/cli/0fdd1caf-a21f-4a18-970c-e40255e8f0ad/spark-examples_2.11-2.1.0.jar`. This location can be used to run the job again without having to uploading it again. The command output contains this information.

To submit a job without waiting for the application ID:

By default, the `spark-submit` command waits (polls) for status information to get the YARN application ID before returning.  If you want to submit a job and not wait for the application ID, you can provide the `--asynchronous` option. The following example uses the {{site.data.keyword.Bluemix_short}} hosting location `us-south`.

```
$ ibmcloud ae spark-submit --asynchronous --upload pi.py
User (clsadmin)>
Password>
Contacting endpoint 'https://chs-xxx-xxx-mn001.us-south.ae.appdomain.cloud:8443'...
If you would like to repeat this request without re-uploading your files again, please use:

ibmcloud ae spark-submit --user clsadmin --password <YOUR PASSWORD HERE>  --asynchronous hdfs://chs-xxx-xxx-mn001.us-south.ae.appdomain.cloud:8020/user/clsadmin/cli/02eb0c74-2c31-41ad-ae63-4d09fa8096a1/pi.py

Finished contacting endpoint 'https://chs-xxx-xxx-mn001.us-south.ae.appdomain.cloud:8443'
OK
Job ID '8'
Application ID ''
```
