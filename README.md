
# Jenkins Remote Access API

The same Projects for WebSphere (deploy, restart, etc.) available in the Jenkins GUI can also be invoked via HTTP Post using a REST-style API.

## For the impatient

The command below deploys WebSphere app 'MyWebApp' in folder cm12345 to the WAS855-01UTCell:

```
curl -X POST -k -u uid:yourJenkinsAPIToken
"https://deploy.example.com/
job/appDeploy/buildWithParameters?
CM_DIR=cm12345&APP_NAME=MyWebApp&token=01utcell"
```
*You must enter this command on a single line. It is split onto different lines here for clarity.*

## Detailed API usage

*These examples use the [cURL](https://curl.haxx.se/docs/manpage.html) command to demonstrate use of the Jenkins API. If not using cURL, check the documentation for your client / language on how to submit HTTP POST requests that require authentication, similar to what's described here.*

### Making a HTTP POST request with authentication

```
curl -X POST -k
```

The -X POST option ensures the request is send as an HTTP POST (not a GET).

The -k option for curl connects to the API endpoint without confirming SSL certificate validity. If you want to confirm the validity of the certificate, visit deploy.example.com in a browser and save the certificate from there, then use per your scripting client's SSL documentation.


Supply your userID (legacy hardware or legacy example), followed by a colon, followed by the [Jenkins API token associated with your userID](https://stackoverflow.com/questions/45466090/how-to-get-the-api-token-for-jenkins):

```
-u uid:yourJenkinsAPIToken
```

*Protect your Jenkins API token as you would your password.*


*Using REST commands for a Jenkins Project requires the same authentication/authorization as running the Project in the Jenkins GUI.*

### Specifying the endpoint to handle the POST request

```
"https://deploy.example.com/
```


`https://deploy.example.com` is the on-prem proxy endpoint that accepts the HTTP POST request. The proxy uses information in the request string (covered below) to route the request to the appropriate WebSphere cell.

** The double-quote before the URL is required, and is closed at the end of the URL *(below)* **

### Specifying the API parameters

```
job/appDeploy/buildWithParameters?
```

Jenkins API URI syntax for performing a build is `/job/JOBNAME/build`

`/job/JOBNAME/buildWithParameters` if the job takes parameters

See [Jenkins Remote Access API Wiki](https://wiki.jenkins.io/display/JENKINS/Remote+access+API) for more information, and examples of using JSON for parameters instead of name/value pairs

`CM_DIR=cm12345&APP_NAME=MyWebApp&token=wasv855-01utcell"`

These are the name/value parameters required by the JOBNAME

The names to use can be viewed in the Jenkins GUI by clicking Build With Parameters within each JOBNAME (appDeploy, exampleappsDeploy, clusterStopStart, etc).

The values to use are self-explanatory (CM folder, app name, cluster name, etc.) specific to what you are deploying.

`token=`*`cellName`* is required so the request gets routed to the correct WebSphere cell. *The cell names to use as token values can be seen in the Jenkins GUI in the description for each JOBNAME*.

** The double-quote at the end of the URL is required to enclose the URL. **
