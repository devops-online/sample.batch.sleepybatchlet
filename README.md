#SleepyBatchlet sample for batch-1.0 on Liberty (Bluemix)

SleepyBatchlet is a simple sample batchlet for use with feature batch-1.0 on WebSphere Liberty Profile.
batch-1.0 is Liberty's implementation of the Batch Programming Model in Java EE 7, as specified by JSR-352.

The batchlet itself is rather uninteresting. All it does is sleep in 1 second increments for a default time
of 15 seconds.  The sleep time is configurable via batch property *sleep.time.seconds*.  The batchlet
prints a message to System.out each second, so you can easily verify that it's running.

##Build the sample (TO BE UPDATED TO THE BLUEMIX STEPS)

For your convenience, the sample application has already been built: SleepyBatchletSample-1.0.war.

To build from source, use maven or import the project into WDT (WebSphere Developer Tools).

##Install and run the sample

1. Use the sample server.xml as a guide for configuring your Liberty server with batch-1.0.

2. Install the sample app to your server by copying SleepyBatchletSample-1.0.war into
the server's dropins/ directory.

3. Run the sample. The sample app includes a generic JobOperatorServlet that acts as
thin wrapper around the JobOperator API.  You can start the batch job thru this servlet
by hitting the following URL:

    http://<bluemix_app_url>/joboperator?action=start&jobXMLName=sleepy-batchlet

##Controlling sample jobs

Besides starting a job, you can use the JobOperatorServlet to stop, restart, and get status, by hitting the following URLs:

    http://<bluemix_app_url>/joboperator?action=stop&executionId=xx
    http://<bluemix_app_url>/joboperator?action=restart&executionId=xx
    http://<bluemix_app_url>/joboperator?action=status&executionId=xx

Where *executionId=xx* is the job execution ID.  Each http request returns JobInstance and JobExecution
records for the job.  

For a complete list of actions available from the JobOperatorServlet, use action=help:

    http://<bluemix_app_url>/joboperator?action=help

**Note:** The JobOperatorServlet is a generic HTTP wrapper for the JobOperator API.  You can copy it
into your own application and use it to control the jobs for that application.  


##Example session

Below is an example session, using cURL.  

Note: The query parameter *sleep.time.seconds* is passed along as a job parameter for the job, and then injected into 
the batchlet via a batch property.  The property controls how long SleepyBatchlet runs.

```
$ curl 'http://<bluemix_app_url>/joboperator?action=start&jobXMLName=sleepy-batchlet'
<XML>
  <jobName>sleepy-batchlet</jobName>
  <Parameters>
    <Name>sleep.time.seconds</Name>
    <Value>300</Value>
  </Parameters>
  <Status>Job started</Status>
  <InstanceID>1</InstanceID>
  <ExecutionID>1</ExecutionID>
  <batchStatus>STARTED</batchStatus>
  <startTime>Thu Apr 21 13:52:21 UTC 2016</startTime>
  <endTime>null</endTime>
</XML>

$ curl 'http://<bluemix_app_url>/joboperator?action=status&executionId=1'

$ curl 'http://<bluemix_app_url>/joboperator?action=start&jobXMLName=sleepy-batchlet&jobParameters=sleep.time.seconds=32'

$ curl 'http://<bluemix_app_url>/joboperator?action=status&executionId=2'

$ curl 'http://<bluemix_app_url>/joboperator?action=stop&executionId=2'

$ curl 'http://<bluemix_app_url>/joboperator?action=status&executionId=2'

$ curl 'http://<bluemix_app_url>/joboperator?action=restart&executionId=2&restartParameters=sleep.time.seconds=10'

$ curl 'http://<bluemix_app_url>/joboperator?action=status&executionId=2'

```


