# Parse Template uses a HTML template below.

```
<html>
<body>
<table>
<tr>
<th>Employee Name</th>
<th>Employee ID</th>
<th>Employee EmailID</th>
</tr>
<tr>
<td>#[payload.empName]</td>
<td>#[payload.empID]</td>
<td>#[payload.empEmailID]</td>
</tr>
</table>
</body>
</html>
```

curl -X POST -H "Content-Type: application/json" localhost:8081/employees -d '{"empName": "Ack", "empID":"3333", "empEmailID": "ack@email.com"}'