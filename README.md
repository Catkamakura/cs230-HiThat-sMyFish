> author: Jiarui Liu
>
> Last modification: 2022-03-21
>
> recommend open as markdown file

> Project code stored in private repo.(According to the university policy)

# Hey, that's my fish

A student implementation of game "hey, that's my fish" in cs230 project 1. 



### Overview

The program mainly have 4 layers: main layer, action layer, motion layer and layer of helper.

+ main layer is the ```main()``` function in fish.c, the only entry function in this program.

+ action layer handles a bunch of motions in a period of time(like AI's behaviors in one turn)
+ motion layer handles one single motions like render the virtual display screen or move a player
+ helper layer is some small function that helps the implementation motion functions 

You can find more detail in [Features](#Features) section.



### How to Start

To start to play, you should compile the ```fish.c``` file and run the output.



In the first turn you need to input the coordinate of your start gird--first you input an integer between 1 and 6 as the row of your start island, then the computer will guide you input an integer again as column of the island. If the island you choose is valid(i.e. the island only has 1 fish), you will find a "P" gird on the game board represents the Human Player. Otherwise, you will be asked to input again.


Then, a mysterious power from the East will change the timeline, and you are forced to start at (1,1). If you want to start at the island you choose, just annotate line 878 - 889 in fish.c.


After you decide your start point, the AI will automatically choose her start girds.



In the later turn, you are asked to input an instruction to help move the "P". The instruction should satisfy "DIRECTION NUMBER" format in [DevelopDoc](https://docs.google.com/document/d/1BydIFtla2wNqu7G8zsF6Aj4cSwLZMzTOCW0vpZhgZPM). 



The AI will automatically decide and move in her turn.



Score Board on the screen update every turn and shows recent score of the two player. 



If one player dead in the game, his turn will be jumped over.



The game end if all the player dead in the map. The one with highest score will win the game. 



To start another game, run the output executable file again.



## Features

### UI

I try to create a friendly command-line user interface in this program include the welcome screen and time delay if you want to disable those features, just annotate below function call in ```fish.c```.

+ ```welcomeScreen()```  call in ```main()```
  
+ ```#include <unistd.h>``` and all the ```sleep()``` in fish.c (default disabled)



### AI

 Magallan is a cute and smart penguin AI in the game, you can find more information about magallan at https://gamepress.gg/arknights/operator/magallan



Magallan is very smart. Thanks to the advanced algorithm using in this program, she won't just go to place she can get most fish in recent turn. Magallan will put herself in play's position and then find the best island enable her get more fish in the near future.


A [game log](#Game Log) is available at the end of this document showing that how AI make decisions.



### Algorithm

Magallan is implemented with **minimax algorithm** with **alpha-beta pruning**--a algorithm in games of perfect information. 



Basically, the minimax algorithm  is a back and forth between the two players, where each player try  to pick the move with the maximum score. The algorithm using a search tree with two kinds of nodes, nodes representing your moves and nodes representing the opponent's move.



Alpha-beta pruning is a search algorithm seeks to decrease the number of nodes that are evaluated by the minimax algorithm in its search tree.[^1]



You can find the implementation of those algorithm in function ```score()``` and ```actionAIMove()```. 

More information about minimax algorithm and alpha-beta pruning algorithm can be found at:

+ http://web.cs.ucla.edu/~rosen/161/notes/alphabeta.html
+ https://www.neverstopbuilding.com/blog/minimax
+ https://www.flyingmachinestudios.com/programming/minimax/.



### Data Structure

This program primarily uses malloc() and free() to allocate memory. 



The map was implemented and operate like the one in 1 dimension array. I use index 0 - 35 to indicate islands in the map. 



00 01 02 03 04 05

06 07 08 09 10 11

12 13 14 15 16 17

18 19 20 21 22 23

24 25 26 27 28 29

30 31 32 33 34 35



The index value of a given a coordinate (x, y) is ```6 * (y - 1) + x - 1```.

the direction of movement can also easily transfer into integer signal on index. For example,

+ a "UPLEFT x" movement substracts recent index by 7x 
+ a "down x" movement adds 6x to recent index



The map(Game board) also has pointers in its struct. It can be easily access though the link. For instance, the (1,1) gird has a "right" neighbor (2,1) but doesn't have a "left" neigbor.

in the map, you can find map[0].right is a pointer to the address of map[1], and map[0].left is a pointer to NULL as its left neighbor doesn't exist. It can be navigate via pointers or Index. In this project, I use Index for navigation which allows my code more clear and efficient.



Variables are stored in structs which allows conveniently transfer of parameters. You can find more information in the Doxygen-style documentation in file ```fish.c```.


### Additional feature

Basically, the algorithm allows you to start at any point you like on the map, I ban those feature to fit the project requirement.

If you annotate related input-verification and forced start point change, you can execute without any problem:
+ start point 1 fish check: line 870 - 873
+ forced start point change: line 878 - 889



### Other 

It's my first program written in c. You can find many java-style codes in the frame and functions. I will try to code more C-like in the next project.



#### Game Log

```
/tmp/tmp.vLgMeLzBrT/cmake-build-edlab/project1
  _   _                _   _           _   _                         _____ _     _     _
 | | | | ___ _   _    | |_| |__   __ _| |_( )___   _ __ ___  _   _  |  ___(_)___| |__ | |
 | |_| |/ _ \ | | |   | __| '_ \ / _` | __|// __| | '_ ` _ \| | | | | |_  | / __| '_ \| |
 |  _  |  __/ |_| |_  | |_| | | | (_| | |_  \__ \ | | | | | | |_| | |  _| | \__ \ | | |_|
 |_| |_|\___|\__, ( )  \__|_| |_|\__,_|\__| |___/ |_| |_| |_|\__, | |_|   |_|___/_| |_(_)
             |___/|/                                         |___/
-----------------------------------------------------------------------------------
Magallan is a field specialist employed by Rhine Lab, operating under a cooperation agreement with Rhodes Island.
She is currently based with us as she prepares to launch a new expedition.
Magallan fishing at post-apocalyptic North Pole ever since she was a little girl.
she is very skilled at "Hey, that's my fish"
-----------------------------------------------------------------------------------
|                 Magallan wants to play this game with you.                      |
-----------------------------------------------------------------------------------
(Game Start in 3 seconds.)

>>>> Magallan(AI): Hehehe, just so you know, I'm very strong, so don't be scared!

Game Start!
initializing Map...
------------Map------------
   1 2 3 4 5 6
1  1 1 1 1 1 1
2  1 2 2 2 2 1
3  1 2 3 3 2 1
4  1 2 3 3 2 1
5  1 2 2 2 2 1
6  1 1 1 1 1 1

This is the initial game board

The Player goes first!
Select your start island!
You can only start at island with 1 fish!
Input the row of your start island:
3
3
Input the column of your start island:
1
1
You selected (3,1) as your start island.

------------Map------------
   1 2 3 4 5 6
1  1 1 1 1 1 1
2  1 2 2 2 2 1
3  P 2 3 3 2 1
4  1 2 3 3 2 1
5  1 2 2 2 2 1
6  1 1 1 1 1 1



-----------------------------------------------------------------------------------
|         Oh, a mysterious power from the East changes the timeline!              |
-----------------------------------------------------------------------------------


You start your game at (1,1)!

-------Score Panel---------
Player: 1
AI: 0
------------Map------------
   1 2 3 4 5 6
1  P 1 1 1 1 1
2  1 2 2 2 2 1
3  1 2 3 3 2 1
4  1 2 3 3 2 1
5  1 2 2 2 2 1
6  1 1 1 1 1 1



Magallan selected (6,6) as her start island.
-------Score Panel---------
Player: 1
AI: 1
------------Map------------
   1 2 3 4 5 6
1  P 1 1 1 1 1
2  1 2 2 2 2 1
3  1 2 3 3 2 1
4  1 2 3 3 2 1
5  1 2 2 2 2 1
6  1 1 1 1 1 A
Your Turn!
Input the direction and the distance you want to move:
down 3
down 3
You moved from (1,1) to (4,1)!

-------Score Panel---------
Player: 2
AI: 1
------------Map------------
   1 2 3 4 5 6
1  . 1 1 1 1 1
2  1 2 2 2 2 1
3  1 2 3 3 2 1
4  P 2 3 3 2 1
5  1 2 2 2 2 1
6  1 1 1 1 1 A



Magallan's Turn!
Magallan moved from (6,6) to (4,4)!

-------Score Panel---------
Player: 2
AI: 4
------------Map------------
   1 2 3 4 5 6
1  . 1 1 1 1 1
2  1 2 2 2 2 1
3  1 2 3 3 2 1
4  P 2 3 A 2 1
5  1 2 2 2 2 1
6  1 1 1 1 1 .



Your Turn!
Input the direction and the distance you want to move:
up 2
up 2
You moved from (4,1) to (2,1)!

-------Score Panel---------
Player: 3
AI: 4
------------Map------------
   1 2 3 4 5 6
1  . 1 1 1 1 1
2  P 2 2 2 2 1
3  1 2 3 3 2 1
4  . 2 3 A 2 1
5  1 2 2 2 2 1
6  1 1 1 1 1 .



Magallan's Turn!
Magallan moved from (4,4) to (4,3)!

-------Score Panel---------
Player: 3
AI: 7
------------Map------------
   1 2 3 4 5 6
1  . 1 1 1 1 1
2  P 2 2 2 2 1
3  1 2 3 3 2 1
4  . 2 A . 2 1
5  1 2 2 2 2 1
6  1 1 1 1 1 .



Your Turn!
Input the direction and the distance you want to move:
down 1
down 1
You moved from (2,1) to (3,1)!

-------Score Panel---------
Player: 4
AI: 7
------------Map------------
   1 2 3 4 5 6
1  . 1 1 1 1 1
2  . 2 2 2 2 1
3  P 2 3 3 2 1
4  . 2 A . 2 1
5  1 2 2 2 2 1
6  1 1 1 1 1 .



Magallan's Turn!
Magallan moved from (4,3) to (3,3)!

-------Score Panel---------
Player: 4
AI: 10
------------Map------------
   1 2 3 4 5 6
1  . 1 1 1 1 1
2  . 2 2 2 2 1
3  P 2 A 3 2 1
4  . 2 . . 2 1
5  1 2 2 2 2 1
6  1 1 1 1 1 .



Your Turn!
Input the direction and the distance you want to move:
upright 2
upright 2
You moved from (3,1) to (1,3)!

-------Score Panel---------
Player: 5
AI: 10
------------Map------------
   1 2 3 4 5 6
1  . 1 P 1 1 1
2  . 2 2 2 2 1
3  . 2 A 3 2 1
4  . 2 . . 2 1
5  1 2 2 2 2 1
6  1 1 1 1 1 .



Magallan's Turn!
Magallan moved from (3,3) to (2,3)!

-------Score Panel---------
Player: 5
AI: 12
------------Map------------
   1 2 3 4 5 6
1  . 1 P 1 1 1
2  . 2 A 2 2 1
3  . 2 . 3 2 1
4  . 2 . . 2 1
5  1 2 2 2 2 1
6  1 1 1 1 1 .



Your Turn!
Input the direction and the distance you want to move:
downleft 1
downleft 1
You moved from (1,3) to (2,2)!

-------Score Panel---------
Player: 7
AI: 12
------------Map------------
   1 2 3 4 5 6
1  . 1 . 1 1 1
2  . P A 2 2 1
3  . 2 . 3 2 1
4  . 2 . . 2 1
5  1 2 2 2 2 1
6  1 1 1 1 1 .



Magallan's Turn!
Magallan moved from (2,3) to (2,4)!

-------Score Panel---------
Player: 7
AI: 14
------------Map------------
   1 2 3 4 5 6
1  . 1 . 1 1 1
2  . P . A 2 1
3  . 2 . 3 2 1
4  . 2 . . 2 1
5  1 2 2 2 2 1
6  1 1 1 1 1 .



Your Turn!
Input the direction and the distance you want to move:
down 4
down 4
You moved from (2,2) to (6,2)!

-------Score Panel---------
Player: 8
AI: 14
------------Map------------
   1 2 3 4 5 6
1  . 1 . 1 1 1
2  . . . A 2 1
3  . 2 . 3 2 1
4  . 2 . . 2 1
5  1 2 2 2 2 1
6  1 P 1 1 1 .



Magallan's Turn!
Magallan moved from (2,4) to (2,5)!

-------Score Panel---------
Player: 8
AI: 16
------------Map------------
   1 2 3 4 5 6
1  . 1 . 1 1 1
2  . . . . A 1
3  . 2 . 3 2 1
4  . 2 . . 2 1
5  1 2 2 2 2 1
6  1 P 1 1 1 .



Your Turn!
Input the direction and the distance you want to move:
right 3
right 3
You moved from (6,2) to (6,5)!

-------Score Panel---------
Player: 9
AI: 16
------------Map------------
   1 2 3 4 5 6
1  . 1 . 1 1 1
2  . . . . A 1
3  . 2 . 3 2 1
4  . 2 . . 2 1
5  1 2 2 2 2 1
6  1 . 1 1 P .



Magallan's Turn!
Magallan moved from (2,5) to (3,4)!

-------Score Panel---------
Player: 9
AI: 19
------------Map------------
   1 2 3 4 5 6
1  . 1 . 1 1 1
2  . . . . . 1
3  . 2 . A 2 1
4  . 2 . . 2 1
5  1 2 2 2 2 1
6  1 . 1 1 P .



Your Turn!
Input the direction and the distance you want to move:
up 3
up 3
You moved from (6,5) to (3,5)!

-------Score Panel---------
Player: 11
AI: 19
------------Map------------
   1 2 3 4 5 6
1  . 1 . 1 1 1
2  . . . . . 1
3  . 2 . A P 1
4  . 2 . . 2 1
5  1 2 2 2 2 1
6  1 . 1 1 . .



Magallan's Turn!
Magallan moved from (3,4) to (4,5)!

-------Score Panel---------
Player: 11
AI: 21
------------Map------------
   1 2 3 4 5 6
1  . 1 . 1 1 1
2  . . . . . 1
3  . 2 . . P 1
4  . 2 . . A 1
5  1 2 2 2 2 1
6  1 . 1 1 . .



Your Turn!
Input the direction and the distance you want to move:
upright 1
upright 1
You moved from (3,5) to (2,6)!

-------Score Panel---------
Player: 12
AI: 21
------------Map------------
   1 2 3 4 5 6
1  . 1 . 1 1 1
2  . . . . . P
3  . 2 . . . 1
4  . 2 . . A 1
5  1 2 2 2 2 1
6  1 . 1 1 . .



Magallan's Turn!
Magallan moved from (4,5) to (5,4)!

-------Score Panel---------
Player: 12
AI: 23
------------Map------------
   1 2 3 4 5 6
1  . 1 . 1 1 1
2  . . . . . P
3  . 2 . . . 1
4  . 2 . . . 1
5  1 2 2 A 2 1
6  1 . 1 1 . .



Your Turn!
Input the direction and the distance you want to move:
down 1
down 1
You moved from (2,6) to (3,6)!

-------Score Panel---------
Player: 13
AI: 23
------------Map------------
   1 2 3 4 5 6
1  . 1 . 1 1 1
2  . . . . . .
3  . 2 . . . P
4  . 2 . . . 1
5  1 2 2 A 2 1
6  1 . 1 1 . .



Magallan's Turn!
Magallan moved from (5,4) to (5,3)!

-------Score Panel---------
Player: 13
AI: 25
------------Map------------
   1 2 3 4 5 6
1  . 1 . 1 1 1
2  . . . . . .
3  . 2 . . . P
4  . 2 . . . 1
5  1 2 A . 2 1
6  1 . 1 1 . .



Your Turn!
Input the direction and the distance you want to move:
down 2
down 2
You moved from (3,6) to (5,6)!

-------Score Panel---------
Player: 14
AI: 25
------------Map------------
   1 2 3 4 5 6
1  . 1 . 1 1 1
2  . . . . . .
3  . 2 . . . .
4  . 2 . . . 1
5  1 2 A . 2 P
6  1 . 1 1 . .



Magallan's Turn!
Magallan moved from (5,3) to (4,2)!

-------Score Panel---------
Player: 14
AI: 27
------------Map------------
   1 2 3 4 5 6
1  . 1 . 1 1 1
2  . . . . . .
3  . 2 . . . .
4  . A . . . 1
5  1 2 . . 2 P
6  1 . 1 1 . .



Your Turn!
Input the direction and the distance you want to move:
left 1
left 1
You moved from (5,6) to (5,5)!

-------Score Panel---------
Player: 16
AI: 27
------------Map------------
   1 2 3 4 5 6
1  . 1 . 1 1 1
2  . . . . . .
3  . 2 . . . .
4  . A . . . 1
5  1 2 . . P .
6  1 . 1 1 . .



Magallan's Turn!
Magallan moved from (4,2) to (5,1)!

-------Score Panel---------
Player: 16
AI: 28
------------Map------------
   1 2 3 4 5 6
1  . 1 . 1 1 1
2  . . . . . .
3  . 2 . . . .
4  . . . . . 1
5  A 2 . . P .
6  1 . 1 1 . .



Your Turn!
Input the direction and the distance you want to move:
downleft 1
downleft 1
You moved from (5,5) to (6,4)!

-------Score Panel---------
Player: 17
AI: 28
------------Map------------
   1 2 3 4 5 6
1  . 1 . 1 1 1
2  . . . . . .
3  . 2 . . . .
4  . . . . . 1
5  A 2 . . . .
6  1 . 1 P . .



Magallan's Turn!
Magallan moved from (5,1) to (5,2)!

-------Score Panel---------
Player: 17
AI: 30
------------Map------------
   1 2 3 4 5 6
1  . 1 . 1 1 1
2  . . . . . .
3  . 2 . . . .
4  . . . . . 1
5  . A . . . .
6  1 . 1 P . .



Your Turn!
Input the direction and the distance you want to move:
left 1
left 1
You moved from (6,4) to (6,3)!

-------Score Panel---------
Player: 18
AI: 30
------------Map------------
   1 2 3 4 5 6
1  . 1 . 1 1 1
2  . . . . . .
3  . 2 . . . .
4  . . . . . 1
5  . A . . . .
6  1 . P . . .



Magallan's Turn!
Magallan moved from (5,2) to (6,1)!

-------Score Panel---------
Player: 18
AI: 31
------------Map------------
   1 2 3 4 5 6
1  . 1 . 1 1 1
2  . . . . . .
3  . 2 . . . .
4  . . . . . 1
5  . . . . . .
6  A . P . . .



GAME OVER!
Your score is: (1 + 1 + 1 + 1 + 1 + 2 + 1 + 1 + 2 + 1 + 1 + 1 + 2 + 1 + 1) = 18
Magallan's score is: (1 + 3 + 3 + 3 + 2 + 2 + 2 + 3 + 2 + 2 + 2 + 2 + 1 + 2 + 1) = 31

YOU LOST!

>>>> Magallan(AI): Do you need my skills?

Process finished with exit code 0


```

​																																				

[^1]: https://en.wikipedia.org/wiki/Alpha%E2%80%93beta_pruning 

