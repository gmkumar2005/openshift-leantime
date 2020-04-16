# Leantime openshift template 

Leantime is an open source project management system for small teams and startups written in PHP, Javascript using MySQL. [https://leantime.io](https://leantime.io)

This is the openshift deployment template and scripts for Leantime </a>. It was built using the <a href="https://github.com/Leantime/leantime/releases">latest Leantime release</a>.

Also provides quick and easy upgrade Leantime to newer release when ever it is avaialble. 

## How to use this template

To run this template you will need an existing project on openshift. 

### Prepare your project 

```
oc login https://console.myopenshifturl.com

oc new-project leantimedev --description="Leantime dev open source project management system"   --display-name="Leantime-dev"
oc policy add-role-to-user edit system:serviceaccount:leantimedev:default   -n leantimedev
oc project leantimedev
```

Checkout this project 

```
git clone https://github.com/parkarteam/openshift-leantime.git

```

## Create app with the template

```
cd openshift-leantime 
oc new-app  -f openshift-templates/leantime-template-dev.yaml -n leantimedev

```
Allow for 15 to 20 mins for the application to be ready. 
The template create an application which can be access at 
https://leantime-leantimedev.apps.myopenshifturl.com

## Following Objects are created after sucessful deployment  
### Images
1. phpbase:7.3-fpm-alpine 
2. leantimedev-basebuild:latest
3. leantimedev-build:latest
4. leantimedev-mysql
5. leantime

### Builds
1. leantimedev-basebuild
2. leantimedev-build
### Secrets
1. leantimedev-secret

### Deployments
1. leantime
2. leantimedev-mysql

### Services
1. leantime
2. leantimedev-mysql

### Routes
1. leantime

## Upgrading to newer version 
The default version of Leantime is Leantime-V2.0.15. 
When a new release is available the build can be changed to match the new version. 
For example if you need to use Leantime 2.1 Beta , run the below command

```
oc set env bc/leantimedev-build \ 
DATABASE_NAME=https://github.com/Leantime/leantime/archive/v2.1-beta6.tar.gz -n leantimedev

```

## Customizing the template 
To use custom project names 

```
oc new-app  -f openshift-template/leantime-template-dev.yaml --param NAMESPACE=customproject
```

Other parameters 
1. NAMESPACE - The OpenShift Namespace where the ImageStream resides (default value leantimedev).
2. NAME -The name assigned to all of the frontend objects defined in this template (default value leantime).
3. PHP_VERSION - Version of PHP image to be used (7.2-fpm-alpine or latest). 
4. MEMORY_LIMIT - Maximum amount of memory the LEANTIME container can use (default 512Mi).
5. MEMORY_MYSQL_LIMIT - Maximum amount of memory the MySQL container can use (default 512Mi).
6. SOURCE_REPOSITORY_URL - The URL of the repository with your application source code or a fork of this project. 
7. SOURCE_REPOSITORY_REF - Set this to a branch name, tag or other ref of your repository if you are not using the default branch.
8. CONTEXT_DIR - Set this to the relative path to your project if it is not in the root of your repository.
9. DATABASE_SERVICE_NAME - Database Service Name (default value leantimedev-mysql)
10. DATABASE_NAME - Database Name (default value leantimedevdb)
11. DATABASE_USER - Databse user (default value leantimedevdbuser)
12. DATABASE_PASSWORD - Random Password is generated. 
13. LEANTIME_RELEASE_URL - Leantime release url (default version is Leantime-V2.0.15). This can be changed after deployment. 

## Debug
This template creates a pod for leantime-installer. Locate the actual pod name by runnin oc get pods.
View the detailed logs ''' oc get logs leantime-installer-sgdxm ```


