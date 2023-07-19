# Mule Training - Error Handling Flow Level


## Initial Configuration
```
curl "localhost:8081/flights?code=PDX" -v
```

HTTP Response is
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
< Date: Wed, 19 Jul 2023 14:10:00 GMT
< Connection: close
<
{
  "message": "Exception(s) were found for route(s): \n\tRoute 0: org.mule.extension.http.api.request.validator.ResponseValidatorTypedException: HTTP GET on resource 'http://training4-american-api.cloudhub.io:80/flights' failed: bad request (400).\n\tRoute 2: org.mule.runtime.api.connection.ConnectionException: Error trying to acquire a new connection:org.mule.wsdl.parser.exception.WsdlGettingException: Error Getting the resource [http://localhost:9191/deltas?wsdl]: http://localhost:9191/deltas?wsdl"
}
```


There are multiple errors thrown within the code as shown below, but when the global error handler finally catches all the errors, it has a `MULE:COMPOSITE_ROUTING`
`errorType`.  

`error.description` is 
```
"Exception(s) were found for route(s): 
	Route 0: org.mule.extension.http.api.request.validator.ResponseValidatorTypedException: HTTP GET on resource 'http://training4-american-api.cloudhub.io:80/flights' failed: bad request (400).
	Route 2: org.mule.runtime.api.connection.ConnectionException: Error trying to acquire a new connection:org.mule.wsdl.parser.exception.WsdlGettingException: Error Getting the resource [http://localhost:9191/deltas?wsdl]: http://localhost:9191/deltas?wsdl"
	```


## Step 2 - Added Try Scope
Added Try Scope around all Flow References in the getAllAirlineFlights data AND added a On Error Continue handler with nothing included.

```
curl "localhost:8081/flights?code=PDX" -v
```

```
*   Trying 127.0.0.1:8081...
* Connected to localhost (127.0.0.1) port 8081 (#0)
> GET /flights?code=PDX HTTP/1.1
> Host: localhost:8081
> User-Agent: curl/8.1.2
> Accept: */*
>
< HTTP/1.1 200 OK
< Content-Type: application/json; charset=UTF-8
< Content-Length: 1071
< Date: Wed, 19 Jul 2023 14:21:01 GMT
<
[
  {
    "message": "HTTP GET on resource 'http://training4-american-api.cloudhub.io:80/flights' failed: bad request (400)."
  },
  {
    "flightCode": "ER49fd",
    "availableSeats": 0,
    "destination": "PDX",
    "planeType": "Boeing 777",
    "origination": "MUA",
    "price": 853.0,
    "departureDate": "2018/02/13",
    "airlineName": "United"
  },
  {
    "flightCode": "ER95jf",
    "availableSeats": 95,
    "destination": "PDX",
    "planeType": "Boeing 777",
    "origination": "MUA",
    "price": 483.0,
    "departureDate": "2018/02/20",
    "airlineName": "United"
  },
  {
    "flightCode": "ER04kf",
    "availableSeats": 30,
    "destination": "PDX",
    "planeType": "Boeing 777",
    "origination": "MUA",
    "price": 532.0,
    "departureDate": "2018/02/12",
    "airlineName": "United"
  },
  {
    "message": "Data unavailable. Try later sucker! Error trying to acquire a new connection:org.mule.wsdl.parser.exception.WsdlGettingException: Error Getting the resource [http://localhost:9191/deltas?wsdl]: http://localhost:9191/deltas?wsdl"
  }
* Connection #0 to host localhost left intact
]%
```

## Step 3 - Added Transform Message to Try Scope
Added Transform Message to Try Scope to send an empty array instead of the error message.

```
curl "localhost:8081/flights?code=PDX" -v
```

```
*   Trying 127.0.0.1:8081...
* Connected to localhost (127.0.0.1) port 8081 (#0)
> GET /flights?code=PDX HTTP/1.1
> Host: localhost:8081
> User-Agent: curl/8.1.2
> Accept: */*
>
< HTTP/1.1 200 OK
< Content-Type: application/json; charset=UTF-8
< Content-Length: 688
< Date: Wed, 19 Jul 2023 14:28:53 GMT
<
[
  {
    "flightCode": "ER49fd",
    "availableSeats": 0,
    "destination": "PDX",
    "planeType": "Boeing 777",
    "price": 853.0,
    "origination": "MUA",
    "departureDate": "2018/02/13",
    "airlineName": "United"
  },
  {
    "flightCode": "ER95jf",
    "availableSeats": 95,
    "destination": "PDX",
    "planeType": "Boeing 777",
    "price": 483.0,
    "origination": "MUA",
    "departureDate": "2018/02/20",
    "airlineName": "United"
  },
  {
    "flightCode": "ER04kf",
    "availableSeats": 30,
    "destination": "PDX",
    "planeType": "Boeing 777",
    "price": 532.0,
    "origination": "MUA",
    "departureDate": "2018/02/12",
    "airlineName": "United"
  }
* Connection #0 to host localhost left intact
]%
```


## mt_44ErrorHandlingFlowLevel Docs and Notes
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