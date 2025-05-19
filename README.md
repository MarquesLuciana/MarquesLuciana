<h1 align="center">Olá! Eu sou a Luciana Marques da Conceição</h1>

<p align="center">
  💻 Desenvolvedora Full Stack em formação<br>
  📚 Estudando JavaScript, React, Node.js e Python<br>
  ☁ Apaixonada por tecnologia, aprendizado contínuo e Dados<br>
  ✨ Pronomes: ela/dela
</p>

---

### *Sobre Mim*

Sou estudante de Ciências da Computação e atualmente participo do programa de formação da *Recode Pro IA*, onde estou mergulhando no mundo do desenvolvimento Full Stack com foco em tecnologias modernas como JavaScript, React, Node.js, Python e Inteligência Artificial.  
Tenho uma trajetória que combina organização, criatividade e muita vontade de aprender.

---

### *Tecnologias que estou aprendendo*

<div style="display: flex; gap: 10px;">
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/javascript/javascript-original.svg" width="40"/>
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/python/python-original.svg" width="40"/>
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/react/react-original.svg" width="40"/>
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/html5/html5-original.svg" width="40"/>
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/css3/css3-original.svg" width="40"/>
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/nodejs/nodejs-original.svg" width="40"/>
</div>

---

### *GitHub Stats*

