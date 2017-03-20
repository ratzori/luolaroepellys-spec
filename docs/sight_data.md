#Sight data

##Full sight data
Full data is used only once in the initialization to inform full possible field of view. In full pattern @ ( the bot ) is in the center of the pattern and width and height are odd numbers. Full pattern contains only symbols S, X and one @. Orientation is the initial orientation of the bot. Full sight data does not contain item or character list.
```
@  The bot
X  Area that can be seen // only used in full pattern data
S  Shadow ( area that is out of the sight )
```

###Example of the partial sight data
```
orientation:  north
type:         full
height:       9
width:        9
pattern:      SSSXXXSSSSSXXXXXSSSXXXXXXXSXXXXXXXXXXXXX@XXXXXXXXXXXXXSXXXXXXXSSSXXXXXSSSSSXXXSSS
```
###Full pattern is decoded to table by splitting with width and height
```
  012345678 ( width )
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

##Partial sight data
Partial sight data is used when the bot has succesfully joined the game and has received full pattern data. Partial sight data must fit inside the full sight data. Partial sight data contains also list of visible items and characters. 

###Example of the partial sight data
```
orientation:         north-east
type:                partial
height:              5
width:               5
pattern:             SSSSSS.##SS..#SS.C.S@SSSS
visible items:       key, weapon ...
visible characters:  human, dog ...
```

###Partial pattern is decoded using the current orientation and splitted with width and height
```
north         @ is in the bottom-center

  01234 ( width )
0 S...S
1 .....
2 .....
3 S...S
4 SS@SS

nort-east     @ is in the bottom-left corner
  01234
0 SSSSS
1 S...S
2 S...S
3 S...S
4 @SSSS

east          @ is in the middle-left

  01234
0 SS..S
1 S....
2 @....
3 S....
4 SS..S

and so on...
```
###Decoded info mapped to full field of view table
```
  01234  ( width )
  45678  ( width mapped to full sight of view )
0 SSSSS
1 S.##S
2 S..#S
3 S.C.S
4 @SSSS
```

###Explanation of the symbols
```
#  Wall
S  Shadow ( area that is out of the sight )
@  Own bot
C  Character ( human, dog... )
I  Item
.  Terrain
%  Food
+  Closed door
-  Open door
<  Staircase up    
>  Staircase down  
```
Characters can stand on the same spot with items and food. @ and C are always shown when there is character on top of item/food.

### Initial state after joining to server
Server will spawn bots only to terrain points that don't contain item/food/bots so bot can assume that it is standing on point with type "."

###Visible items and characters
Visible items are given as list so that first item is given from the top-left corner and last item from bottom-right corner of the partial pattern data. When other character is standing on the same spot with an item the item is not given in the list.

Example
```
Orientation: north

  01234
0 S...S
1 .I...
2 ..I..
3 SI..S
4 SS@SS

Item array index counting increments from left to right. When line ends continues from next line i.e.
  01234
0 S...S
1 .0...
2 ..1..
3 S2..S
4 SS@SS

"visible_items": [
  {
    "type": "key",    // 0
    "id": 123456789,
    "key": {
      "type": "..."
    }
  },
  {
    "type": "weapon",  // 1
    "id": 123456789,
    "weapon": {
      "type": "melee"
    }
  },
  {
    "type": "key",    // 2
    "id": 123456789,
    "key": {
      "type": "..."
    }
  }
]
```
Similar method is applied to visible characters:
```
Orientation: north

  01234
0 S...S
1 ...C.
2 C...C
3 S...S
4 SS@SS

Item array index counting increments from left to right. When line ends continues from next line i.e.
  01234
0 S...S
1 ...0.
2 1...2
3 S...S
4 SS@SS

"visible_characters": [
  {
    "type": "human",     // 0
    "human": {
      "id": 123456789,
      "name": "bot4321",
      "status": "dead"
    }
  },
  {
    "type": "dog",       // 1
    "dog": {
      "id": 123456789,
      "name": "Musti",
      "status": "alive"
    }
  },
  {
    "type": "human",     // 2
    "human": {
      "id": 123456789,
      "name": "bossimat",
      "status": "unconscious"
    }
  }
]
```
