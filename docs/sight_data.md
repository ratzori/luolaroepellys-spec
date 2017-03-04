#Sight data

##Full sight data
Full data is used only once in the initialization to inform full possible field of view. In full pattern @ is in the center of the pattern and width and height are odd numbers. Full pattern contains only symbols S, X and one @.
```
X  Area than can be seen
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

##Partial sight data
Partial sight data is used when the bot has succesfully joined the game

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
nort-east     @ is in the bottom-left corner
east          @ is in the middle-left
and so on...
```
###Decoded info is mapped to full field of view
```
  45678
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
