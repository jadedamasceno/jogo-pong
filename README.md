<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Pong</title>
    <style>
        body {
            margin: 0;
            padding: 0;
            overflow: hidden;
            background-color: black;
        }
        canvas {
            display: block;
            margin: auto;
            background-color: black;
        }
    </style>
</head>
<body>
    <canvas id="pong" width="800" height="400"></canvas>

    <script>
        const canvas = document.getElementById("pong");
        const ctx = canvas.getContext("2d");

        // Configuração do jogo
        const paddleWidth = 10;
        const paddleHeight = 100;
        const ballSize = 10;
        const paddleSpeed = 5;
        const ballSpeed = 5;

        // Posições iniciais
        let paddle1Y = canvas.height / 2 - paddleHeight / 2;
        let paddle2Y = canvas.height / 2 - paddleHeight / 2;
        let ballX = canvas.width / 2;
        let ballY = canvas.height / 2;
        let ballDX = ballSpeed;
        let ballDY = ballSpeed;
        let playerScore = 0;
        let computerScore = 0;

        // Controles
        let upPressed = false;
        let downPressed = false;

        // Função principal do jogo
        function gameLoop() {
            // Atualizar
            update();
            // Desenhar
            draw();
            // Loop do jogo
            requestAnimationFrame(gameLoop);
        }

        // Atualizar estado do jogo
        function update() {
            // Mover a bola
            ballX += ballDX;
            ballY += ballDY;

            // Verificar colisões com as paredes superior e inferior
            if (ballY + ballSize > canvas.height || ballY - ballSize < 0) {
                ballDY = -ballDY;
            }

            // Verificar colisões com as paletas
            if (
                ballX - ballSize < paddleWidth &&
                ballY > paddle1Y &&
                ballY < paddle1Y + paddleHeight
            ) {
                ballDX = -ballDX;
                playerScore++;
            } else if (
                ballX + ballSize > canvas.width - paddleWidth &&
                ballY > paddle2Y &&
                ballY < paddle2Y + paddleHeight
            ) {
                ballDX = -ballDX;
                computerScore++;
            }

            // Mover a paleta do jogador
            if (upPressed && paddle1Y > 0) {
                paddle1Y -= paddleSpeed;
            } else if (downPressed && paddle1Y < canvas.height - paddleHeight) {
                paddle1Y += paddleSpeed;
            }

            // Mover a paleta do computador
            const paddle2Center = paddle2Y + paddleHeight / 2;
            if (paddle2Center < ballY - 35) {
                paddle2Y += paddleSpeed;
            } else if (paddle2Center > ballY + 35) {
                paddle2Y -= paddleSpeed;
            }

            // Verificar se o jogo acabou
            if (playerScore === 5 || computerScore === 5) {
                gameOver();
            }
        }

        // Desenhar elementos do jogo
        function draw() {
            // Limpar o canvas
            ctx.clearRect(0, 0, canvas.width, canvas.height);

            // Desenhar paletas
            ctx.fillStyle = "white";
            ctx.fillRect(0, paddle1Y, paddleWidth, paddleHeight);
            ctx.fillRect(canvas.width - paddleWidth, paddle2Y, paddleWidth, paddleHeight);

            // Desenhar a bola
            ctx.beginPath();
            ctx.arc(ballX, ballY, ballSize, 0, Math.PI * 2);
            ctx.fill();

            // Desenhar placar
            ctx.fillText(`Player: ${playerScore}`, 50, 50);
            ctx.fillText(`Computer: ${computerScore}`, canvas.width - 150, 50);
        }

        // Tratamento de eventos de teclado
        document.addEventListener("keydown", keyDownHandler, false);
        document.addEventListener("keyup", keyUpHandler, false);

        function keyDownHandler(e) {
            if (e.key === "ArrowUp") {
                upPressed = true;
            } else if (e.key === "ArrowDown") {
                downPressed = true;
            }
        }

        function keyUpHandler(e) {
            if (e.key === "ArrowUp") {
                upPressed = false;
            } else if (e.key === "ArrowDown") {
                downPressed = false;
            }
        }

        // Função de fim de jogo
        function gameOver() {
            // Mostrar mensagem de fim de jogo
            if (playerScore > computerScore) {
                alert("Você perdeu!");
            } else {
                alert("Você venceu!");
            }

            // Reiniciar o jogo
            document.location.reload();
        }

        // Iniciar o loop do jogo
        gameLoop();
    </script>
</body>
</html>
