# Onpremise Cluster Group

This project shows how to run Mulesoft on a local machine (VM) and then deploy proxies to the VM via API Manager.

1. Export the Application as `Anypoint Studio Project to Mule Deployable Archive`.
 
2. Install Mulesoft EE on the local VM. Mine is installed here.

`/Users/USERNAME/apps/mule-enterprise-standalone-4.4.0-20230616/bin`

3. Set the JAVA_HOME to Java 8. 
`export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk8u372-b07/Contents/Home`

4. You will also need to configure AMC, which links the local machine to API Manager. Follow the links here which shows you how to create the AMC command.
You have to do this twice to setup 2 servers on your local machine.

https://docs.mulesoft.com/runtime-manager/installing-and-configuring-runtime-manager-agent#register-mule-with-the-cloud-based-runtime-manager

`./amc_setup -H TOKEN NODENAME1`

`./amc_setup -H TOKEN NODENAME2` 
 
5. Start Mulesoft local server.  Start mulesoft twice; the second instance listening on a different port.
```
cd /Users/USERNAME/apps/mule-enterprise-standalone-4.4.0-20230616/bin

./mule -M-Dhttp.port=8085

```

6. Open a new terminal and run the following.
```
cd /Users/USERNAME/apps/mule-enterprise-standalone-4.4.0-20230616instance2/bin

./mule -M-Dhttp.port=8086

```

7. Both of your servers statuses should show connected in Anypoint Runtime Manager.

8. Create a server group in Anypoint Platform Runtime Manager and add both servers to the group.
https://docs.mulesoft.com/runtime-manager/server-group-create

9. Deploy the JAR to the Group.
https://docs.mulesoft.com/runtime-manager/deploying-to-your-own-servers#deploy-an-application-from-runtime-manager 

10. Send the request to your API. 


Test the API. The application is listening on port 8082.
```
curl localhost:8082/onpremdemocluster

```

Response
```
{
  "test": "test"
}
```


11. Now delete application and the group in Anypoint Runtime Manager.

12. Create a new cluster and add the two servers.

13. test the API again.

Test the API. The application is listening on port 8082.
```
curl localhost:8082/onpremdemocluster

```

Response
```
{
  "test": "test"
}
```