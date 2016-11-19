#GESS
#Fulguritude


CONCEPTION LOG

RESSOURCES :
http://www.archim.org.uk/eureka/53/gess.html







V0.1.0) First phase of architecture, after debugging.

Built :
Board
Unit
FootPrint

and could construct, iterate and check footprints on the board properly.




V0.2.0) Second phase of architecture, after debugging

Built :
Piece
Movespace

Can print each piece's correct MoveSpace
Code works for all pieces with a center on the board.
Rules for Pieces conflict for Gess depending on the source :
1- Pieces have a center between 1-16 for both x and y (Norm NbHood); we could call it the Chess-rule.
2- Pieces have a center between (-1)-18 for both x and y (centers are abstract and can be out-of-bounds : perimeter pieces are more free) we could call it the Go-rule.
3- Pieces have a center between 0-17 for both x an y (the center must be inBounds : a perimeter piece is a bit more free than Chess-rule, a bit less free than Go-rule, we will call it the Mid-rule ; I haven't seen this rule anywhere, but my code, if FootPrint.isPiece is unrestricted by Chess-rule, functions this way)

This version implements rule 1 and can be immediately converted to rule 3. Rule 2 poses many problems though; maybe a later implementation ? 







V0.3.0) MADE THE GAME PLAYABLE

This version includes a fully playable text based Gess game with the MID rule as a selection rule. CHESS rule and GO rule have been abandoned.

Since last version :
- player, game classes implemented
- players only human
- various stdin scanners and stdout printers added to make the game more easily playable



The MID rule stipulates that a Unit can only serve for the center of a piece if it is located on the board. 

This is :
-less restrictive than the CHESS rule (which only allows Units with Norm-Neighborhood to be valid pieces). //implementable without too much hassle
-more restrictive than the GO rule (which allows off-board "centers" for pieces, as long as only the player's stones are near the edge) //not-implemented



V0.3.5)

Made classes for AI, but the protocol is just wayyyyy too computation hungry. This will have to stay an intermediary step. 
The countKing countStones functions are called way too much and their results should be made an object attribute upon construction.
The step which is most computation hungry is checking whether you are in check, or if a move solves the fact that you are in check. This can't be avoided with the current architecture.




V0.3.9)

I toyed with the code a lot and got this version, with an AI which is sort of interesting.

First of all, it's FAST, which is great. 

It is also rather smartly, but recklessly aggressive. There are barely any defense protocols.

It goes for the kill of a king when it gets a chance, any kill, with priority on CHECKMATE in case of "two eyes" as in Gô, ie :

XXXXX
X X X
XXXXX

Counts as two kings; and if the AI can destroy the middle, and thus both kings at once, it'll go for that move first.


It doesn't know when it is in check. However, to make up for that, creating new kings is given very high priority. It's an economical strategy, and works well enough, even if a human would probably be able to break it.
It will go for ring/king creation even if it means losing stones ! 


The next logical step is to optimize reward for killing simple stones ; avoiding the death of our own stones, and knowing when we're putting an enemy in check, 1 turn deep. 


This archive was made to allow the cleaning up of the AIPlayer class.






V0.4.0) FINALLY A WORKING AI !!!

NB : here is a description for 0.4.1, which basically just adds validity checks for moves everywhere compared to 0.4.0.

It's not perfect, I grant it a finite wait time near the end, once the important things have been checked.

It is a sequential approach, in three parts :

Part 1 :
In order of return priority :
1- If a move is CHECKMATE, and it is valid : DO IT ! nothing else matters. 
2- If a move kills an enemy king or creates a king for you, and it is valid, DO IT. //the equality in priority may be worth changing ? It was simpler to code this way.

Y-Any move that loses a king is removed.
Z-Any move that loses the game is removed.


Part 2 : 
We check all moves that eat opponents stones, and extract them into an array. Movelist become moves with 0 health.
These PositiveMoves are then studied better, and it's checked whether they are valid or not, and whether they cause to be put into CHECK or if the opponent is put into CHECK. 

The best move left over, if any are valid, is selected and used. 


Part 3 :
If no positive moves are found up to now, we run a check for a random valid move that isn't bad, with an iteration limit (MAXWAIT = 15).
If any move that isn't directly penalizing is found within the limit, it is chosen. 
If not, a random move, perhaps penalizing, is selected. 


//Maybe make a "valid but bad moves array" so the game can pick a penalizing move rather than none ; this might be a problem towards the end of the game when less moves are available.



O.4.2) Most of the code has been cleaned from clutter



NB : for human play, change the arguments in Main to read Player.HU
NB : It seems my code has taught me a use for macros. In such a sequential computation, it can be interesting to modify which lines are read without removing the rest of the function, and thus run only some of the checks. Macros could allow for a more fluid manipulation of heuristics than retyping. Verify.
NB : B seems to lose a lot against W. Maybe the AI is just better at reacting than planning (which would make a lot of sense considering my code and the NP clusterfuck that is this game).






NEXT STEP IS TO LEARN HOW TO GIVE IT A GUI ! 

I might try deep learning the software next semester (if I can learn the techniques by then), as this is definitely the kind of problem that could use a convolutional neural network. 

I might have to recode most of the game in C though, make the code more optimal with bitmasks and whatnot. 
