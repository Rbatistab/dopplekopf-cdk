# Game overview

![game_overview](https://drive.google.com/uc?export=view&id=10djvgmbEifvyU-RYfDERHeuBATuP8i0l)

# Architecture overview

Doppelkopf is a game in which 4 players. In a normal workflow:
* Dealer deals 12 cards to each player → Each player plays a trick (optionally make an announcement) → The game rules are applied to determine a winner and to determine the amount of earned points → The winner player and the points are added to the team (determined by rules or verbally) → Repeat until there is a winner.

For the sake of having a solution that is cost-effective the chose architecture is:
* EventDriven: The UI of the game reacts to events from other players
* Serverless: Use ApiGateways + Lambda for game’s logic with API’s from a Rust library
* Track the game with a database that is consumable both from both from BE (update game state after applying logic) and FE (fetch game state after receiving notifications)

## Infrastructure choices:
TBD
