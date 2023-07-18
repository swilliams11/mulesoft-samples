# Mule Training - Error Handling Flow Level

This calls the getAllAirlineFlights flow in the Choice Router.  
```
curl "localhost:8081/flights?code=PDX" -v
```

This is the response.  The getAllAirlineFlights flow calls the getAmerican and getDelta flows, both of which return
errors. The getAmericanFlights doesn't have a PDX code and the getDelta SOAP service is artificially down (because
we put an invalid URL".  

The Global Error Handler is executed in both cases, which propagates the error up AND
Mulesoft aggregates the error from both errors into one message.     

Response
```
*   Trying 127.0.0.1:8081...
* Connected to localhost (127.0.0.1) port 8081 (#0)
> GET /flights?code=PDX HTTP/1.1
> Host: localhost:8081
> User-Agent: curl/8.1.2
> Accept: */*
>
< HTTP/1.1 500 Server Error
< Content-Type: application/json; charset=UTF-8
< Content-Length: 508
< Date: Tue, 18 Jul 2023 23:29:36 GMT
< Connection: close
<
{
  "message": "Exception(s) were found for route(s): \n\tRoute 0: org.mule.extension.http.api.request.validator.ResponseValidatorTypedException: HTTP GET on resource 'http://training4-american-api.cloudhub.io:80/flights' failed: bad request (400).\n\tRoute 2: org.mule.runtime.api.connection.ConnectionException: Error trying to acquire a new connection:org.mule.wsdl.parser.exception.WsdlGettingException: Error Getting the resource [http://localhost:9191/deltas?wsdl]: http://localhost:9191/deltas?wsdl"
* Closing connection 0
}%
```