![Luciana GitHub stats](https://github-readme-stats.vercel.app/api?username=MarquesLuciana&show_icons=true&theme=tokyonight)
![Top Langs](https://github-readme-stats.vercel.app/api/top-langs/?username=MarquesLuciana&layout=compact&theme=tokyonight)

---

### *Onde me encontrar*

[![LinkedIn](https://img.shields.io/badge/-LinkedIn-0077B5?style=flat-square&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/luciana-marques-da-concei%C3%A7%C3%A3o-5843222b4/)
[![Email](https://img.shields.io/badge/-Email-D14836?style=flat-square&logo=gmail&logoColor=white)](mailto:marques.conceicaolu@gmail.com)

---



<!DOCTYPE html>
<html>
<head>
    <title>Cobrinha que come commits do GitHub</title>
    <style>
        body {
            display: flex;
            flex-direction: column;
            align-items: center;
            font-family: Arial, sans-serif;
        }
        canvas {
            border: 1px solid black;
            margin-bottom: 20px;
        }
        .controls {
            margin-bottom: 20px;
        }
    </style>
</head>
<body>
    <h1>Cobrinha que come commits do GitHub</h1>
    <div class="controls">
        <label for="repo">Repositório GitHub (user/repo):</label>
        <input type="text" id="repo" value="torvalds/linux">
        <button id="start">Iniciar Jogo</button>
    </div>
    <canvas id="gameCanvas" width="600" height="400"></canvas>
    <div id="score">Pontuação: 0</div>

    <script>
        const canvas = document.getElementById('gameCanvas');
        const ctx = canvas.getContext('2d');
        const repoInput = document.getElementById('repo');
        const startButton = document.getElementById('start');
        const scoreDisplay = document.getElementById('score');

        let snake = [];
        let food = {};
        let direction = 'right';
        let nextDirection = 'right';
        let gameSpeed = 150;
        let gameLoop;
        let score = 0;
        let commits = [];
        let currentCommitIndex = 0;

        // Tamanho de cada segmento da cobra e da comida
        const gridSize = 20;

        function initGame() {
            // Inicializa a cobra
            snake = [
                {x: 5 * gridSize, y: 10 * gridSize},
                {x: 4 * gridSize, y: 10 * gridSize},
                {x: 3 * gridSize, y: 10 * gridSize}
            ];
            
            direction = 'right';
            nextDirection = 'right';
            score = 0;
            scoreDisplay.textContent = Pontuação: ${score};
            
            // Busca commits do GitHub
            fetchCommits();
        }

        async function fetchCommits() {
            const repo = repoInput.value.trim();
            if (!repo) return;
            
            try {
                const response = await fetch(https://api.github.com/repos/${repo}/commits);
                if (!response.ok) throw new Error('Erro ao buscar commits');
                
                commits = await response.json();
                currentCommitIndex = 0;
                generateFood();
                
                if (gameLoop) clearInterval(gameLoop);
                gameLoop = setInterval(gameStep, gameSpeed);
            } catch (error) {
                console.error('Erro:', error);
                alert('Erro ao buscar commits. Verifique o nome do repositório.');
            }
        }

        function generateFood() {
            if (commits.length === 0) return;
            
            // Pega o próximo commit
            const commit = commits[currentCommitIndex % commits.length];
            currentCommitIndex++;
            
            // Gera posição aleatória para a comida
            const maxX = Math.floor(canvas.width / gridSize) - 1;
            const maxY = Math.floor(canvas.height / gridSize) - 1;
            
            food = {
                x: Math.floor(Math.random() * maxX) * gridSize,
                y: Math.floor(Math.random() * maxY) * gridSize,
                message: commit.commit.message.split('\n')[0].substring(0, 30),
                author: commit.commit.author.name
            };
        }

        function gameStep() {
            // Atualiza a direção
            direction = nextDirection;
            
            // Move a cobra
            const head = {x: snake[0].x, y: snake[0].y};
            
            switch (direction) {
                case 'up':
                    head.y -= gridSize;
                    break;
                case 'down':
                    head.y += gridSize;
                    break;
                case 'left':
                    head.x -= gridSize;
                    break;
                case 'right':
                    head.x += gridSize;
                    break;
            }
            
            // Verifica colisão com as bordas
            if (head.x < 0 || head.x >= canvas.width || head.y < 0 || head.y >= canvas.height) {
                gameOver();
                return;
            }
            
            // Verifica colisão com o próprio corpo
            for (let i = 0; i < snake.length; i++) {
                if (snake[i].x === head.x && snake[i].y === head.y) {
                    gameOver();
                    return;
                }
            }
            
            // Adiciona nova cabeça
            snake.unshift(head);
            
            // Verifica se comeu a comida
            if (head.x === food.x && head.y === food.y) {
                score++;
                scoreDisplay.textContent = Pontuação: ${score};
                generateFood();
                
                // Aumenta a velocidade a cada 5 pontos
                if (score % 5 === 0) {
                    clearInterval(gameLoop);
                    gameSpeed = Math.max(50, gameSpeed - 10);
                    gameLoop = setInterval(gameStep, gameSpeed);
                }
            } else {
                // Remove a cauda se não comeu
                snake.pop();
            }
            
            // Desenha o jogo
            drawGame();
        }

        function drawGame() {
            // Limpa o canvas
            ctx.fillStyle = 'white';
            ctx.fillRect(0, 0, canvas.width, canvas.height);
            
            // Desenha a cobra
            ctx.fillStyle = 'green';
            for (let i = 0; i < snake.length; i++) {
                ctx.fillRect(snake[i].x, snake[i].y, gridSize, gridSize);
                
                // Desenha os olhos na cabeça
                if (i === 0) {
                    ctx.fillStyle = 'black';
                    const eyeSize = gridSize / 5;
                    
                    // Olhos dependendo da direção
                    if (direction === 'right' || direction === 'left') {
                        ctx.fillRect(snake[i].x + (direction === 'right' ? gridSize - eyeSize - 2 : 2), 
                                      snake[i].y + 4, eyeSize, eyeSize);
                        ctx.fillRect(snake[i].x + (direction === 'right' ? gridSize - eyeSize - 2 : 2), 
                                      snake[i].y + gridSize - eyeSize - 4, eyeSize, eyeSize);
                    } else {
                        ctx.fillRect(snake[i].x + 4, 
                                      snake[i].y + (direction === 'down' ? gridSize - eyeSize - 2 : 2), 
                                      eyeSize, eyeSize);
                        ctx.fillRect(snake[i].x + gridSize - eyeSize - 4, 
                                      snake[i].y + (direction === 'down' ? gridSize - eyeSize - 2 : 2), 
                                      eyeSize, eyeSize);
                    }
                    
                    ctx.fillStyle = 'green';
                }
            }
            
            // Desenha a comida (commit)
            ctx.fillStyle = 'red';
            ctx.fillRect(food.x, food.y, gridSize, gridSize);
            
            // Desenha informações do commit
            ctx.fillStyle = 'black';
            ctx.font = '10px Arial';
            ctx.fillText(food.message, food.x, food.y - 5);
            ctx.fillText(food.author, food.x, food.y - 15);
        }

        function gameOver() {
            clearInterval(gameLoop);
            alert(Game Over! Pontuação: ${score});
        }

        // Controles
        document.addEventListener('keydown', function(e) {
            switch (e.key) {
                case 'ArrowUp':
                    if (direction !== 'down') nextDirection = 'up';
                    break;
                case 'ArrowDown':
                    if (direction !== 'up') nextDirection = 'down';
                    break;
                case 'ArrowLeft':
                    if (direction !== 'right') nextDirection = 'left';
                    break;
                case 'ArrowRight':
                    if (direction !== 'left') nextDirection = 'right';
                    break;
            }
        });

        startButton.addEventListener('click', initGame);
    </script>
</body>
</html>


> “O código é poesia escrita em lógica.” – Luciana
