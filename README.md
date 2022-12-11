# Simple RPS Game with MUD

This project demonstrates a MUD and ECS model implementation of the classic Rock-Paper-Scissors game.

# Game Structure (Contract-Side): 

1. Entities:

- Player:
  - player of the game. Each player component is identified by their wallet address.

2. Components:

- MoveSet:
  - stores player move set: { string: rock, paper, scissors | default = rock}.
- Hearts: 
  - stores number of hearts left during a game of RPS: { int: default = 3}.
- Wins:
  - stores number of total wins: {int: default = 0}.

- GameStatus:
  - stores number to denote status of player in terms of matchmaking:
    {
      0: offline
      1: looking for match
      2: currently in a match
    }

- ReadyToPlay:
  - stores a list of Player entities that are currently looking for a match (aka GameStatus = 1).

- CurrentOpponent:
  - stores address of Player entity that user is going to play against.

3. Systems:

- Request(): Player sends request to match up with another player to start a game of rps:
  - changes GameStatus from 0 -> 1.
  - adds user Player address to ReadyToPlay list.
  - calls Matchmake()

- Matchmake(): pairs Players randomly to start a game:
  - searches through ReadyToPlay list and randomly selects a Player to match with.
  - sends waiting player an invite to accept game.

- Accept(): accepts game request:
  - after Matchmake(), opponent receives request and must call Accept() to start the game.
  - sets Player and opponent GameStatus = 2.
  - removes Players from ReadyToPlay list?
  - sets CurrentOpponent to opponent Player entity address (aka wallet address) for both players
  - calls StartGame().

- StartGame(): starts a game of RPS against the two players:
  - reset MoveSet to rock (default).
  - reset Hearts to 3 (default).
  - requests players to select a move.
  