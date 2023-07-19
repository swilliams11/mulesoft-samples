# Mule Training - Error Handling System Errors

#### Step 1



## Prior Labs

### mt_48...

#### Step 1
This request go all the way through the private flows and finally throws and error. 
```
curl "localhost:8081/api/flights?airline=american&code=PDX" -v
```

```
*   Trying 127.0.0.1:8081...
* Connected to localhost (127.0.0.1) port 8081 (#0)
> GET /api/flights?airline=american&code=PDX HTTP/1.1
> Host: localhost:8081
> User-Agent: curl/8.1.2
> Accept: */*
>
< HTTP/1.1 200 OK
< Content-Type: application/json; charset=UTF-8
< Content-Length: 121
< Date: Wed, 19 Jul 2023 17:06:13 GMT
<
{
  "message": "HTTP GET on resource 'http://training4-american-api.cloudhub.io:80/flights' failed: bad request (400)."
* Connection #0 to host localhost left intact

```

This request throws a bad request right away in the APIKit Router, because the RAML file defines the valid enums for the `code` query parameter. 
```
curl "localhost:8081/api/flights?airline=american&code=FOO" -v
```

```
*   Trying 127.0.0.1:8081...
* Connected to localhost (127.0.0.1) port 8081 (#0)
> GET /api/flights?airline=american&code=FOO HTTP/1.1
> Host: localhost:8081
> User-Agent: curl/8.1.2
> Accept: */*
>
< HTTP/1.1 400 Bad Request
< Content-Type: application/json; charset=UTF-8
< Content-Length: 30
< Date: Wed, 19 Jul 2023 17:09:41 GMT
< Connection: close
<
{
  "message": "Bad request"
* Closing connection 0
}%
```


This returns invalid destination because of the Is True Validation.  The Global Error Handler On Error Continue with the custom error mapping executes.  and returns the
error below.  Why doesn't the rest of the Processors execute if On Error Continue is present???
```
curl "localhost:8081/api/flights" -v
```

```
*   Trying 127.0.0.1:8081...
* Connected to localhost (127.0.0.1) port 8081 (#0)
> GET /api/flights HTTP/1.1
> Host: localhost:8081
> User-Agent: curl/8.1.2
> Accept: */*
>
< HTTP/1.1 400 Bad Request
< Content-Type: application/json; charset=UTF-8
< Content-Length: 40
< Date: Wed, 19 Jul 2023 17:13:37 GMT
< Connection: close
<
{
  "message": "Invalid destination  "
* Closing connection 0
}%
```

#### Step 2
* Changed the error handler in getAmericanFlights flow to On Error Continue
* Changed the Global Error Handler On Error Propagate to On Error Continue. 


```
curl "localhost:8081/api/flights?airline=american&code=PDX" -v
```

```
*   Trying 127.0.0.1:8081...
* Connected to localhost (127.0.0.1) port 8081 (#0)
> GET /api/flights?airline=american&code=PDX HTTP/1.1
> Host: localhost:8081
> User-Agent: curl/8.1.2
> Accept: */*
>
< HTTP/1.1 200 OK
< Content-Type: application/json; charset=UTF-8
< Content-Length: 36
< Date: Wed, 19 Jul 2023 17:26:13 GMT
<
{
  "message": "No flights to PDX"
* Connection #0 to host localhost left intact
}%
```


```
curl "localhost:8081/api/flights?airline=delta&code=PDX" -v
```

```
*   Trying 127.0.0.1:8081...
* Connected to localhost (127.0.0.1) port 8081 (#0)
> GET /api/flights?airline=delta&code=PDX HTTP/1.1
> Host: localhost:8081
> User-Agent: curl/8.1.2
> Accept: */*
>
< HTTP/1.1 500 Server Error
< Content-Type: application/json; charset=UTF-8
< Content-Length: 246
< Date: Wed, 19 Jul 2023 17:28:05 GMT
< Connection: close
<
{
  "message": "Data unavailable. Try later sucker! Error trying to acquire a new connection:org.mule.wsdl.parser.exception.WsdlGettingException: Error Getting the resource [http://localhost:9191/deltas?wsdl]: http://localhost:9191/deltas?wsdl"
* Closing connection 0
}%
```


## Prior Labs

### mt_47...
#### Step 1
Connected API Kit interface to implementation.   The reason you get an error is because the interface
has a base path of `/api/*`

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
< HTTP/1.1 404 Not Found
< Content-Type: text/plain
< Date: Wed, 19 Jul 2023 15:32:26 GMT
< Transfer-Encoding: chunked
<
* Connection #0 to host localhost left intact
No listener for endpoint: /flights%

```

```
curl "localhost:8081/api/flights?code=PDX" -v
```
```
*   Trying 127.0.0.1:8081...
* Connected to localhost (127.0.0.1) port 8081 (#0)
> GET /api/flights?code=PDX HTTP/1.1
> Host: localhost:8081
> User-Agent: curl/8.1.2
> Accept: */*
>
< HTTP/1.1 200 OK
< Content-Type: application/json; charset=UTF-8
< Content-Length: 688
< Date: Wed, 19 Jul 2023 15:33:49 GMT
<
[
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
  }
* Connection #0 to host localhost left intact
]%
```


## Prior Labs

### mt_46...
#### Step 1 - add a custom error type for Is True Validation

* Added an Error Type Mapping to the Is True Validation
* Updated the On Error Continue Scope to handle the custom Error Type Mapping.

```
curl "localhost:8081/flights?code=FOO" -v
```

```
*   Trying 127.0.0.1:8081...
* Connected to localhost (127.0.0.1) port 8081 (#0)
> GET /flights?code=FOO HTTP/1.1
> Host: localhost:8081
> User-Agent: curl/8.1.2
> Accept: */*
>
< HTTP/1.1 400 Bad Request
< Content-Type: application/json; charset=UTF-8
< Content-Length: 42
< Date: Wed, 19 Jul 2023 15:05:11 GMT
< Connection: close
<
{
  "message": "Invalid destination FOO"
}
```

#### Step 2 - Moved the error handlers around

```
curl "localhost:8081/flights?code=FOO" -v
```

```
*   Trying 127.0.0.1:8081...
* Connected to localhost (127.0.0.1) port 8081 (#0)
> GET /flights?code=FOO HTTP/1.1
> Host: localhost:8081
> User-Agent: curl/8.1.2
> Accept: */*
>
< HTTP/1.1 400 Bad Request
< Content-Type: application/json; charset=UTF-8
< Content-Length: 42
< Date: Wed, 19 Jul 2023 15:12:31 GMT
< Connection: close
<
{
  "message": "Invalid destination FOO"
* Closing connection 0
}%
```


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
< Date: Wed, 19 Jul 2023 15:13:35 GMT
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



### mt_45ErrorHandlingProcessorLevel
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


### mt_44ErrorHandlingFlowLevel Docs and Notes
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