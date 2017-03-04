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
    "status": "string",  // ok, forbidden, bad request ...
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
  "from": {
    "nick": "string",
    "password": "string",
    "id": 123456789
  },
  "request": {
    "type": "quit",
  }
}
```
### Server->client response
```javascript
{
  "type": "response",
  "to": {
    "id": 123456789
  },
  "response": {
    "type": "quit",
    "status": "string",
    "error": "string"
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
Fow data contains the full view around the bot

```javascript
{
  "type": "response",
  "to": {
    "id": 123456789
  },
  "response": {
    "type": "fow",
    "sight_data": {
      "type": "full",  // Bot in the middle, width and height odd
      "orientation": "string", // Bot orientation: north, east, south, west, north-east ...
      "width": 9,
      "height": 9,
      "pattern": "SSSXXXSSSSSXXXXXSSSXXXXXXXSXXXXXXXXXXXXX@XXXXXXXXXXXXXSXXXXXXXSSSXXXXXSSSSSXXXSSS"
    }
  }
}
```
```
Type "full" = bot in the middle, width and height are odd numbers. I.e. the full area that could be visible around the bot. Used only once after the join request.

Orientation: north points to up in screen ( initial orientation )

Pattern:
SSSXXXSSSSSXXXXXSSSXXXXXXXSXXXXXXXXXXXXX@XXXXXXXXXXXXXSXXXXXXXSSSXXXXXSSSSSXXXSSS equals to:
S = shadow, X = seen area, @ = bot position

  012345678
0 SSSXXXSSS
1 SSXXXXXSS
2 SXXXXXXXS
3 XXXXXXXXX
4 XXXX@XXXX
5 XXXXXXXXX
6 SXXXXXXXS
7 SSXXXXXSS
8 SSSXXXSSS
```

## Bot status request
### Client->server request
```javascript
{
  "type": "request",
  "from": {
    "nick": "string",
    "password": "string",
    "id": 123456789
  },
  "request": {
    "type": "status"
  }
}
```
### Server->client response
#### Server indicates the current status of bot
```javascript
{
  "type": "response",
  "to": {
    "id": 123456789
  },
  "response": {
    "type": "status",
    "sight_data": {
      "orientation": "string",
      "type": "partial"         // Partial pattern
      "width": 123456789,
      "height": 123456789,
      "pattern": "string"
    }
    "character": {
      "status": "string",   // alive, unconscious, dead
      "health": 69,         // 0 - 100, 0 = dead ( uint8 )
      "inventory": {
        "item": {
          "type": "string", // weapon, key ...
          "id": 123456789   // Item ID
        },
        "item": {
          "type": "string",
          "id": 123456789
        },
        "item": {
          "type": "string",
          "id": 123456789
        }
      }
    }
  }
}
```

Partial pattern:
 - Pattern must fit inside the full pattern
 
Pattern is decoded using the current orientation info.
```
north, @ is in the bottom-center
nort-east @ is in the bottom-left corner
east @ is in the middle-left
and so on...
```

Example:
```
orientation: north-east
height: 5
width: 5 ( compare to full table )
pattern: SSSSSS.##SS..#SS...S@SSSS

  45678
0 SSSSS
1 S.##S
2 S..#S
3 S...S
4 @SSSS
```
