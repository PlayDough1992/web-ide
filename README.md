# PD-WebDev: In-Browser Web IDE

PD-WebDev is a modern, single-file, in-browser web development IDE featuring a Monaco Editor core, tabbed editing, and a session-based virtual file system. It is designed for rapid prototyping, teaching, and web development directly in your browser—no installation required.

## Features

### 🗂️ File Explorer
- **Create and delete files/folders** via a modern sidebar.
- **In-memory file system**: All files and folders exist in your browser session (no server required).
- **Download** any file or folder (folders as .zip archives).
- **Move** files and folders using a two-pane modal for intuitive organization.

### 📝 Monaco Editor
- **Tabbed editing**: Open multiple files at once, switch between them easily.
- **Syntax highlighting** for HTML, CSS, JS, TypeScript, Python, Markdown, and more.
- **Auto-detects language** based on file extension.
- **Modern UI**: Clean, dark theme.

### ▶️ Run Web Files
- **Run HTML files** in a built-in webview tab (iframe) with a single click.
- **Inlines CSS and JS**: When you run an HTML file, any linked CSS/JS files from your project are automatically inlined for accurate preview.

### 📦 Download
- **Download individual files** as the extension they are in PD-WebDev.
- **Download folders** as .zip archives, preserving the folder structure.
- **Project root** is zipped as a single top-level folder for easy extraction.

### 🖼️ Modern UI/UX
- **Modern design**: Subtle blurs, rounded corners, and vibrant accent colors.
- **Responsive layout**: Works on desktops and large tablets.
- **Accessible modals** for file/folder creation and moving.

## Getting Started
1. Open [PD-WebDev](https://playdough1992.github.io/web-ide/) in your browser.
2. Use the sidebar to create files/folders, edit code, and organize your project.
3. Click the green ▶️ button to run your HTML file in a new tab within the IDE.
4. Download files or folders as needed.

## Dependencies
- [Monaco Editor](https://microsoft.github.io/monaco-editor/) (CDN) for the editor.
- [JSZip](https://stuk.github.io/jszip/) (CDN, loaded on demand for zipping)

## Limitations
- All files/folders are stored in memory and will be lost when you close or refresh the browser tab, UNLESS YOU DOWNLOAD THEM.
- No server-side features; everything runs client-side in your browser.

## License
MIT License. Free for personal and educational use.

---

Enjoy fast, distraction-free web development—right in your browser!


### 📝 Example Simple Pong Game

## Create these directories and files in PD-WebDev and click run!

## The game should look like this:

<img width="873" height="662" alt="image" src="https://github.com/user-attachments/assets/0f77b1da-2cfe-4cf2-8e9f-c18ec664751d" />


## HTML/index.html
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Awesome Pong Game</title>
    <link rel="stylesheet" href="Styles/styles.css">
</head>
<body>
    <div id="game-container">
        <h1>Awesome Pong</h1>

        <div id="game-info">
            <div id="score-display">
                <p>Player Score: <span id="player-score">0</span></p>
                <p>AI Score: <span id="ai-score">0</span></p>
            </div>

            <div id="user-controls">
                <button id="start-game-btn">Start Game</button>
            </div>

        </div>

        <canvas id="gameCanvas" width="800" height="400"></canvas>

    </div>

    <script src="Scripts/script.js"></script>
</body>
</html>
```

## Styles/styles.css
```css
body {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    background-color: #2c3e50; /* Dark blue background */
    color: #ecf0f1; /* Light text */
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 100vh;
    margin: 0;
    padding: 20px;
    box-sizing: border-box;
}

#game-container {
    background-color: #34495e; /* Slightly lighter dark blue */
    padding: 30px;
    border-radius: 10px;
    box-shadow: 0 10px 20px rgba(0, 0, 0, 0.3);
    text-align: center;
    max-width: 900px;
    width: 100%;
}

h1 {
    color: #e74c3c; /* Reddish accent */
    margin-bottom: 20px;
    font-size: 2.5em;
}

#game-info {
    display: flex;
    justify-content: space-around;
    align-items: center;
    margin-bottom: 20px;
    flex-wrap: wrap;
    gap: 15px;
}

#score-display p {
    margin: 5px 0;
    font-size: 1.1em;
}

#user-controls label {
    margin-right: 10px;
}


#start-game-btn, .difficulty-btn {
    background-color: #2ecc71; /* Green accent */
    color: white;
    border: none;
    padding: 10px 15px;
    border-radius: 5px;
    cursor: pointer;
    font-size: 1em;
    transition: background-color 0.3s ease;
    margin: 5px;
}

#start-game-btn:hover, .difficulty-btn:hover {
    background-color: #27ae60;
}

#difficulty-selection p {
    margin-bottom: 5px;
    font-weight: bold;
}

