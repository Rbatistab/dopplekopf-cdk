* If you don’t want to have accounts, whenever people start a game (lobby is a good idea), a lambda generates a UUID and store that locally, that’s their API key
* Anonymous API keys -> UUID identifies the player
* For games a lobby has it’s own UUID, I want to play a game with A, B and C and when a game started, the BE has to sign an ID for the game and the players has the id of the game so -> GameID/UserId
* BE is the only one that can be thrusted to generate the UUIDs
* Key -> game UUID and keep like a cache
* Main questions and responsibilities are related to the lobby
* Make a flow chart or list all the things that happen
* Probably you’ll want to have a chat too
* I’m gonna need AI to test on development -> maybe who starts the lobby the best way to make things cheap is to force clients instead of the server
You don’t have to worry about cheating if everything is private, just let the browser do it, offload some work to the browser, even if it’s possible to get the state from the FE to cheat

