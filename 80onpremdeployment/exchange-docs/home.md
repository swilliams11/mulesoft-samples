# Onpremise Deployment

This project shows how to run Mulesoft on a local machine (VM) and then deploy proxies to the VM via API Manager.

1. Install Mulesoft EE on the local VM. Mine is installed here.

`/Users/USERNAME/apps/mule-enterprise-standalone-4.4.0-20230616/bin`

2. Set the JAVA_HOME to Java 8. 
`JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk8u372-b07/Contents/Home`

3. You will also need to configure AMC, which links the local machine to API Manager. Follow the links here which shows you how to create the AMC command.
https://docs.mulesoft.com/runtime-manager/installing-and-configuring-runtime-manager-agent#register-mule-with-the-cloud-based-runtime-manager

`./amc_setup -H TOKEN NODENAME` 
 
4. Start Mulesoft local server
```
cd /Users/USERNAME/apps/mule-enterprise-standalone-4.4.0-20230616/bin

./mule
```

5. Your server status should show connected in Anypoint Runtime Manager.

6. Send the request to your API. 


Test the API.
```
curl localhost:8081/onpremdemo
```

Response
```
{
  "test": "test"
}
```