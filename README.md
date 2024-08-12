<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Jogo da Bolinha</title>
    <style>
        body {
            margin: 0;
            padding: 0;
            overflow: hidden;
            background-color: #f0f0f0;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
        }
        #gameContainer {
            position: relative;
            display: flex;
            flex-direction: column;
            align-items: center;
        }
        #gameArea {
            position: relative;
            width: 500px;
            height: 500px;
            background-color: #fff;
            border: 10px solid;
            border-image: linear-gradient(45deg, rgba(255,0,0,1), rgba(0,255,0,1), rgba(0,0,255,1));
            border-image-slice: 1;
            overflow: hidden;
            margin-bottom: 20px;
        }
        .square {
            position: absolute;
            width: 50px;
            height: 50px;
            background-color: #4CAF50;
            border: 2px solid #000;
            border-radius: 8px;
            transition: background-color 0.3s, transform 0.2s;
        }
        .ball {
            position: absolute;
            width: 30px;
            height: 30px;
            background-color: #FF5722;
            border-radius: 50%;
        }
        #watermark {
            position: absolute;
            bottom: 10px;
            right: 10px;
            font-size: 24px;
            color: rgba(0, 0, 0, 0.5);
            font-family: Arial, sans-serif;
            pointer-events: none;
        }
        #updateButton {
            padding: 10px 20px;
            font-size: 16px;
            color: #fff;
            background-color: #007BFF;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }
        #updateButton:hover {
            background-color: #0056b3;
        }
    </style>
</head>
<body>
    <div id="gameContainer">
        <div id="gameArea">
            <!-- Bolinha -->
            <div class="ball" id="ball"></div>
            <!-- Marca d'água -->
            <div id="watermark">MOUJUN</div>
        </div>
        <button id="updateButton">Atualizar</button>
    </div>

    <script>
        const ball = document.getElementById('ball');
        const gameArea = document.getElementById('gameArea');
        const updateButton = document.getElementById('updateButton');
        const ballRadius = ball.offsetWidth / 2;
        const speed = 2;
        let x, y, dx, dy;
        const numSquares = 15; // Número de quadrados a serem adicionados
        let squares = [];

        function createSquare() {
            const square = document.createElement('div');
            square.classList.add('square');
            const size = 50;
            const left = Math.random() * (gameArea.clientWidth - size);
            const top = Math.random() * (gameArea.clientHeight - size);
            square.style.left = `${left}px`;
            square.style.top = `${top}px`;
            gameArea.appendChild(square);
            return square;
        }

        function initializeGame() {
            // Remove os quadrados existentes
            squares.forEach(square => square.remove());
            squares = [];

            // Adiciona novos quadrados
            for (let i = 0; i < numSquares; i++) {
                squares.push(createSquare());
            }

            // Inicializa a posição da bolinha e a velocidade
            x = Math.random() * (gameArea.clientWidth - ball.offsetWidth);
            y = Math.random() * (gameArea.clientHeight - ball.offsetHeight);
            dx = speed * (Math.random() < 0.5 ? 1 : -1);
            dy = speed * (Math.random() < 0.5 ? 1 : -1);

            ball.style.left = `${x}px`;
            ball.style.top = `${y}px`;
        }

        function update() {
            x += dx;
            y += dy;

            // Verifica colisões com as bordas da área de jogo
            if (x + ballRadius * 2 > gameArea.clientWidth || x < 0) {
                dx = -dx;
            }
            if (y + ballRadius * 2 > gameArea.clientHeight || y < 0) {
                dy = -dy;
            }

            // Atualiza a posição da bolinha
            ball.style.left = `${x}px`;
            ball.style.top = `${y}px`;

            // Verifica colisões com os quadrados
            squares.forEach(square => {
                const sq = square.getBoundingClientRect();
                const ballRect = ball.getBoundingClientRect();

                if (
                    ballRect.left < sq.right &&
                    ballRect.right > sq.left &&
                    ballRect.top < sq.bottom &&
                    ballRect.bottom > sq.top
                ) {
                    // Altera a cor do quadrado e aplica uma transformação ao colidir
                    square.style.backgroundColor = `hsl(${Math.random() * 360}, 100%, 50%)`;
                    square.style.transform = 'scale(1.2)';
                    setTimeout(() => {
                        square.style.transform = 'scale(1)';
                    }, 100);

                    // Adiciona aleatoriedade à mudança de direção da bolinha
                    dx = speed * (Math.random() < 0.5 ? 1 : -1);
                    dy = speed * (Math.random() < 0.5 ? 1 : -1);
                }
            });

            // Repetir a atualização
            requestAnimationFrame(update);
        }

        // 
        initializeGame();
        update();

        // Adiciona funcionalidade ao botão de atualizar
        updateButton.addEventListener('click', () => {
            initializeGame();
        });
    </script>
</body>
</html>
