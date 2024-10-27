# Table of Contents
<!-- TOC -->
* [dopplekopf-cdk](#dopplekopf-cdk)
    * [Problem](#problem)
    * [Solution](#solution)
* [Game Overview](#game-overview)
    * [1. Card deal and contract](#1-card-deal-and-contract)
    * [2. Contract](#2-contract)
        * [2.1 Normal game](#21-normal-game)
        * [2.2 Wedding](#22-wedding)
        * [2.3 Solo](#23-solo)
    * [3. Play tricks and bids](#3-play-tricks-and-bids)
        * [3.1 Play a trick](#31-play-a-trick)
        * [3.2 Bids](#32-bids)
    * [4. Evaluate Rules (Scoring)](#4-evaluate-rules-scoring)
        * [4.1 Trumps](#41-trumps)
        * [4.2 Special features (extra points)](#42-special-features-extra-points)
        * [4.3 Scoring](#43-scoring)
<!-- TOC --># Game Overview

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


![game_overview](https://drive.google.com/uc?export=view&id=1fq7aoRl1goi8oo3bYw82Q7YoS-LBFTDm)

## 1. Card deal and contract

![first_step_card_deal_and_contract](https://drive.google.com/uc?export=view&id=15rBJbUubBfbccd2OJ7pha5I9s-Dr5Ql6)

On the standard game, you'll have to deal 2 decks (red and black) of 24 (*or 20*) cards each for a total of 48 (*or 40*) cards. Remove all cards with rank lower to 9 to have 48 cards, consisting of:
* 4 Suites (French or [German](https://en.wikipedia.org/wiki/German-suited_playing_cards) deck):
  * ♦ Diamonds (*or Bells 🇩🇪*)
  * ❤ Hearts (*german and french*)
  * ♠ Spades (*or Leaves 🇩🇪*)
  * ♣ Clubs (*or Acorns 🇩🇪*)
* Each suit has 6 possible ranks (2 cards of each suit per rank or *'Doppelkopf'*):
***If you want to play with 40 cards just remove the nines.***

Start by dealing 12 cards to each player (*10 for 40-card games*).The dealer is the first player to deal, and dealing passes clockwise.

## 2. Contract

After this, you'll need to agree a contract, also known as auction. In this phase, the players either agree to a [normal-game](#normal_game) by saying "*Fine*" (*Gesund 🇩🇪*) or one or more of the players call "*Hold*" (*Halt 🇩🇪*) to call a [wedding](#wedding) or a [solo](#solo). 

***Always two there are, no more, no less.*** Once the contract is set, there will be two teams of 2 vs 2 or 1 vs 3.

### 2.1 Normal game

When 2 different players have a Queens of Clubs (or *Obers* 🇩🇪, known as *Die Alten*, the elders") they are a team (**Re**) and will play against the other 2 (**Kontra**):
* `Re` team achieves at least 121 points (*or a different pre-agreed condition*) -> `Re` wins
* `Re` fails to get the points -> `Kontra` wins

### 2.2 Wedding

A player (*suitor*) that has both Queens of Clubs can call a "wedding" and form a **Re** team with the first player to win a trick. However, if the *suitor* wons the first three games, he/she should play a *Diamond [solo](#solo)* against the other three. Also, a *suitor* can chose not to say "wedding" on the contract and play a *Diamond [solo](#solo)* too, called *Silent Solo* (*Stilles Solo 🇩🇪*).

### 2.3 Solo

***May you ride eternal, shiny and chrome.*** A single player will play against the other three. He/She will earn trice the value of the score card in case of winning a trick or lose trice the value on the other case, there are:
* Diamond Solo: Only Diamonds make a Trump 
* Jack Solo (*Bubensolo 🇩🇪*): Only Jacks make a Trump
* Queen Solo (*Damensolo 🇩🇪*): Only Queen make a Trump
* Ace Solo (*Fleischloser/Knochenmann 🇩🇪*): There are no Trumps
* Suit Solo (*Farbensolo 🇩🇪*): Announce a Suit to be a Trump for Jacks and Queens

## 3. Play tricks and bids

[IMAGE IN PROGRESS]

Doppelkopf is won by playing tricks to score points in a team. **The default is to win a round by scoring 121 points or more**, however it's possible to pre-agree a different score or a number of rounds.

Points are scored based on the number and value of the tricks won. Some specific card combinations or achievements can also earn extra points.

### 3.1 Play a trick

The game flows like this:

1. The player to the left of the dealer plays the first trick (normally the highest at hand)
1. Players must follow suit if possible. There are 3 options:
    * Play a trump
    * Play any card
    * Discard 
1. After all tricks are plyed the round is won by:
    1. The highest trump 
    1. The highest card of the suit led. Since each card exists twice, there is the possibility of a tie; in that case, the first-played card wins the trick

### 3.2 Bids

A player can complement the trick with a bid or announcement. The bid claims that their team will accomplish certain goal. Bids increase the game value regardless of whether they are fulfilled. By failing a self-given goal, the team automatically lose.

The bids, in order, are:
1. "Re"/"Contra": The name of the player's team, claims that the team will make more than 120 points. 
and undertaking that their team will score more than 120 points. ***All following bids should follow this bid, this is the first valid bid*** => Doubles the value
    * Can only be played with 11 cards left 
1. "No 90" (also called "No 9"): Claims the opponents will get less than 90 points => Triples the value
    * Can only be played with 10 cards left 
1.  "No 60" (also called "No 6"): Claims the opposing team will not make 60 points => X4 the value
    * Can only be played with 9 cards left 
1. "No 30" (also called "No 3": You get the idea
    * Can only be played with 8 cards left 
1. "Schwarz": Claims the opponents will not get a single trick, not even a trick worth zero points
    * Can only be played with 7 cards left 

The fun part here is that each bid will imply all previous: "No 6" will imply "No 9" which will imply "Re/Contra", hence the game increases by 4.
Also, every bid can have a counterbid making both valid.

## 4. Evaluate Rules (Scoring)

The game rules will determine the winner of the round and the amount of points that each team wins or loses.


### 4.1 Trumps

**Normal game:** Starts with the 10 of hearts, called *Dulle* (or *Tolle* 🇩🇪):

❤️ 10 | ♣️ Q | ♠️ Q | ❤️ Q | ♦️ Q | ♣️ J | ♠️ J | ❤️ J | ♦️ J | ♦️ A | ♦️ 10 | ♦️ K | ♦️ 9

**Wedding:** Same Trump as the [normal game](#normal-game)

**Solos:**
* Diamond Solo: Same Trump as the [normal game](#normal-game)
* Jack Solo (*Bubensolo 🇩🇪*): Only Jacks make a Trump
* Queen Solo (*Damensolo 🇩🇪*): Only Queen make a Trump
* Ace Solo (*Fleischloser/Knochenmann 🇩🇪*): There are no Trumps
* Suit Solo (*Farbensolo 🇩🇪*): Announce a Suit to be a Trump for Jacks and Queens

### 4.2 Special features (extra points)

***There are no extra points for Solo games***

* **Doppelkopf**: Get an extra point if a trick has 40 or more points
* **Charlie Miller**: Get an extra point if a team's jack of clubs wins the last trick

### 4.3 Scoring

Game score and round winner:
1. The player that won gets all the cards, which won't be played again. The card points taken in the tricks are counted and each player in the winning team gets the game points added to their score, while the losing players have that value deducted.
1. The winner of the trick leads the next one 

    | Card | Points per card |
    |------|-----------------|
    | Aces (*Deuces*) | 11 |
    | Tens | 10 |
    | Kings | 4 |
    | Queens (*Obers*) | 3 |
    | Jacks (*Unters*) | 2 |
    | Nines | 0 |

Score of each player:

* Add game value to score of each player of the wininng team
* Substract game value to score of each player of the losing team
* If a Solo wins she gets thrice added/substracted (ouch!)

References:
* [Wikipedia](https://en.wikipedia.org/wiki/Doppelkopf)
* https://www.pagat.com/schafkopf/doko.html
* https://boardgamegeek.com/blogpost/127676/doppelkopf-20-a-brilliant-traditional-trick-taker
* https://www.deck-of-cards.com/doppelkopf.html
