#Sight data

###Example of the sight pattern data
```
orientation:  north-east
type:         partial
height:       5
width:        5
pattern:      SSSSSS.##SS..#SS.C.S@SSSS
```
### Full pattern is used in initialization to inform full possible field of view area
```
In full pattern @ is in the center of the pattern and width and height are odd.
Full pattern indicates only symbols S and X.
```
###Partial pattern is decoded using the current orientation and splitted with width and height
```
north         @ is in the bottom-center
nort-east     @ is in the bottom-left corner
east          @ is in the middle-left
and so on...
```
###Decoded info mapped to full field of view
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
X  Area than can be seen ( used only in full pattern )
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
