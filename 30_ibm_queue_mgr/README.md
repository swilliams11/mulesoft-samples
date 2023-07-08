# Mulesoft IBM MQ demo

1. Create a free IBM Cloud Account and create a free IBM MQ message broker.

2. Create several Secure Properties Config files

3. Use AES encryption instead of Blowfish.  AES uses 256 bit encryption and Blowfish is capped at 64bits.

You can use Python to generate the key. 
```shell
pip3 install pycryptodome 
```

```python
from Crypto.Random import get_random_bytes
key = get_random_bytes(16)
print(key.hex())
```

```
java -cp secure-properties-tool.jar com.mulesoft.tools.SecurePropertiesTool string encrypt AES CBC Secure16BitRandomKey "VALUE"
```

4. Test it
```shell
curl localhost:8081/ibmqueue
```

You must add the correct Key to Global Secure Config.

## Errors
This project cannot connect to IBM MQ.  It keeps throwing an connection error.  
