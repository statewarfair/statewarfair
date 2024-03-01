<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Game Title</title>
    <style>
        canvas {
            display: block;
            margin: 0 auto;
            background: #000; /* Example background color */
        }
    </style>
</head>
<body>
    <canvas id="gameCanvas"></canvas>
    <script>
        // Constants for screen dimensions
        const SCREEN_WIDTH = 800;
        const SCREEN_HEIGHT = 600;

        // Set up the canvas and context
        const canvas = document.getElementById('gameCanvas');
        const ctx = canvas.getContext('2d');
        canvas.width = SCREEN_WIDTH;
        canvas.height = SCREEN_HEIGHT;

        // Sound Manager
        class SoundManager {
            constructor() {
                this.sounds = {
                    gunFire: new Audio('gunFire.mp3'),
                    explosion: new Audio('explosion.mp3'),
                    gunSwitch: new Audio('gunSwitch.mp3'), // Make sure to have this sound file
                    // Add more sounds as needed
                };
            }

            play(soundName) {
                if (this.sounds[soundName]) {
                    this.sounds[soundName].play();
                }
            }
        }

        // Load assets function
        function loadAssets() {
            // Load images and sounds here
            return {
                playerImg: new Image(),
                zombieImg: new Image(),
                bulletImg: new Image(),
                bombImg: new Image(),
                mapBackgroundImg: new Image(),
                healthPackImg: new Image(), // New asset for health pack
                ammoBoxImg: new Image(), // New asset for ammo box
                // Add other assets
            };
        }

        // Initialize assets
        const assets = loadAssets();
        assets.playerImg.src = 'player.png';
        assets.zombieImg.src = 'zombie.png';
        assets.bulletImg.src = 'bullet.png';
        assets.bombImg.src = 'bomb.png';
        assets.mapBackgroundImg.src = 'map_background.png';
        assets.healthPackImg.src = 'health_pack.png'; // Set source for health pack
        assets.ammoBoxImg.src = 'ammo_box.png'; // Set source for ammo box

        // Score Manager
        class ScoreManager {
            constructor() {
                this.score = 0;
            }

            increaseScore(points) {
                this.score += points;
            }

            getScore() {
                return this.score;
            }
        }

        // PowerUp Class
        class PowerUp {
            constructor(image, type, x, y) {
                this.img = image;
                this.type = type;
                this.x = x;
                this.y = y;
                this.width = 32; // Assuming a size for the power-up
                this.height = 32;
                this.collected = false;
            }

            draw() {
                if (!this.collected) {
                    ctx.drawImage(this.img, this.x, this.y, this.width, this.height);
                }
            }

            collect(player) {
                if (!this.collected && this.x < player.x + player.width &&
                    this.x + this.width > player.x &&
                    this.y < player.y + player.height &&
                    this.y + this.height > player.y) {
                    this.collected = true;
                    // Apply the power-up effect based on its type
                    switch (this.type) {
                        case 'health':
                            player.increaseHealth(25); // Example value
                            break;
                        case 'ammo':
                            player.currentGun.increaseAmmo(50); // Example value
                            break;
                        // Add more types as needed
                    }
                }
            }
        }

        // New Feature: Speed Power-Up
        class SpeedPowerUp extends PowerUp {
            constructor(image, x, y) {
                super(image, 'speed', x, y);
            }

            collect(player) {
                if (!this.collected && this.x < player.x + player.width &&
                    this.x + this.width > player.x &&
                    this.y < player.y + player.height &&
                    this.y + this.height > player.y) {
                    this.collected = true;
                    player.increaseSpeed(1.5); // Increase speed by 1.5 times
                    setTimeout(() => player.resetSpeed(), 5000); // Reset speed after 5 seconds
                }
            }
        }

        // Collision Detection Function
        function checkCollisions(objects1, objects2, onCollision) {
            objects1.forEach(obj1 => {
                objects2.forEach(obj2 => {
                    if (obj1.x < obj2.x + obj2.width &&
                        obj1.x + obj1.width > obj2.x &&
                        obj1.y < obj2.y + obj2.height &&
                        obj1.y + obj1.height > obj2.y) {
                        onCollision(obj1, obj2);
                    }
                });
            });
        }

        // Player Class
        class Player {
            constructor(image, x, y) {
                this.image = image;
                this.x = x;
                this.y = y;
                this.width = 50; // Example width
                this.height = 50; // Example height
                this.speed = 5; // Example speed
            }

            // Draw the player on the canvas
            draw() {
                ctx.drawImage(this.image, this.x, this.y, this.width, this.height);
            }

            // Move the player based on input
            move(direction) {
                switch (direction) {
                    case 'up':
                        this.y -= this.speed;
                        break;
                    case 'down':
                        this.y += this.speed;
                        break;
                    case 'left':
                        this.x -= this.speed;
                        break;
                    case 'right':
                        this.x += this.speed;
                        break;
                }
            }

            // Method to handle player actions like shooting
            action(actionType) {
                switch (actionType) {
                    case 'shoot':
                        // Implement shooting logic here
                        console.log('Player shot a bullet!');
                        break;
                    // Add more actions as needed
                }
            }
        }

        // Initialize the player
        const player = new Player(assets.playerImg, 100, 100); // Starting position of the player

        // Game loop function
        function gameLoop() {
            // Clear the canvas
            ctx.clearRect(0, 0, SCREEN_WIDTH, SCREEN_HEIGHT);

            // Draw the map background
            ctx.drawImage(assets.mapBackgroundImg, 0, 0, SCREEN_WIDTH, SCREEN_HEIGHT);

            // Draw and move the player
            player.draw();
            // player.move(); // Uncomment and implement player movement logic

            // Check for collisions
            // checkCollisions(); // Uncomment and implement collision detection logic

            // Request the next animation frame
            requestAnimationFrame(gameLoop);
        }

        // Start the game loop
        gameLoop();
    </script>
</body>
</html>
