### How the clients get sync with the server? 
* (Tentative) SSE - Server Side events

### How does the FE knows it’s looking at the latest data? How does it know it has to fetch?Define doppelkopf architercture - DRAFT
* Autorefresh every X minutes?
* BE checksum of the latest game and constantly poll
* If the FE knows it looses connection it should check against the BE/S3
* Resolve if 2 or more users act at the same time

### How does the user know if their message was received?
* TBD (A checksum?)

### How does the BE differentiate requests from different players?
* When a new player joins the game, the BE will provide a player GUID, so that each request from the FE will contain an identifier as `GameGUID-PlayerGUID` which should match the player information on the game state stored on the database

### How does the BE differentiate from game to game?
* Each game is identified with a GUID which is also the identifier (or primary key) on the database that will track the game state

### Do you need a DB to implement chat? Do people get chat history back? Maybe look for web socket chat
* Sounds like a desired feature on such game, but is not contemplated at the moment to have a chat

### How to tell th players that the game has started, maybe like a poll and then transition to the game state on the FE
* TBD (Try with Server-Sent events)
