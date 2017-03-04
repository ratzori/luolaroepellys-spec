#Movement

##Moving is requested with "action" type request

###Action type "move" moves bot one point to requested direction

```javascript
"request": {
  "type": "action",
  "action_type": "move",
  "move": {
    "direction": "forward" // forward, backward, left, right
  }
```
  
##Rotation is requested with "rotate" type request

###Rotation turns bot to requested orientation
( server can add delay when rotating more than one point; to be definied )

```javascript
"request": {
  "type": "action",
  "action_type": "rotate",
  "rotate": {
    "orientation": "south" // north, east, south, west ( half-cardinal points supported later )
  }
```
