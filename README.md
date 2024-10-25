# dopplekopf-cdk

CDK for Doppelkopf whole game:
* Infrasctrucre: [dopplekopf-cdk](https://github.com/Rbatistab/dopplekopf-cdk/tree/main)
* Frontend application: [dopplekopf-app](https://github.com/Rbatistab/doppelkopf-app)
* Backend: [dopplekopf-rs](https://github.com/Rbatistab/doppelkopf-rs)

## Problem

This application intents to develop a card game, [Doppelkopf](https://en.wikipedia.org/wiki/Doppelkopf), for 4 players that can be played on mobile devices or web. This is an End-to-End exercise given the fact that doing E2E is a valuable skill which, on my personal context, I don't get to do as often as I'd whish to. 

The main goal is to make a game to entertain people. This system should be the cheapest possible in terms of money (not to say totally free), but should be able to scale infrastructure in case of some weird case of people actually wanting to use the game with an unexpected volume for which further billing solutions need to be developed

The second goal is to do it with [Rust](https://www.rust-lang.org/).

## Solution

Design and develop a card game for web browser and mobile that has at least a backend in rust and has dive deep, definition and discussion of the architecture, infrastructure, design and technologies chosen. This process should get feedback and reviews along the way. And will rely on an iterative approach.

* See [docs/ARCHITECTURE_AND_DESIGN.md](https://github.com/Rbatistab/dopplekopf-cdk/blob/main/docs/ARCHITECTURE_AND_DESIGN.md) for details.
* See [docs/TODO.md](https://github.com/Rbatistab/dopplekopf-cdk/blob/main/docs/TODO.md) for progress.

# Game Overview

![game_overview](https://drive.google.com/uc?export=view&id=17qDfg14eofoHQsIuZk-Pci7aqCvqct63)

## Card deal and contract

![first_step_card_deal_and_contract](https://drive.google.com/uc?export=view&id=15rBJbUubBfbccd2OJ7pha5I9s-Dr5Ql6)
