### How the clients get sync with the server? 
* (Tentative) SSE - Server Side events

### How does the FE knows it’s looking at the latest data? How does it know it has to fetch?Define doppelkopf architercture
- Autorefresh every X minutes?
- BE checksum of the latest game and constantly poll
- If the FE knows it looses connection it should check against the BE/S3
- Resolve if 2 or more users act at the same time

### How does the user know if their message was received?
### How does the BE differentiate requests from different players?
### How does the BE differentiate from game to game?
If you don’t want to have accounts, whenever people start a game (lobby is a good idea), a lambda generates a UUID and store that locally, that’s their API key
Anonymous API keys -> UUID identifies the player
For games a lobby has it’s own UUID, I want to play a game with A, B and C and when a game started, the BE has to sign an ID for the game and the players has the id of the game so -> GameID/UserId

BE is the only one that can be thrusted to generate the UUIDs
Key -> game UUID and keep like a cache
Main questions and responsibilities are related to the lobby
How to tell the players that the game has started, maybe like a poll and then transition to the game state on the FE
Make a flow chart or list all the things that happen
Probably you’ll want to have a chat too
Do you need a DB to implement chat? Do people get chat history back? Maybe look for web socket chat
I’m gonna need AI to test on development -> maybe who starts the lobby the best way to make things cheap is to force clients instead of the server
You don’t have to worry about cheating if everything is private, just let the browser do it, offload some work to the browser, even if it’s possible to get the state from the FE to cheat
