
# Waffles and syrup

The same Projects for Waffles (deploy, restart, etc.) available in the Syrup GUI can also be invoked via HTTP POST using a REST-style API.

*These examples use the [cURL](https://curl.haxx.se/docs/manpage.html) command to demonstrate use of the Waffles Deploy API. If not using cURL, check the documentation for your client / language on how to submit HTTP POST requests that require authentication*

## API Quick Start

The command below deploys Waffles app 'MyWebApp' in folder cm12345 to the BreakfastCell:

```
curl -X POST -k -u uid:yourSyrupAPIToken
"https://deploy.example.com/
job/appDeploy/buildWithParameters?
CM_DIR=cm12345&APP_NAME=MyWebApp&token=breakfastcell"
```
*You must enter this command on a single line. It is split onto different lines here for clarity.*

## Detailed API usage



### Make a HTTP POST request

```
curl -X POST -k
```

`-X POST` ensures the request is send as an HTTP POST (not a GET).

`-k` connects to the API endpoint without confirming SSL certificate validity of the endpoint. If you want to confirm the validity of the certificate, visit deploy.example.com in a browser and save the certificate from there, then use per your scripting client's SSL documentation.

### Authenticating the HTTP POST request

Supply your userID (legacy hardware or legacy example), followed by a colon, followed by the Syrup API token associated with your userID:

```
-u uid:yourSyrupAPIToken
```

*Protect your Syrup API token as you would your password.*


*Using REST commands for a Syrup Project requires the same authentication/authorization as running the Project in the Syrup GUI.*

### Specifying the endpoint to handle the POST request

```
"https://deploy.example.com/
```


`https://deploy.example.com` is the on-prem proxy endpoint that accepts the HTTP POST request. The proxy uses information in the API parameters *(covered below)* to route the request to the appropriate Waffles cell.

**The double-quote before the URL is required, and is closed at the end of the URL (below)**

### Specifying the REST-style URI

```
job/appDeploy/buildWithParameters?
```

Syrup syntax for performing a build via the REST API is `/job/JOBNAME/build?`

where JOBNAME is the name of the job as shows in the Syrup GUI

`/job/JOBNAME/buildWithParameters?` if the job takes parameters


### Specifying the API parameters

```
CM_DIR=cm12345&APP_NAME=MyWebApp&token=breakfastcell"
```

These are the name/value parameters required by the JOBNAME

The *names* required by a particular JOBNAME can be viewed in the Syrup GUI by clicking "Build With Parameters" within each JOBNAME (appDeploy, exampleappsDeploy, clusterStopStart, etc).

The *values* to use are specific to what you are deploying (CM folder, app name, cluster name, etc.)

`token=`*`cellName`* is required by Syrup for using the API, and so the request gets routed to the correct Waffles cell. The cell names to use as token values can be seen in the Syrup GUI in the description for each JOBNAME.

**The double-quote at the end of the URL is required to enclose the URL.**
