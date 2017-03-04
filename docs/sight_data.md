#Sight data

##Full sight data
Full data is used only once in the initialization to inform full possible field of view. In full pattern @ is in the center of the pattern and width and height are odd numbers. Full pattern contains only symbols S, X and one @. Orientation is the initial orientation of the bot.
```
X  Area that can be seen
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
Partial sight data is used when the bot has succesfully joined the game and has received full pattern data. Partial sight data must fit inside the full sight data.

###Example of the partial sight data
```
orientation:  north-east
type:         partial
height:       5
width:        5
pattern:      SSSSSS.##SS..#SS.C.S@SSSS
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
C  Character ( other bots )
I  Item
.  Terrain
%  Food
+  Closed door
-  Open door
<  Staircase up    
>  Staircase down  
```
