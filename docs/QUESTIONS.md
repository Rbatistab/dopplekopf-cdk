### How the clients get sync with the server? 
* (Tentative) SSE - Server Side events

### How does the FE knows it’s looking at the latest data? How does it know it has to fetch?Define doppelkopf architercture - DRAFT
* Autorefresh every X minutes?
* BE checksum of the latest game and constantly poll
* If the FE knows it looses connection it should check against the BE/S3
* Resolve if 2 or more users act at the same time

### How does the user know if their message was received?
* 

### How does the BE differentiate requests from different players?
*

### How does the BE differentiate from game to game?
*

### Do you need a DB to implement chat? Do people get chat history back? Maybe look for web socket chat
* 

### How to tell the players that the game has started, maybe like a poll and then transition to the game state on the FE
*