#gameCanvas {
    background-color: #1a252f; /* Even darker for the canvas */
    border: 2px solid #e74c3c;
    display: block;
}
```

## Scripts/script.js
```js
document.addEventListener('DOMContentLoaded', () => {
    const canvas = document.getElementById('gameCanvas');
    const ctx = canvas.getContext('2d');
    const playerScoreDisplay = document.getElementById('player-score');
    const aiScoreDisplay = document.getElementById('ai-score');
    
    // Set fixed dimensions for clarity
    const GAME_WIDTH = canvas.width;
    const GAME_HEIGHT = canvas.height;
    
    // --- 1. Game Constants ---
    const PADDLE_WIDTH = 15;
    const PADDLE_HEIGHT = 75;
    const PADDLE_RADIUS = 7; // NEW: Radius for rounded corners
    const BALL_SIZE = 10;
    const WINNING_SCORE = 5;

    // --- 2. Game State Variables ---
    let player1Score = 0;
    let player2Score = 0;
    let gameRunning = false;

    // Player Paddle (controlled by user)
    let playerPaddle = {
        x: 10,
        y: GAME_HEIGHT / 2 - PADDLE_HEIGHT / 2,
        dy: 0 // change in y position (for movement)
    };

    // AI Paddle (opponent)
    let aiPaddle = {
        x: GAME_WIDTH - PADDLE_WIDTH - 10,
        y: GAME_HEIGHT / 2 - PADDLE_HEIGHT / 2,
        dy: 0
    };
    
    // Ball
    let ball = {
        x: GAME_WIDTH / 2,
        y: GAME_HEIGHT / 2,
        dx: 5, // initial horizontal speed
        dy: 5  // initial vertical speed
    };

    // --- 3. Core Game Logic Functions ---

    // Resets the ball to the center after a score
    function resetBall() {
        ball.x = GAME_WIDTH / 2;
        ball.y = GAME_HEIGHT / 2;
        
        // Reverse the horizontal direction to serve towards the player who was scored against
        ball.dx = -ball.dx; 
        
        // Give the ball a new, random vertical direction
        ball.dy = (Math.random() < 0.5 ? -1 : 1) * (Math.random() * 4 + 3);
    }
    
    // Checks for a winner and stops the game
    function checkWin() {
        if (player1Score >= WINNING_SCORE) {
            alert("Player Wins! 🎉 Click OK to restart.");
            resetGame();
        } else if (player2Score >= WINNING_SCORE) {
            alert("AI Wins! 😥 Click OK to restart.");
            resetGame();
        }
    }

    // Resets all scores and positions
    function resetGame() {
        player1Score = 0;
        player2Score = 0;
        playerScoreDisplay.textContent = player1Score;
        aiScoreDisplay.textContent = player2Score;
        resetBall();
        gameRunning = true;
    }

    // --- 4. Drawing Functions (UPDATED FOR ROUNDED SHAPES) ---

    // Draws a rounded rectangle (for paddles)
    function roundRect(x, y, w, h, r, color) {
        ctx.beginPath();
        ctx.moveTo(x + r, y);
        
        // Top-right corner
        ctx.lineTo(x + w - r, y);
        ctx.arcTo(x + w, y, x + w, y + r, r);

        // Bottom-right corner
        ctx.lineTo(x + w, y + h - r);
        ctx.arcTo(x + w, y + h, x + w - r, y + h, r);

        // Bottom-left corner
        ctx.lineTo(x + r, y + h);
        ctx.arcTo(x, y + h, x, y + h - r, r);

        // Top-left corner
        ctx.lineTo(x, y + r);
        ctx.arcTo(x, y, x + r, y, r);
        
        ctx.closePath();
        ctx.fillStyle = color;
        ctx.fill();
    }
    
    // Draws a simple rectangle (kept for background/net, since rounded isn't needed)
    function drawRect(x, y, w, h, color) {
        ctx.fillStyle = color;
        ctx.fillRect(x, y, w, h);
    }

    // Draws a perfect circle (for the ball)
    function drawCircle(x, y, radius, color) {
        ctx.fillStyle = color;
        ctx.beginPath();
        ctx.arc(x, y, radius, 0, Math.PI * 2, false);
        ctx.closePath();
        ctx.fill();
    }

    // The main function to draw the entire game scene
    function draw() {
        // 1. Clear the canvas (draw background)
        drawRect(0, 0, GAME_WIDTH, GAME_HEIGHT, '#1a252f');

        // 2. Draw the dividing net (optional)
        for (let i = 0; i < GAME_HEIGHT; i += 15) {
            drawRect(GAME_WIDTH / 2 - 1, i, 2, 5, '#ecf0f1');
        }

        // 3. Draw Paddles - NOW ROUNDED
        roundRect(playerPaddle.x, playerPaddle.y, PADDLE_WIDTH, PADDLE_HEIGHT, PADDLE_RADIUS, '#2ecc71');
        roundRect(aiPaddle.x, aiPaddle.y, PADDLE_WIDTH, PADDLE_HEIGHT, PADDLE_RADIUS, '#e74c3c');

        // 4. Draw Ball - NOW A PERFECT CIRCLE
        drawCircle(ball.x, ball.y, BALL_SIZE / 2, '#f1c40f');
    }

    // --- 5. Update Game State (Physics and AI) ---
    // (No changes needed here, physics still treats them as simple rectangles/circles)
    function update() {
        if (!gameRunning) return;
        
        // --- A. Ball Movement ---
        ball.x += ball.dx;
        ball.y += ball.dy;

        // --- B. Ball Collision with Top/Bottom Walls ---
        if (ball.y < BALL_SIZE / 2 || ball.y > GAME_HEIGHT - BALL_SIZE / 2) {
            ball.dy = -ball.dy; // Reverse vertical direction
        }

        // --- C. Ball Scoring (Left/Right Walls) ---
        if (ball.x < 0) {
            // AI (Player 2) scores
            player2Score++;
            aiScoreDisplay.textContent = player2Score;
            resetBall();
            checkWin();
        } else if (ball.x > GAME_WIDTH) {
            // Player 1 scores
            player1Score++;
            playerScoreDisplay.textContent = player1Score;
            resetBall();
            checkWin();
        }

        // --- D. Player Paddle Movement ---
        playerPaddle.y += playerPaddle.dy;
        // Keep paddle within bounds
        if (playerPaddle.y < 0) playerPaddle.y = 0;
        if (playerPaddle.y > GAME_HEIGHT - PADDLE_HEIGHT) playerPaddle.y = GAME_HEIGHT - PADDLE_HEIGHT;

        // --- E. Simple AI Movement (Follow the ball) ---
        const aiCenter = aiPaddle.y + PADDLE_HEIGHT / 2;
        const speed = 4; // AI reaction speed
        if (aiCenter < ball.y - 10) {
            aiPaddle.y += speed;
        } else if (aiCenter > ball.y + 10) {
            aiPaddle.y -= speed;
        }
        // Keep AI paddle within bounds
        if (aiPaddle.y < 0) aiPaddle.y = 0;
        if (aiPaddle.y > GAME_HEIGHT - PADDLE_HEIGHT) aiPaddle.y = GAME_HEIGHT - PADDLE_HEIGHT;


        // --- F. Ball Collision with Paddles (Simplified Logic) ---

        // Player Paddle collision
        if (ball.dx < 0 && 
            ball.x - BALL_SIZE/2 < playerPaddle.x + PADDLE_WIDTH &&
            ball.y > playerPaddle.y && 
            ball.y < playerPaddle.y + PADDLE_HEIGHT) {
            
            // Reverse direction and increase speed slightly
            ball.dx = -ball.dx * 1.05; 
            
            // Add angle based on where the ball hit the paddle (middle = flat, edge = steep)
            let collidePoint = ball.y - (playerPaddle.y + PADDLE_HEIGHT / 2);
            collidePoint = collidePoint / (PADDLE_HEIGHT / 2); // Normalize to -1 to 1
            ball.dy = collidePoint * 8; // Max vertical speed of 8
        }

        // AI Paddle collision
        if (ball.dx > 0 && 
            ball.x + BALL_SIZE/2 > aiPaddle.x &&
            ball.y > aiPaddle.y && 
            ball.y < aiPaddle.y + PADDLE_HEIGHT) {

            // Reverse direction and increase speed slightly
            ball.dx = -ball.dx * 1.05; 
            
            // Add angle based on where the ball hit the paddle
            let collidePoint = ball.y - (aiPaddle.y + PADDLE_HEIGHT / 2);
            collidePoint = collidePoint / (PADDLE_HEIGHT / 2); // Normalize to -1 to 1
            ball.dy = collidePoint * 8;
        }
    }

    // --- 6. The Game Loop ---

    function gameLoop() {
        update();
        draw();
        
        requestAnimationFrame(gameLoop);
    }
    
    // --- 7. Input Handling ---

    document.addEventListener('keydown', (e) => {
        if (e.key === 'w' || e.key === 'W') {
            playerPaddle.dy = -7; // Move up
        } else if (e.key === 's' || e.key === 'S') {
            playerPaddle.dy = 7; // Move down
        }
    });

    document.addEventListener('keyup', (e) => {
        if ((e.key === 'w' || e.key === 'W') || (e.key === 's' || e.key === 'S')) {
            playerPaddle.dy = 0; // Stop
        }
    });
    
    // --- 8. Game Initialization ---
    
    // Start the game when the "Start Game" button is clicked
    document.getElementById('start-game-btn').addEventListener('click', () => {
        if (!gameRunning) {
            resetGame(); // Initialize/reset and start
            gameLoop(); // Start the animation loop
            // NOTE: Locking username/button text logic assumes the HTML still has these IDs
            document.getElementById('username').disabled = true;
            document.getElementById('start-game-btn').textContent = "Game In Progress";
        }
    });
    
    // Initial draw before the game loop starts
    draw(); 
});
```
