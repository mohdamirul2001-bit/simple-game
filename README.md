<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Snake Game</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Press+Start+2P&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Press Start 2P', cursive;
            background-color: #1a1a1a;
            color: #f0f0f0;
            overflow: hidden; /* Prevent scrolling on mobile */
        }
        canvas {
            background-color: #000;
            border: 4px solid #4a4a4a;
            box-shadow: 0 0 20px rgba(0,255,0,0.3);
        }
        .game-container {
            touch-action: none; /* Disable default touch actions like scroll/zoom */
        }
    </style>
</head>
<body class="flex flex-col items-center justify-center min-h-screen p-4">

    <h1 class="text-4xl md:text-5xl text-green-400 mb-4 tracking-wider">SNAKE</h1>

    <div class="w-full max-w-lg md:max-w-2xl bg-[#2a2a2a] p-4 rounded-lg shadow-2xl border-2 border-gray-600">
        <!-- Game Info Header -->
        <div class="flex flex-col sm:flex-row justify-between items-center mb-4 text-lg">
            <div class="mb-2 sm:mb-0">
                <span>SCORE: </span>
                <span id="score" class="text-yellow-400">0</span>
            </div>
            <div class="flex items-center space-x-3">
                <label for="snake-shape">Shape:</label>
                <select id="snake-shape" class="bg-gray-700 text-white p-1 rounded-md focus:outline-none focus:ring-2 focus:ring-green-400">
                    <option value="square">Square</option>
                    <option value="circle">Circle</option>
                    <option value="triangle">Triangle</option>
                </select>
            </div>
        </div>

        <!-- Game Canvas -->
        <div class="relative game-container">
            <canvas id="gameCanvas" class="w-full aspect-square rounded-md"></canvas>
            <div id="gameOverScreen" class="absolute inset-0 bg-black bg-opacity-70 flex-col justify-center items-center text-center hidden">
                <h2 class="text-4xl text-red-500 mb-4">GAME OVER</h2>
                <p class="text-xl mb-6">Your Score: <span id="finalScore" class="text-yellow-400"></span></p>
                <button id="restartButton" class="px-6 py-3 bg-green-500 text-black rounded-lg hover:bg-green-400 focus:outline-none focus:ring-2 focus:ring-green-300 text-lg">
                    RESTART
                </button>
            </div>
             <div id="startScreen" class="absolute inset-0 bg-black bg-opacity-70 flex flex-col justify-center items-center text-center">
                <h2 class="text-3xl text-white mb-6">Ready to Play?</h2>
                <button id="startButton" class="px-6 py-3 bg-green-500 text-black rounded-lg hover:bg-green-400 focus:outline-none focus:ring-2 focus:ring-green-300 text-lg">
                    START GAME
                </button>
            </div>
        </div>
    </div>

    <!-- Instructions -->
    <div class="mt-6 text-center text-gray-400">
        <p>Use <span class="text-yellow-400">ARROW KEYS</span> or <span class="text-yellow-400">SWIPE</span> to move.</p>
    </div>

    <script>
        // --- DOM Elements ---
        const canvas = document.getElementById('gameCanvas');
        const ctx = canvas.getContext('2d');
        const scoreElement = document.getElementById('score');
        const gameOverScreen = document.getElementById('gameOverScreen');
        const startScreen = document.getElementById('startScreen');
        const finalScoreElement = document.getElementById('finalScore');
        const startButton = document.getElementById('startButton');
        const restartButton = document.getElementById('restartButton');
        const shapeSelector = document.getElementById('snake-shape');

        // --- Game Configuration ---
        const gridSize = 20; // Number of cells in width/height
        let canvasSize = Math.min(window.innerWidth * 0.9, 600); // Responsive canvas size
        canvas.width = canvasSize;
        canvas.height = canvasSize;
        let tileSize = canvas.width / gridSize;
        
        // --- Game State ---
        let snake = [];
        let food = {};
        let score = 0;
        let direction = 'right';
        let changingDirection = false;
        let gameRunning = false;
        let gameLoop;
        let snakeShape = 'square';
        
        // --- Touch Controls State ---
        let touchStartX = 0;
        let touchStartY = 0;
        let touchEndX = 0;
        let touchEndY = 0;

        // --- Game Setup and Initialization ---
        
        /**
         * Resets the game to its initial state
         */
        function initializeGame() {
            // Reset snake in the middle
            snake = [{ x: 10, y: 10 }];
            
            // Reset direction and score
            direction = 'right';
            score = 0;
            scoreElement.textContent = score;

            // Generate first food
            generateFood();

            // Hide overlays
            gameOverScreen.classList.add('hidden');
            gameOverScreen.classList.remove('flex');
            startScreen.classList.add('hidden');
            startScreen.classList.remove('flex');
        }
        
        /**
         * Starts the game loop.
         */
        function startGame() {
            if (gameRunning) return;
            gameRunning = true;
            initializeGame();
            // Using setInterval for a consistent game speed
            gameLoop = setInterval(main, 100);
        }

        /**
         * The main game loop function that updates and draws the game.
         */
        function main() {
            if (!gameRunning) return;
            
            changingDirection = false; // Allow next direction change
            clearCanvas();
            moveSnake();
            drawGameElements();
            
            if (hasGameEnded()) {
                endGame();
            }
        }
        
        /**
         * Ends the game, stops the loop, and shows the game over screen.
         */
        function endGame() {
            gameRunning = false;
            clearInterval(gameLoop);
            finalScoreElement.textContent = score;
            gameOverScreen.classList.remove('hidden');
            gameOverScreen.classList.add('flex');
        }

        // --- Drawing Functions ---

        /**
         * Clears the entire canvas.
         */
        function clearCanvas() {
            ctx.fillStyle = '#000'; // Black background
            ctx.fillRect(0, 0, canvas.width, canvas.height);
        }

        /**
         * Draws all game elements: snake and food.
         */
        function drawGameElements() {
            drawSnake();
            drawFood();
        }

        /**
         * Draws the snake on the canvas based on the selected shape.
         */
        function drawSnake() {
            snakeShape = shapeSelector.value;
            snake.forEach((segment, index) => {
                // Head is brighter green, body is darker
                ctx.fillStyle = index === 0 ? '#00ff00' : '#00a000';
                
                const x = segment.x * tileSize;
                const y = segment.y * tileSize;

                switch (snakeShape) {
                    case 'circle':
                        ctx.beginPath();
                        ctx.arc(x + tileSize / 2, y + tileSize / 2, tileSize / 2, 0, 2 * Math.PI);
                        ctx.fill();
                        break;
                    case 'triangle':
                        ctx.beginPath();
                        ctx.moveTo(x + tileSize / 2, y);
                        ctx.lineTo(x, y + tileSize);
                        ctx.lineTo(x + tileSize, y + tileSize);
                        ctx.closePath();
                        ctx.fill();
                        break;
                    case 'square':
                    default:
                        ctx.fillRect(x, y, tileSize, tileSize);
                        break;
                }
            });
        }

        /**
         * Draws the food on the canvas.
         */
        function drawFood() {
            ctx.fillStyle = '#ff0000'; // Red food
            ctx.strokeStyle = '#a00000';
            ctx.lineWidth = 2;
            ctx.fillRect(food.x * tileSize, food.y * tileSize, tileSize, tileSize);
            ctx.strokeRect(food.x * tileSize, food.y * tileSize, tileSize, tileSize);
        }

        // --- Game Logic ---
        
        /**
         * Updates the snake's position and handles food collision.
         */
        function moveSnake() {
            const head = { x: snake[0].x, y: snake[0].y };

            // Update head position based on direction
            if (direction === 'up') head.y -= 1;
            if (direction === 'down') head.y += 1;
            if (direction === 'left') head.x -= 1;
            if (direction === 'right') head.x += 1;

            // Add new head to the front of the snake
            snake.unshift(head);
            
            // Check for food collision
            const hasEatenFood = head.x === food.x && head.y === food.y;
            if (hasEatenFood) {
                score += 10;
                scoreElement.textContent = score;
                generateFood(); // Create new food
            } else {
                // If no food eaten, remove the tail
                snake.pop();
            }
        }
        
        /**
         * Generates food at a random position, avoiding the snake's body.
         */
        function generateFood() {
            let foodX, foodY;
            while (true) {
                foodX = Math.floor(Math.random() * gridSize);
                foodY = Math.floor(Math.random() * gridSize);
                
                // Ensure food is not on the snake
                let foodOnSnake = snake.some(segment => segment.x === foodX && segment.y === foodY);
                if (!foodOnSnake) break;
            }
            food = { x: foodX, y: foodY };
        }
        
        /**
         * Checks for game-ending conditions (wall or self collision).
         * @returns {boolean} True if the game has ended, false otherwise.
         */
        function hasGameEnded() {
            const head = snake[0];
            
            // Wall collision
            if (head.x < 0 || head.x >= gridSize || head.y < 0 || head.y >= gridSize) {
                return true;
            }
            
            // Self collision (check if head hits any part of the body)
            for (let i = 1; i < snake.length; i++) {
                if (head.x === snake[i].x && head.y === snake[i].y) {
                    return true;
                }
            }
            
            return false;
        }

        // --- Event Handlers ---
        
        /**
         * Handles keyboard input for changing snake direction.
         * @param {KeyboardEvent} event The keyboard event.
         */
        function handleKeyDown(event) {
            if (changingDirection) return;
            changingDirection = true;
            
            const keyPressed = event.key;
            const goingUp = direction === 'up';
            const goingDown = direction === 'down';
            const goingLeft = direction === 'left';
            const goingRight = direction === 'right';

            if ((keyPressed === 'ArrowUp' || keyPressed.toLowerCase() === 'w') && !goingDown) {
                direction = 'up';
            }
            if ((keyPressed === 'ArrowDown' || keyPressed.toLowerCase() === 's') && !goingUp) {
                direction = 'down';
            }
            if ((keyPressed === 'ArrowLeft' || keyPressed.toLowerCase() === 'a') && !goingRight) {
                direction = 'left';
            }
            if ((keyPressed === 'ArrowRight' || keyPressed.toLowerCase() === 'd') && !goingLeft) {
                direction = 'right';
            }
        }
        
        // --- Touch Control Handlers ---
        function handleTouchStart(event) {
            touchStartX = event.changedTouches[0].screenX;
            touchStartY = event.changedTouches[0].screenY;
        }
        
        function handleTouchEnd(event) {
            touchEndX = event.changedTouches[0].screenX;
            touchEndY = event.changedTouches[0].screenY;
            handleSwipe();
        }

        function handleSwipe() {
            const dx = touchEndX - touchStartX;
            const dy = touchEndY - touchStartY;
            const absDx = Math.abs(dx);
            const absDy = Math.abs(dy);

            if (changingDirection) return;
            
            // Check for horizontal vs vertical swipe
            if (absDx > absDy) { // Horizontal swipe
                if (dx > 0 && direction !== 'left') direction = 'right';
                else if (dx < 0 && direction !== 'right') direction = 'left';
            } else { // Vertical swipe
                if (dy > 0 && direction !== 'up') direction = 'down';
                else if (dy < 0 && direction !== 'down') direction = 'up';
            }
            changingDirection = true;
        }


        /**
         * Resizes the canvas and recalculates tile sizes on window resize.
         */
        function handleResize() {
            canvasSize = Math.min(window.innerWidth > 768 ? 600 : window.innerWidth * 0.9, 500);
            canvas.width = canvasSize;
            canvas.height = canvasSize;
            tileSize = canvas.width / gridSize;
            // Redraw immediately after resize
            if (gameRunning) {
                drawGameElements();
            }
        }
        
        // --- Event Listeners ---
        document.addEventListener('keydown', handleKeyDown);
        startButton.addEventListener('click', startGame);
        restartButton.addEventListener('click', startGame);
        window.addEventListener('resize', handleResize);
        
        // Touch Listeners
        canvas.addEventListener('touchstart', handleTouchStart, false);
        canvas.addEventListener('touchend', handleTouchEnd, false);

        // --- Initial Call ---
        // Draw initial state of the empty canvas
        clearCanvas();
    </script>
</body>
</html>
