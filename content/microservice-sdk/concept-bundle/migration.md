---
weight: 50
title: Microservice migration to API Version 2
layout: redirect
---

**With release 10.14**, {{< company-sag >}} announces the availability of microservice API Version 2 and the deprecation of API Version 1 to comply with new security requirements.  
Microservice API Version 2 provides an improved microservice container security context restricting the invocation of privileged Linux Kernel APIs.
In details this means that with microservice API Version 2 the microservice container is granted only specific capabilities.

Refer to the [Linux manual page](https://man7.org/linux/man-pages/man7/capabilities.7.html) for more information on Kernel capabilities.
With the API Version change, the microservice is granted the capability NET_BIND_SERVICE.

#### Microservice migration from release 10.15

Perform the following steps to migrate your microservice to the new API Version.

1. Change the API Version to 2 in the microservice manifest. See [Microservice manifest](#manifest).   

2. Deploy your microservice to the test environment.

3. Test the functionality of your microservice and analyze possible errors.

In the simplest case it is sufficient to set the API Version to 2 in your microservice manifest.  

However, for microservices which currently make use of specific privileges of the Linux Kernel API that are not granted anymore, you additionally need to refactor the source code so that the service doesn't require the invocation of these privileges.  

#### Affected microservices

Set the API Version field in the microservice manifest to 2 and deploy this service to your {{< company-c8y >}} test environment.
This environment must be in version 10.14 or higher.
Verify that the functionality provided by the miroservice still works as expected.

#### Microservices still using old user privileges after the environment upgrade

If your microservice is using the deprecated API Version 1 and
is deployed to a {{< company-c8y >}} environment in version 10.15 or higher, it may be affected by the change, depending on the configuration of this environment.
In this case, you will not be able to upload or subscribe the microservice.