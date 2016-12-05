#GESS
#Fulguritude


CONCEPTION LOG

RESOURCES :
http://www.archim.org.uk/eureka/53/gess.html







0.4.5) I decided to pursue the naive approach to this AI for fun. I added a random move function at the end, corrected all the necessary validity tests for moves, divided them into groups of respective health to optimize treatment and overall effectiveness. I kept watching this 0.4.3 compete against itself, giving the AIs debug lines to "explain how they think" and thus understand them better.

I came to the realization that because of the checks in place against putting yourself in a lethal position, if we arrived at the end of Part 3, it meant no positive move (part 2) prevented being in fullCheck (ie, the opponent has a killing move right now). 
Hence, after finding a fullCheck (there is no need to check for every fullCheck as the existence of two different ones implies checkmate ; the probability of such a checkmate existing AND there existing a response for all attacks is very small and not worth checking ; I also made sure to avoid the AI would accept a move that would once again put it in fullCheck). 
Once I have found such a fullCheck for my opponent, I analyze it. I used a bunch of try-catches to see what every one of my remaining moves can do against it ; and how much that move still works after my move. Ie, if it still a valid piece, if can still end on the same destination it had chosen, and finally, if I am still in fullCheck even after the move. 


I have also been coding debugging material, like extra string methods to copy-paste as a startConfig in a given part in the game, as well as a constructor in Game that could take into account whose turn is starting given this start config. Maybe I should make startConfig a Pair<String[],Stone> to define the starting turn in an easier manner.


By now I've taken care of most of the debugging to do I believe. In order to make the testing more fun, I'll be coding the GUI soon enough. 
 

Making the GUI seems essential now that I have so much text appearing between each turn ; it would allow for interactive expression of speed and give visibility to each move being studied. With BASH CRTL+Z you can probably do some interesting pausing.
Maybe add a sleep() in the end so that the human player can witness the chosen move, to accept validity.
It might also be interesting to code a log file saving system for games, and a way to graphically reenact games from log files.


As for some of the most interesting notes : I'm now rather satisfied with this naive approach. Aesthetically speaking, this AI is more /mechanic/ (which speaks lengths about its quality when it makes no lethal mistakes, I believe) : it basically charges head on as soon as it gets the chance, and always (?) finds a counter to fullCheck. But it is also slightly less entertaining to watch play. The larger importance given to randomness in v0.4.2 and v0.4.3 (only exists as a jar I made to debug my work on v0.4.2) gave something more /organic/ to the pseudo cellular automatons fighting on the board. They were more surprising and original. However, I believe that 0.4.5 would trounce 0.4.2, if only because games between two 0.4.5s often end on having both a lonely king and some sparse or immobile stones (edges etc). 



I wonder if this part 4 should be put earlier (like right after the initial check for a winning move) to gain speed. It might be interesting to put it before stoneCounts and kingCounts in particular, as well as before the positive move function (part2). However, it might be more interesting to check for positive moves first, if an agressive move just so happens to also get you out of being in fullCheck. I'll have to analyze my code to see what's best.


_____________________________________________________




O.4.2) Most of the code has been cleaned from clutter



NB : for human play, change the arguments in Main to read Player.HU
NB : It seems my code has taught me a use for macros. In such a sequential computation, it can be interesting to modify which lines are read without removing the rest of the function, and thus run only some of the checks. Macros could allow for a more fluid manipulation of heuristics than retyping. Verify.
NB : B seems to lose a lot against W. Maybe the AI is just better at reacting than planning (which would make a lot of sense considering my code and the NP clusterfuck that is this game).





NEXT STEP IS TO LEARN HOW TO GIVE IT A GUI ! 

I might try deep learning the software next semester (if I can learn the techniques by then), as this is definitely the kind of problem that could use a convolutional neural network. 

I might have to recode most of the game in C though, make the code more optimal with bitmasks and whatnot. 





_________________________________________________________________






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





________________________________________________________________________



V0.3.9)

I toyed with the code a lot and got this version, scrapped most of it, with an AI which is sort of interesting.

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


This archive was made to allow the cleaning up and scrapping of most of the AIPlayer class.




_______________________________________________________________







V0.3.5)

Made classes for AI, but the protocol is just wayyyyy too computation hungry. This will have to stay an intermediary step. 
The countKing countStones functions are called way too much and their results should be made an object attribute upon construction.
The step which is most computation hungry is checking whether you are in check, or if a move solves the fact that you are in check. This can't be avoided with the current architecture.




_____________________________________________________________





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





________________________________________________________________







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







____________________________________________________________________





V0.1.0) First phase of architecture, after debugging.

Built :
Board
Unit
FootPrint

and could construct, iterate and check footprints on the board properly.


