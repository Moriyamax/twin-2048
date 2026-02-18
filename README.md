# Twin-2048: Human vs AI

[Play Online](https://Moriyamax.github.io/twin-2048/Twin2048gameVS_CPU.html)

A 2048 variant where you control two boards simultaneously against a CPU opponent. Players alternate turns — whoever traps a board first loses.

## Demo

Just open the HTML file in any browser. No dependencies, no build step required.

## Rules

- Two 4×4 boards exist, and **every move is applied to both boards simultaneously**
- If **either board gets stuck on your turn, you lose immediately**
- **Creating a tile of 32 or higher earns a bonus turn** — you get one extra move
- Bonus turns can chain: if you make another 32+ tile during a bonus turn, you keep going
- But **if your board gets trapped during a chain, you still lose** — the more aggressively you attack, the more you risk trapping yourself
- Controls: arrow keys or swipe

## CPU Design

At the start of each game, the CPU is randomly assigned one of three personality types. The type is never shown in the UI — curious players can check the browser console (F12).

| Type | Probability | Lookahead Depth | Behavior |
|------|-------------|-----------------|----------|
| Normal | 75% | 6 moves | Standard difficulty |
| Strong | 20% | 12 moves | Consistently strong from the start |
| Hidden | 5% | 6 moves normally / 30 moves in endgame | Appears weak until the total empty cells across both boards drops to 12 or fewer, then increases lookahead by 5× |

The CPU uses Monte Carlo simulation (500 rollouts per direction) to evaluate survival probability for each possible move.

Additionally, **every 10th reset is guaranteed to produce a Hidden-type CPU**, regardless of the random draw.

## Technical Notes

**Zero-dependency single file**  
Pure HTML/CSS/JS. No frameworks, no build tools.

**Chaining bonus turns**  
Bonus turn chaining works by keeping the `isPlayerTurn` flag unchanged and re-invoking `executeStep` for the same player. CPU bonus turns use a 900ms delay vs. 600ms for normal turn transitions, giving a subtle visual distinction.

**Asymmetric information design**  
The CPU type is logged to the browser console only — never shown in the UI. The Hidden type is intentionally indistinguishable from Normal until late game, creating an element of uncertainty for the player.

**Animation without CSS transitions**  
Slide and merge-swell animations are implemented via `requestAnimationFrame` with manual interpolation, avoiding CSS transition overhead.

## Controls

| Input | Action |
|-------|--------|
| Arrow keys ↑↓←→ | Move both boards |
| Swipe | Move both boards (mobile) |
| Reset Game | Restart and re-roll CPU type |

## File Structure

```
Twin2048gameVS_CPU.html   // Entire game in a single file
```

## License

MIT

##
