#Luolaroepellys client-server API ver. 0.0.1

## Join game ##

### Client->server request

```javascript
{
  "type": "req_join",
  "req_join": {
    "from": {
      "nick": "bot1234",     // Nick to use requested by bot
      "password": "p4ssw0rd" // Password to use requested by bot
    }
  }
}
```
### Server->client response
```javascript
{
  "type": "resp_join",
  "resp_join": {
    "id": 12345678,      // Unique ID assigned for the bot given by server ( uint64 )
    "status": "string",  // ok, forbidden, bad request ...
    "error": "string"    // Filled when status != OK
  }
}
```
## Quit game ##

### Client->server request

```javascript
{
  "type": "req_quit",
  "req_quit": {
    "from": {
      "id": 123456789,     // Bot ID
      "nick": "string",
      "password": "string"
    }
  }
}
```
### Server->client response
```javascript
{
  "type": "resp_quit",
  "resp_quit": {
    "id": 123456789
    "status": "string",
    "error": "string"
  }
}
```
## Field of view request
### Client->server request
```javascript
{
  "type": "req_fow",
  "req_fow": {
    "from": {
      "id": 123456789,
      "nick": "string",
      "password": "string"
    }
  }
}
```
### Server->client response
#### Server indicates what kind of shape to use for FOW
Fow data contains the full view around the bot

```javascript
{
  "type": "resp_fow",
  "resp_fow": {
    "id": 123456789,
    "sight_data": {
      "type": "full",          // Bot in the middle, width and height odd
      "orientation": "north",  // Bot initial orientation: north, east, south, west, north-east ...
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
  "type": "req_status",
  "req_status": {
    "from": {
      "id": 123456789,
      "nick": "string",
      "password": "string"
    }
  }
}
```
### Server->client response
#### Server indicates the current status of bot
```javascript
{
  "type": "resp_status",
  "resp_status": {
    "id": 123456789,
    "sight_data": {
      "type": "partial",        // Partial pattern
      "orientation": "string",
      "width": 123456789,
      "height": 123456789,
      "pattern": "string"
    },
    "character": {
      "status": "string",       // alive, unconscious, dead
      "health": 123456789,      // 0 - 100, 0 = dead ( uint8 )
      "inventory": [
        {
          "type": "weapon",
          "id": 123456789,      // Item ID
          "damage": 123456789,
          "weapon": {
            "type": "melee"
          }
        },
        {
          "type": "key",
          "id": 123456789,
          "key": {
            "type": "string"
          }
        },
        {
          "type": "weapon",
          "id": 123456789,
          "damage": 123456789,
          "weapon": {
            "type": "gun",
            "ammo": 123456789
          }
        }
      ]
    }
  }
}
```

Partial pattern:
 - Partial pattern must fit inside the full pattern
 
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
## Action request ##

### Client->server request

```javascript
{
  "type": "req_action",
  "req_action": {
    "from": {
      "id": 123456789,
      "nick": "string",
      "password": "string"
    },
    "action_type": "string",    // rotate, interact, pick...
    "move": {
      "direction": "string"
    },
    "rotate": {                 //  available when action_type == rotate
      "orientation": "string"
    },
    "interact": {               //  available when action_type == interact
      "target_type": "string",  // weapon, key ...
      "target_id": 123456789    // ID of the target to be interact with
    },
    "pick": {                   //  ...
      "target_type": "string",
      "target_id": 123456789
    },
    "drop": {
      "target_type": "string",
      "target_id": 123456789
    }
  }
}
```
### Server->client response
```javascript
{
  "type": "resp_action",
  "resp_action": {
    "id": 123456789,
    "action_type": "string",
    "status": "string",
    "error": "string",
    "sight_data": {
      "type": "partial",
      "orientation": "string",
      "width": 123456789,
      "height": 123456789,
      "pattern": "string"
    },
    "character": {
      "status": "string",
      "health": 123456789
    }
  }
}
```
