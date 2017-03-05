#Luolaroepellys server-client API ver. 0.0.1

## Ping ##

### Server->client request

```javascript
{
  "type": "req_ping",
  "req_ping": {
    "id": 123456789        // Bot ID
  }
}
```
### Client->server response
```javascript
{
  "type": "resp_ping",
  "resp_ping": {
    "from": {
      "id": 123456789,      // Bot ID
      "nick": "string",
      "password": "string"
    }
  }
}
```
