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

[ARCH DIAGRAM HERE]

For the sake of having a solution that is cost-effective the chose architecture is:
* EventDriven: The UI of the game reacts to events from other players
* Serverless: Use ApiGateways + Lambda for game’s logic with API’s from a Rust library
* Track the game with a database that is consumable both from both from BE (update game state after applying logic) and FE (fetch game state after receiving notifications)

## Infrastructure choices:

The componentes for this projects are:
1. A cross-platform frontend GraphQL Apollo client 
1. An ApiGateway as GraphQL server
1. Lambdas as operations
1. S3 as a game state trackers
1. S3 events to inform the FE of game state changes

### 1. A cross-platform frontend GraphQL Apollo client 

In order to develop a single front end available for mobile app and web browser. The main options here are:
* [Yew](https://yew.rs/) in accordance of the first requirement of this project (to be rust-based). This framework allows to create applications with WebAssembly that may target mobile platforms (additional research is needed to confirm this and find out it's capabilities)
* [React native](https://reactnative.dev/) is primarily focused on mobile but can be easily extended to web applications. Support for this option is widely available and many features could be obtained from libraries instead of creating it from scratch. Also may be the best in terms of how easy it is to develop. The main con is that it leaves all the FE work in Typescript/Javascript instead of Rust.

Because of the goal number 2 of keeping the costs as low as possible, it is desired to move a significant amount of "work" into the client's side to "keep the BE frugal"

The responsibility of the FE will be:
[TBD]

Also the FE will interact with the BE by calling to it's post operations. The BE will let the FE know that an update has been made to the game state, so that the FE queries for the game state to update it to the client. To receive updates for the BE, there are 2 options:
* Keep a web socket connection 
* Use Server-Sent events

## 2 ApiGateway as GraphQL server

This ApiGatway will call to the proper operation or retrieve data directly to 
