# Overview

[Doppelkopf](https://en.wikipedia.org/wiki/Doppelkopf) is a popular 4-player card game in Germany, but it's a bit complicated, and sadly, it's very hard to find a user friendly game online for non-german speakers that allows you to learn. You can find a few apps and websites that, in my experience, are in german or have a tutorial that is unclear and with room for a lot of questions.

This application intents to develop [Doppelkopf](https://en.wikipedia.org/wiki/Doppelkopf) game to be played on mobile devices or web. The motivation for this project is to peform a rust-based E2E(End-To-End) project as an exercise.

1. **Main goal**: Make a game to entertain people.
1. Keep the cost as low as possible (ideally in the free tier of AWS). 
1. Try to have flexibility to allow the architecture to scale in case of some weird case of people actually wanting to use the game with an unexpected volume for which further billing solutions need to be developed

## Game overview

![game_overview](https://drive.google.com/uc?export=view&id=1mwXmJg7Wf5M09esn7J_9TTux8-zYyMJh)

Doppelkopf is a game in which 4 players. In a normal workflow:
* Dealer deals 12 cards to each player → Each player plays a trick (optionally make an announcement) → The game rules are applied to determine a winner and to determine the amount of earned points → The winner player and the points are added to the team (determined by rules or verbally) → Repeat until there is a winner.

References:
* [Wikipedia](https://en.wikipedia.org/wiki/Doppelkopf)
* https://www.pagat.com/schafkopf/doko.html
* https://boardgamegeek.com/blogpost/127676/doppelkopf-20-a-brilliant-traditional-trick-taker
* https://www.deck-of-cards.com/doppelkopf.html

## Requirements

* The project should use [Rust](https://www.rust-lang.org/) as the main language
* ....

# Architecture overview

![architecture_diagram](https://drive.google.com/uc?export=view&id=1TiFBRmeEne3J1EG_Q1Jk0Z8-5pHftzMI)

For the sake of having a solution that is cost-effective the chose architecture is:
* EventDriven: The UI of the game reacts to events from other players
* Serverless: Use ApiGateways + Lambda for game’s logic with API’s from a Rust library
* Track the game with a database that is consumable both from both from BE (update game state after applying logic) and FE (fetch game state after receiving notifications)

## Infrastructure choices:

The componentes for this projects are:
1. A cross-platform frontend GraphQL Apollo client 
1. An ApiGateway as GraphQL server and Lambdas as operations
1. S3 as a game state tracker and S3 events to inform the FE of game state changes

### 1. A cross-platform frontend GraphQL Apollo client 

In order to develop a single front end available for mobile app and web browser. The main options here are:
* [Yew](https://yew.rs/) in accordance of the first requirement of this project (to be rust-based). This framework allows to create applications with WebAssembly that may target mobile platforms (additional research is needed to confirm this and find out it's capabilities)
* [React native](https://reactnative.dev/) is primarily focused on mobile but can be easily extended to web applications. Support for this option is widely available and many features could be obtained from libraries instead of creating it from scratch. Also may be the best in terms of how easy it is to develop. The main con is that it leaves all the FE work in Typescript/Javascript instead of Rust.

Because of the goal number 2 of keeping the costs as low as possible, it is desired to move a significant amount of "work" into the client's side to "keep the BE frugal"

The responsibility of the FE will be:
* Render a main screen for the user to begin a new game or join a new game
* Render the game to the user
* Generate and id for the current user
* Call the BE API's after every user action that generates an update on the game state (only `POST` operations)
* Listen to the BE for events to update the game state (a single `GET` operation)
* Inform the users about results from the game state (ex. a winner team, update the scores, etc.)

To receive updates for the BE, there are 2 options:
* Use Server-Sent events (dive deep in progress)
* Keep a web socket connection (dive deep in progress)

### 2 ApiGateway as GraphQL server and Lambadas as operations

This ApiGatway will act as a GraphQL server to call to the proper operation. Operations will be Rust lambdas that will execute game logic and updata the game state. Operations are:
1. `POST/get_new_game_id`: Creates a new game and stores it on the database. Also will create a new UUID for the player creating the game and return it to the FE. (All UUIDs are Backend-generated).
1. `POST/join_new_game/game_id`: Takes the `game_id` to add the user to the game (after player count validation)
1. `POST/deal_cards/game_id`: Takes a `user_id` from the FE and the `game_id` to update the game state to a representation in which that player dealt the cards.
1. `POST/make_announcement/game_id`: Takes a `user_id` from the FE and the `game_id` to update the game state to a representation of the player announcement
1. `POST/play_trick/game_id`: Takes a `user_id` from the FE and the `game_id` to evaluate the rules from a representation of the play of the trick. Always will execute the rules against the card. If the move is valid it will update the game state
1. **`GET/get_game_state`**: (Assumption that is possible) This operation has no lambda, the ApiGateway fetches the game state directly from S3.

### 3. S3 as a game state tracker and S3 events to inform the FE of game state changes

S3 update events will trigger an event update lambda that will "push" (I need a better term here) the event through an ApiGateway that serves as Server-Sent-Event server which the FE will be listening.
Once the FE get's notification it will make a call to `GET/get_game_state`.

# User stories and requiremets
[TBD]
## User stories
[TBD]
## Functional requirements
[TBD]
## Non-fucntional requiremets
[TBD]

# Questions

* [Questions](https://github.com/Rbatistab/dopplekopf-cdk/blob/main/docs/QUESTIONS.md)


### In progress stuff and notes (Don't read, draft content)

If you don’t want to have accounts, whenever people start a game (lobby is a good idea), a lambda generates a UUID and store that locally, that’s their API key
Anonymous API keys -> UUID identifies the player
For games a lobby has it’s own UUID, I want to play a game with A, B and C and when a game started, the BE has to sign an ID for the game and the players has the id of the game so -> GameID/UserId

BE is the only one that can be thrusted to generate the UUIDs
Key -> game UUID and keep like a cache
Main questions and responsibilities are related to the lobby
How to tell the players that the game has started, maybe like a poll and then transition to the game state on the FE
Make a flow chart or list all the things that happen
Probably you’ll want to have a chat too

I’m gonna need AI to test on development -> maybe who starts the lobby the best way to make things cheap is to force clients instead of the server
You don’t have to worry about cheating if everything is private, just let the browser do it, offload some work to the browser, even if it’s possible to get the state from the FE to cheat

