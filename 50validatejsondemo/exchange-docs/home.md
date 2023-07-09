# JSON Schema Validator

Picked the schema from this site.
https://json-schema.org/learn/miscellaneous-examples.html

Sample Schema
```json
{
  "$id": "https://example.com/person.schema.json",
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Person",
  "type": "object",
  "properties": {
    "firstName": {
      "type": "string",
      "description": "The person's first name."
    },
    "lastName": {
      "type": "string",
      "description": "The person's last name."
    },
    "age": {
      "description": "Age in years which must be equal to or greater than zero.",
      "type": "integer",
      "minimum": 0
    }
  }
}
```

Sample Payload
```json
{
  "firstName": "John",
  "lastName": "Doe",
  "age": 21
}
```

```shell
curl -X POST localhost:8081/jsonvalidate -d '{
  "firstName": "John",
  "lastName": "Doe",
  "age": 21
}'
```

```shell
curl -X POST localhost:8081/jsonvalidate -d '{
  "firstName": "John",
  "lastName": "Doe",
  "age": -1
}'
```