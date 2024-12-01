# Table of contents
<!-- TOC -->
* [Overview](#overview)
  * [Goals](#goals)
  * [Game overview](#game-overview)
* [Proposal](#proposal)
  * [Users](#users)
  * [User stories](#user-stories)
  * [Architecture overview](#architecture-overview)
  * [Infrastructure choices:](#infrastructure-choices)
    * [1. A cross-platform frontend GraphQL Apollo client](#1-a-cross-platform-frontend-graphql-apollo-client-)
    * [2 ApiGateway as GraphQL server and Lambadas as operations](#2-apigateway-as-graphql-server-and-lambadas-as-operations)
    * [3. S3 as a game state tracker and S3 events to inform the FE of game state changes](#3-s3-as-a-game-state-tracker-and-s3-events-to-inform-the-fe-of-game-state-changes)
* [Design details](#design-details)
  * [Requirements](#requirements)
  * [Functional requirements](#functional-requirements)
  * [Non-fucntional requiremets](#non-fucntional-requiremets)
  * [Frontend](#frontend)
  * [Backend](#backend-)
  * [Events](#events)
* [Questions](#questions)
* [Notes](#notes)
<!-- TOC -->
# Overview

This application intents to develop [Doppelkopf](https://en.wikipedia.org/wiki/Doppelkopf) game to be played on mobile devices or web. The motivation for this project is to peform a **rust-based E2E(End-To-End)** project as an exercise.

[Doppelkopf](https://en.wikipedia.org/wiki/Doppelkopf) is a popular 4-player card game in Germany, but it's complicated to understand and it's hard to find a user friendly game online for non-german speakers that allows you to learn. 

For non-german speakers there's user friendly app or online game to learn how to play. There's plenty documentation online with the rules and you can find a few apps and websites. However, if you want to learn by playing, the websites and apps are in german only or have a tutorial that is unclear, leaving room for a lot of questions.

## Goals

1. **Main goal**: Make a game to entertain people.
1. Keep the cost as low as possible (ideally in the free tier of AWS). 
1. Try to have flexibility to allow the architecture to scale for some weird case where people actually wanting to use the game with an unexpected volume for which further billing solutions need to be developed.

## Game overview

![game_overview](https://drive.google.com/uc?export=view&id=1fq7aoRl1goi8oo3bYw82Q7YoS-LBFTDm)

Doppelkopf is a game in which 4 players. In a normal workflow:
* Dealer deals 12 cards to each player → Players agree to a game type (wedding or solo) → Each player, including the dealer, plays a trick (optionally make an announcement) → The game rules are applied to determine a winner and to determine the amount of earned points → The winner takes the cards (which won't be played again) and her team gets the points (determined by rules or verbally) → Repeat all the steps until there is a winner.

For a more detailed explanation of the game, check the [README of this project](https://github.com/Rbatistab/dopplekopf-cdk/tree/main)

# Proposal

## Users
* Any person with access to a mobile device or a computer with a browser who wants to play a game

## User stories
* As a player, I will be able to create a new game
* As a player, I will be able to join a new game
* As a player, I will be able to make an contract/auction at the start of a game to agree a game type (wedding, solo, etc)
* As a player, I will be able to deal cards
* As a player, I will be able to play a trick
* As a player, I will be able to optionally make a bid on my turn of playing a trick
* As a player, the game won't allow me to do an invalid play
* As a player, I will be able to see the winner, and the amoun of points of each player after a round

## Architecture overview

![architecture_diagram](https://drive.google.com/uc?export=view&id=1Ym_weEHZ5I-QC4gbxL8x4Zu5TIc0yQyI)

For the sake of having a solution that is cost-effective the chosen architecture is:
* EventDriven: The UI of the game reacts to events from other players
* Serverless: Use ApiGateways + Lambda for game’s logic with API’s from a Rust library and Server-Sent events to update the FE clients
* Track the game with a database that is consumable both from both from BE (update game state after applying logic) and FE (fetch game state after receiving notifications)

## Infrastructure choices:

The componentes for this projects are:
1. A cross-platform frontend app
1. A REST ApiGateway
1. S3 as a game state tracker and S3 events to inform the FE of game state changes

### 1. A cross-platform frontend app 

In order to develop a single front end available for mobile app and web browser. The main options here are:
* [Yew](https://yew.rs/) in accordance of the first requirement of this project (to be rust-based). This framework allows to create applications with WebAssembly that may target mobile platforms (additional research is needed to confirm this and find out it's capabilities)
    * Most mature and widely-used
    * JSX-like template syntax
    * React-like (historically Elm-like struct components, now React-like function components)
    * More boilerplate for things like event callbacks, need to `.clone()` things into closures, etc.
    * Relatively good performance (think something like Preact/Vue, faster than React)
    * Safe Rust
    * Medium WASM binary sizes
    * Community-maintained
* [Leptos](https://leptos.dev/) (also rust-based). Full-stack framework for building web applications in Rust
    * Relatively new, fast moving
    * Based on SolidJS/fine-grained reactivity, with no virtual DOM overhead
    * JSX-like template syntax
    * Entirely safe Rust
    * Very, very good performance; outcompetes others listed here not only on rendering speed but on memory usage and on server-rendering speed
    * Small WASM binary sizes (slightly larger than Sycamore)
    * Fastest server rendering and strong emphasis on/support for integrations with the server, including multiple server-rendering modes (like both in-order and out-of-order HTML streaming)
* [React native](https://reactnative.dev/) is primarily focused on mobile but can be easily extended to web applications. Support for this option is widely available and many features could be obtained from libraries instead of creating it from scratch. Also may be the best in terms of how easy it is to develop. The main con is that it leaves all the FE work in Typescript/Javascript instead of Rust.



### 2 A REST ApiGateway 

Regular REST endpoints handled by an API Gateaway that either calls a lambda for processing or to directly proxy a query to the database. Operations will be Rust lambdas that will execute game logic and updata the game state. 

### 3. S3 as a game state tracker and S3 events to inform the FE of game state changes

S3 update events will trigger an event update lambda that will "push" (I need a better term here) the event through an ApiGateway that serves as Server-Sent-Event server which the FE will be listening.

# Design details

## Requirements

## Functional requirements
* The app should allow a user to create a new game
* The app should allow a user to join a new game and restric players to only four per game
* The frontend should communicate with the backend by events
* ...

## Non-fucntional requiremets
* The project should use [Rust](https://www.rust-lang.org/) as the main language
* The cost should ideally be on AWS free tier
*  ...

## Frontend

[IN PROGRESS]
Because of the goal number 2 of keeping the costs as low as possible, it is desired to move a significant amount of "work" into the client's side to "keep the BE frugal". The responsibility of the FE will be:
* Render a main screen for the user to begin a new game or join a new game
* Render the game to the user
* Generate and id for the current user
* Call the BE API's after every user action that generates an update on the game state (only `POST` operations)
* Listen to the BE for events to update the game state (a single `GET` operation)
* Inform the users about results from the game state (ex. a winner team, update the scores, etc.)

To receive updates for the BE, there are 2 options:
* Use Server-Sent events (dive deep in progress)
* Keep a web socket connection (dive deep in progress)

## Backend 

[IN PROGRESS]
BE is pretty straigtforward as it will have an ApiGateway, `<NAME>`, as apollo server for GraphQL 

The operations will live in project [doppelkopf-rs](https://github.com/Rbatistab/doppelkopf-rs) which will follow a structure like this:
```
├── Cargo.lock
├── Cargo.toml
├── operation1/
├── operation2/
│   ├── Cargo.toml
│   └── src
│       └── main.rs
...
└── target

```

Operations are:
1. `POST/get_new_game_id`: Creates a new game and stores it on the database. Also will create a new UUID for the player creating the game and return it to the FE. (All UUIDs are Backend-generated).
1. `POST/join_new_game/game_id`: Takes the `game_id` to add the user to the game (after player count validation)
1. `POST/deal_cards/game_id`: Takes a `user_id` from the FE and the `game_id` to update the game state to a representation in which that player dealt the cards.
1. `POST/make_announcement/game_id`: Takes a `user_id` from the FE and the `game_id` to update the game state to a representation of the player announcement
1. `POST/play_trick/game_id`: Takes a `user_id` from the FE and the `game_id` to evaluate the rules from a representation of the play of the trick. Always will execute the rules against the card. If the move is valid it will update the game state
1. **`GET/get_game_state`**: (Assumption that is possible) This operation has no lambda, the ApiGateway fetches the game state directly from S3.

## Events
Once the FE get's notification it will make a call to `GET/get_game_state`.
[IN PROGRESS]

# Questions

* [Questions](https://github.com/Rbatistab/dopplekopf-cdk/blob/main/docs/QUESTIONS.md)

# Notes
* [Notes](https://github.com/Rbatistab/dopplekopf-cdk/blob/main/docs/NOTES.md)
