# PHP source to image Helloworld Example

This is an example php application, which can be deployed to OpenShift using the following commands

## How to deploy

### Webconsole

* log into your OpenShift V3 Master (eg. oc login https://atomic1.example.com)
* Create a new Project
* "Add to Project" a php:5.6 application
* name the application for example php-example and provide the git repository URL, in this example https://github.com/gatesch/example-php-sti-helloworld.git
* the build and deployment is automatically triggered and the example application will be deployed soon

### CLI / oc Client

#### Create New OpenShift Project
```
$ oc new-project example-php-sti-helloworld
```

#### Create Application and expose Service
```
$ oc new-app https://github.com/gatesch/example-php-sti-helloworld.git --name=php-example

$ oc expose service php-example --hostname=php.example.com
```

## Add Webhook to trigger rebuilds

Take the Webhook GitHub URL from

```
$ oc describe bc php-example

oc describe bc php-example
Name:			php-example
Created:		20 seconds ago
Labels:			app=php-example
Annotations:		openshift.io/generated-by=OpenShiftNewApp
Latest Version:		1
Strategy:		Source
Source Type:		Git
URL:			https://github.com/gatesch/example-php-sti-helloworld.git
From Image:		ImageStreamTag openshift/php:latest
Output to:		ImageStreamTag gatesch-php-sti-example:latest
Triggered by:		Config, ImageChange
Webhook GitHub:		https://[Server]/oapi/v1/namespaces/example-php-sti-helloworld/buildconfigs/php-example/webhooks/[GitHubsecret]/github
Webhook Generic:	https://[Server]/oapi/v1/namespaces/example-php-sti-helloworld/buildconfigs/php-example/webhooks/[genericsecret]/generic
```

and add the URL as a Webhook in your github Repository, read https://developer.github.com/webhooks/ for more details about github Webhooks

--- To remove from the project ---

$ oc delete all --selector app=php-example
