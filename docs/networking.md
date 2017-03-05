#Networking

## Luolapeliroepellys uses client-server networking architecture

Client and server uses the same agreed tick rate and lag compensation is applied. Server needs to keep stored previous states for characters, items and doors for specified time ( for example 1000ms ).
Server discards client requests when ping is so long that previous state can't be returned.

Example of request handling if tick rate is 20ms:
```
1 = client sends request
2 = server receives the request
3 = server handles the request, uses state from point T to calculate results and sends resp
4 = client receives resp
         
1st |___XX___| = delay from client to server = ping/2
2nd |___XX___| = delay from server to client = ping/2

           0     20     40     60     80    100    120    140    160    180    200
Server     |      |      |      |      |      |      |      |      |      |      |
                         T  1             2   3             5
Client                   |  |_______40____|   |_______40____|
