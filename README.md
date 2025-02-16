<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Snake Game</title>
    <style>
        body {
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            background-color: #222;
        }
        canvas {
            border: 2px solid white;
        }
    </style>
</head>
<body>
    <canvas id="gameCanvas" width="400" height="400"></canvas>
    <script>
        const canvas = document.getElementById("gameCanvas");
        const ctx = canvas.getContext("2d");

        const box = 20;
        let snake = [{ x: 10 * box, y: 10 * box }];
        let direction = "RIGHT";
        let food = {
            x: Math.floor(Math.random() * 20) * box,
            y: Math.floor(Math.random() * 20) * box,
        };

        document.addEventListener("keydown", changeDirection);

        function changeDirection(event) {
            if (event.key === "ArrowUp" && direction !== "DOWN") direction = "UP";
            else if (event.key === "ArrowDown" && direction !== "UP") direction = "DOWN";
            else if (event.key === "ArrowLeft" && direction !== "RIGHT") direction = "LEFT";
            else if (event.key === "ArrowRight" && direction !== "LEFT") direction = "RIGHT";
        }

        function draw() {
            ctx.fillStyle = "black";
            ctx.fillRect(0, 0, canvas.width, canvas.height);
            
            ctx.fillStyle = "red";
            ctx.fillRect(food.x, food.y, box, box);

            for (let i = 0; i < snake.length; i++) {
                ctx.fillStyle = i === 0 ? "lime" : "green";
                ctx.fillRect(snake[i].x, snake[i].y, box, box);
            }
            
            let newX = snake[0].x;
            let newY = snake[0].y;
            if (direction === "UP") newY -= box;
            if (direction === "DOWN") newY += box;
            if (direction === "LEFT") newX -= box;
            if (direction === "RIGHT") newX += box;
            
            if (newX === food.x && newY === food.y) {
                food.x = Math.floor(Math.random() * 20) * box;
                food.y = Math.floor(Math.random() * 20) * box;
            } else {
                snake.pop();
            }
            
            let newHead = { x: newX, y: newY };
            if (
                newX < 0 || newX >= canvas.width ||
                newY < 0 || newY >= canvas.height ||
                snake.some(segment => segment.x === newX && segment.y === newY)
            ) {
                alert("Game Over");
                document.location.reload();
            }
            
            snake.unshift(newHead);
        }

        setInterval(draw, 100);
    </script>
</body>
</html>
