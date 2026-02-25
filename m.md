```mermaid
    stateDiagram-v2
        [*] --> Lobby: Player clicks Invite Link
        Lobby --> Matchmaking: Check Elo & Game Mode
        Matchmaking --> GameStart: Opponent Found
        
        state GameStart {
            [*] --> PlayerA_Turn
            
            PlayerA_Turn --> ValidateMoveA: A clicks cell
            ValidateMoveA --> PlayerA_Turn: Invalid Move
            ValidateMoveA --> UpdateState: Valid Move
            
            UpdateState --> CheckWinCondition
            
            CheckWinCondition --> DetermineNextGrid: Game Continues
            DetermineNextGrid --> PlayerB_Turn: Enforce grid or Autonomy
            
            PlayerB_Turn --> ValidateMoveB: B clicks cell
            ValidateMoveB --> PlayerB_Turn: Invalid Move
            ValidateMoveB --> UpdateState: Valid Move
        }
        
        CheckWinCondition --> GameOver: SuperBoard Won or Draw
        GameOver --> UpdateElo: Calculate points
        UpdateElo --> [*]
```

```mermaid
    stateDiagram-v2
        [*] --> Lobby: Player clicks Invite Link
        Lobby --> Matchmaking: Check Elo & Game Mode
        Matchmaking --> GameStart: Opponent Found
        
        state GameStart {
            [*] --> Player_Turn
            
            Player_Turn --> ValidateMove: Player clicks cell
            ValidateMove --> Player_Turn: Invalid Move
            ValidateMove --> UpdateState: Valid Move
            
            UpdateState --> EvaluateAccuracy: Run Minimax in bg
            EvaluateAccuracy --> CheckWinCondition: Save move delta
            
            CheckWinCondition --> DetermineNextGrid: Game Continues
            DetermineNextGrid --> Player_Turn: Swap Player
        }
        
        CheckWinCondition --> GameOver: SuperBoard Won or Draw
        GameOver --> CalculateAccuracy: Aggregate move deltas
        CalculateAccuracy --> UpdateElo: Apply Elo + Accuracy math
        UpdateElo --> [*]
```