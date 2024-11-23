# Minesweeper Game

A simple Minesweeper game built with **Svelte**.

## Features

- Customizable grid size and number of mines.
- Responsive design for an enjoyable gaming experience.
- Classic Minesweeper rules:
  - Reveal a cell to see its adjacent mines.
  - Avoid clicking on mines (💣).
  - Clear the board to win!

## Getting Started

### Prerequisites

- Node.js (v16 or later)
- npm or yarn

### Installation

1. Clone the repository:

   ```bash
   git clone https://github.com/your-username/minesweeper-svelte.git
   cd minesweeper-svelte


2. Install dependencies:

   ```bash
    npm install

3. Start the development server:

   ```bash
    npm run dev
## Project Structure

```bash
    /src
        App.svelte          
        /components
            Cell.svelte     
```
### How to Play


1. Reveal a Cell

2. Click on a cell to reveal it.
If it contains a mine, you lose!
Safe Cells

3. If the cell is safe, you'll see the number of adjacent mines.
Winning

4. Clear the entire board without hitting any mines to win the game! mines.
Clear the board without hitting a mine to win.