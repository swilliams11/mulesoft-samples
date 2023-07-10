# My Hello World API

I performed the following steps:
1. Created a RAML file in Anypoint Studio Design Center

2. I imported the RAML API into this project

3. I created a new API Manager in API Studio for this API.

4. You must make your API available in [API Manager](https://docs.mulesoft.com/mule-gateway/mule-gateway-config-autodiscovery-mule4)
 
5. Then you must add the API Manager Credentials according to the article below.
https://mikeconroy.com/2020/04/resolving-api-key-blocked-error/

You can get these credentials from the Anypoint Platform -> Access Management -> Business Groups section 

6. Once you enter the Client ID and Secret into Anypoint Studio -> Settings -> API Manager. Execute the curl command below 4 times and you will get the quota error.
```
 curl localhost:8081/api/hello                                           
```

Quota error.
```
{
  "error": "Quota has been exceeded"
}%
```

