#Doors

###Can be detected from the sight_data by following symbols
```
+  Closed door
-  Open door
```
Door can be opened and closed when @ orientation is pointing against the door and the door is in the next square from @.
They can't be closed when other character is in the same square with the door. A door can't be opened or closed when orientation is half-cardinal point.
```
Orientation: east
  SS
 #SSS
@+SSS
 #SSS
  SS
```

```
Orientation: east
  SS
 #SSS
@-...
 #SSS
  SS
  
( TODO: shadow shape to be defined ... )
```
###Door is both opened and closed with action request "interact". Target ID value is not used.
```
{
  "type": "req_action",
  "req_action": {
    "from": {
      "id": 1010101,
      "nick": "bot1234",
      "password": "passivoerdi"
    },
    "action_type": "interact",
    "interact": {
      "target_type": "door",
      "target_id": 0
    }
  }
}
```
###Server replies to the request by telling was the action successful
```javascript
{
  "type": "resp_action",
  "resp_action": {
    "id": 123456789,
    "action_type": "interact",
    "status": "ok",    // ok, forbidden, locked, conflict
    "error": "string",
    "sight_data": {
      ...
    }
  }
}
```
###Status codes for door related actions:
```
ok = opening or closing successful
forbidden = can't be closed due to other character in the same square with the door
locked = can't be opened since door is locked ( todo: key functionality )
conflict = other character is opening/closing at the same time, first to interact gets status "ok"
```
