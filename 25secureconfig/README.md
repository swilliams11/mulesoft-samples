# Secure Config


https://docs.mulesoft.com/mule-runtime/4.4/secure-configuration-properties

1. Download the secure properties tool.jar from link above. 
2. Open terminal and `cd` to the directory where this jar is located.
3. `java -cp secure-properties-tool.jar com.mulesoft.tools.SecurePropertiesTool`
```
java -cp secure-properties-tool.jar com.mulesoft.tools.SecurePropertiesTool string encrypt Blowfish CBC MyMuleSoft "password"
```

4. Add the Configuration Property element in Global Elements
5. Add a Secure Configuration property element in Global Elements
* you must search for "secure" in the Search in Exchange component and add the Mule Secure Configuration component.
* go back to global elements and then click "create"
* add Secure Properties Config and configure it.