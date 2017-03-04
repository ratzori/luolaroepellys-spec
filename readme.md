#Luolaroepellys spec 0.0.1

## Join game ##

### Client->server request

```javascript
{
  "type": "request",
  "request": {
    "type": "join",
    "nick": "bot1234",      // Nick to use requested by bot
    "password": "p4ssw0rd"  // Password to use requested by bot
  }
}
```
### Server->client response
```javascript
{
  "type": "response",
  "response": {
    "type": "join",
    "status": "string",  // OK, Forbidden
    "error": "string",   // Filled when status != OK
    "id": 123456789      // Unique ID assigend for the bot given by server ( uint64 )
  }
}
```
## Quit game ##

### Client->server request

```javascript
{
  "type": "request",
  "request": {
    "type": "quit",
    "nick": "bot1234",
    "password": "p4ssw0rd",
    "id": 123456789          // Bot ID
  }
}
```
### Server->client response
```javascript
{
  "type": "response",
  "response": {
    "type": "quit",
    "status": "string",  // OK, Forbidden
    "error": "string",   // Filled when status != OK
    "id": 123456789      // Bot ID
  }
}
```
## Field of view request
### Client->server request
```javascript
{
  "type": "request",
  "from": {
    "nick": "string",
    "password": "string",
    "id": 123456789        // Bot ID
  },
  "request": {
    "type": "fow"
  }
}
```
### Server->client response
#### Server indicates what kind of shape to use for FOW
```javascript
{
  "type": "response",
  "to": {
    "id": 123456789
  },
  "response": {
    "type": "fow",
    "height": 9,      // odd
    "width": 9,       // odd
    "sight_pattern": "SSSXXXSSSSSXXXXXSSSXXXXXXXSXXXXXXXXXXXXX@XXXXXXXXXXXXXSXXXXXXXSSSXXXXXSSSSSXXXSSS"
  }
}
```
```
SSSXXXSSSSSXXXXXSSSXXXXXXXSXXXXXXXXXXXXX@XXXXXXXXXXXXXSXXXXXXXSSSXXXXXSSSSSXXXSSS equals to:
S = shadow, X = seen area, @ = bot position

SSSXXXSSS
SSXXXXXSS
SXXXXXXXS
XXXXXXXXX
XXXX@XXXX
XXXXXXXXX
SXXXXXXXS
SSXXXXXSS
SSSXXXSSS
```
