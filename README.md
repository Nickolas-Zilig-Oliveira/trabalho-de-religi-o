<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Jogo Cara a Cara Bíblico</title>
    <style>
        body {
            #question {
        font-size: 32px; /* Aumentando o tamanho da fonte */
        color: #D32F2F; /* Cor vermelha */
        margin-bottom: 20px;

            }
            font-family: 'Arial', sans-serif;
            background-color: #f4f4f4;
            color: #333;
            margin: 0;
            padding: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            flex-direction: column;
        }

        #game-container {
            text-align: center;
            width: 100%;
            max-width: 1500px; /* Aumentando a largura */
            padding: 20px;
            box-sizing: border-box;
        }

        h1 {
            font-size: 3em;
            color: #4CAF50;
        }

        .character-card {
            background-color: #fff;
            margin: 10px;
            padding: 20px;
            width: 220px;
            display: inline-block;
            border-radius: 8px;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
            cursor: pointer;
            transition: transform 0.2s ease-in-out;
        }

        .character-card:hover {
            transform: scale(1.05);
        }

        .character-image {
            width: 150px;
            height: 200px;
            object-fit: cover;
            border-radius: 8px;
            margin-bottom: 10px;
        }

        .button {
            padding: 10px 20px;
            font-size: 16px;
            background-color: #4CAF50;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            margin: 20px;
            transition: background-color 0.3s ease;
        }

        .button:hover {
            background-color: #45a049;
        }

        .hidden {
            display: none;
        }

        #question {
            font-size: 22px;
            margin-bottom: 20px;
        }

        #result {
            font-size: 24px;
            font-weight: bold;
            color: #D32F2F;
        }

        #score {
            font-size: 18px;
            font-weight: bold;
            margin-bottom: 20px;
        }

        #end-screen {
            font-size: 20px;
            font-weight: bold;
            color: #4CAF50;
        }

        #countdown {
            font-size: 30px;
            color: #FF5722;
            font-weight: bold;
            margin-top: 10px;
        }

        /* Animação de sucesso */
        @keyframes successAnimation {
            0% {
                color: #4CAF50;
                transform: scale(1);
            }
            50% {
                color: #FFEB3B;
                transform: scale(1.2);
            }
            100% {
                color: #4CAF50;
                transform: scale(1);
            }
        }

        .success {
            animation: successAnimation 1s ease-in-out;
        }
    </style>
</head>
<body>
    <div id="game-container">
        <h1>Jogo Cara a Cara Bíblico</h1>
        <div id="score">Pontuação: 0</div>
        <div id="question">Quem é o personagem bíblico?</div>
        <div id="character-cards"></div>
        <button class="button hidden" id="next-round">Próxima rodada</button>
        <div id="result" class="hidden"></div>
        <div id="end-screen" class="hidden"></div>
        <div id="countdown" class="hidden"></div>
    </div>

    <script>
        const characters = [
            {
                name: 'Moisés',
                image: 'https://1.bp.blogspot.com/-yCpoeLw4LLU/T0qmQAQcmxI/AAAAAAAAScs/o6TPmS5Sylw/s1600/f42018810.jpg',
                clues: ['Liderou os israelitas na fuga do Egito', 'Recebeu os Dez Mandamentos', 'Partiu o Mar Vermelho'],
            },
            {
                name: 'Davi',
                image: 'https://upload.wikimedia.org/wikipedia/commons/thumb/b/b6/Gerard_van_Honthorst_-_King_David_Playing_the_Harp_-_Google_Art_Project.jpg/800px-Gerard_van_Honthorst_-_King_David_Playing_the_Harp_-_Google_Art_Project.jpg',
                clues: ['Derrotou Golias com uma pedra', 'Foi um rei de Israel', 'Compôs muitos salmos'],
            },
            {
                name: 'Maria',
                image: 'https://www.portaloracao.com/wp-content/uploads/2020/07/oracao-virgem-maria-930x620.jpg',
                clues: ['Mãe de Jesus', 'Virgem Maria', 'Fugiu para o Egito com o filho para escapar de Herodes'],
            },
            {
                name: 'Paulo',
                image: 'https://4.bp.blogspot.com/-QPzNgBt62T4/Wk17YssjC0I/AAAAAAAAB6A/H-vob6gvDRYBfOBqsIYg4de0EI_xukNVwCLcBGAs/s1600/e1e1ad60f07c4aa3ccbcb2973e9d7007_XL.jpg',
                clues: ['Saulo se converteu no caminho para Damasco', 'Escreveu muitas cartas do Novo Testamento', 'Foi um missionário do cristianismo'],
            },
            {
                name: 'Pedro',
                image: 'https://cdn.pixabay.com/photo/2024/05/07/20/59/ai-generated-8746777_1280.png',
                clues: ['Era um pescador', 'Negou Jesus três vezes', 'É considerado o primeiro papa'],
            },
            {
                name: 'Noé',
                image: 'https://estiloadoracao.com/wp-content/uploads/2015/10/Quem-foi-No%C3%A9.jpg',
                clues: ['Construiu a arca', 'Sobreviveu ao Dilúvio', 'Teve três filhos: Sem, Cão e Jafé'],
            },
            {
                name: 'Abraão',
                image: 'https://hayaperegrinaciones.com/wp-content/uploads/2022/02/historia-de-abraham.jpg',
                clues: ['Foi o pai de Isaque', 'Fez um pacto com Deus', 'Foi chamado para ser o pai de uma grande nação'],
            },
            {
                name: 'Sara',
                image: 'https://abibliadavida.com.br/wp-content/uploads/2024/05/Quem-foi-Sara-na-Biblia-1.png',
                clues: ['Esposa de Abraão', 'Mãe de Isaque', 'Teve um filho na velhice'],
            },
            {
                name: 'José',
                image: 'https://th.bing.com/th/id/R.06b88391671db7f6db21d1dbf87af506?rik=grTFl6yWsE2H%2fg&riu=http%3a%2f%2fchristianitymalaysia.com%2fwp%2fwp-content%2fuploads%2f2013%2f08%2fjoseph_coat_colors_bible_hero_poster.jpg&ehk=W1UHhf7YSGGx1AaoZPJlKmKJSeRhvJyZN%2fHf%2f5FerKQ%3d&risl=&pid=ImgRaw&r=0',
                clues: ['Foi vendido pelos seus irmãos', 'Era filho de Jacó', 'Interpretou os sonhos do Faraó'],
            },
            {
                name: 'Eliseu',
                image: 'https://agrestepresbiteriano.com.br/wp-content/uploads/2023/07/Eliseu4.png',
                clues: ['Foi sucessor de Elias', 'Fez muitos milagres', 'Curou Naamã da lepra'],
            }
        ];

        let currentCharacter = null;
        let score = 0;
        let remainingCharacters = [...characters];
        const questionElement = document.getElementById("question");
        const characterCardsElement = document.getElementById("character-cards");
        const nextRoundButton = document.getElementById("next-round");
        const resultElement = document.getElementById("result");
        const scoreElement = document.getElementById("score");
        const endScreenElement = document.getElementById("end-screen");
        const countdownElement = document.getElementById("countdown");

        let countdownInterval;

        function startRound() {
            resultElement.classList.add('hidden');
            nextRoundButton.classList.add('hidden');
            countdownElement.classList.add('hidden');
            clearInterval(countdownInterval); // Limpa qualquer contagem regressiva anterior
            countdownElement.innerHTML = '';

            characterCardsElement.innerHTML = '';

            if (remainingCharacters.length === 0) {
                endScreenElement.innerHTML = `Parabéns, você fez um total de ${score} pontos! Você é um expert!`;
                endScreenElement.classList.remove('hidden');
                return;
            }

            const randomIndex = Math.floor(Math.random() * remainingCharacters.length);
            currentCharacter = remainingCharacters[randomIndex];
            remainingCharacters.splice(randomIndex, 1); // Remove o personagem escolhido

            let clueIndex = 0;

            questionElement.innerHTML = `Quem é o personagem bíblico? <br> Dica: ${currentCharacter.clues[clueIndex]}`;

            characters.forEach((char, index) => {
                const card = document.createElement("div");
                card.classList.add("character-card");
                card.innerHTML = `
                    <img class="character-image" src="${char.image}" alt="${char.name}">
                    <p>${char.name}</p>
                `;
                card.onclick = function() {
                    if (char.name === currentCharacter.name) {
                        score++;
                        showResult(true);
                    } else {
                        showResult(false);
                    }
                };
                characterCardsElement.appendChild(card);
            });

            startCountdown(15); // 15 segundos de tempo para a rodada
        }

        function showResult(isCorrect) {
            if (isCorrect) {
                resultElement.innerHTML = `Você acertou! +1 ponto. Pontuação: ${score}`;
                resultElement.classList.add('success');
            } else {
                resultElement.innerHTML = `Você errou. O personagem era: ${currentCharacter.name}`;
            }
            resultElement.classList.remove('hidden');
            nextRoundButton.classList.remove('hidden');
            scoreElement.innerHTML = `Pontuação: ${score}`;
            clearInterval(countdownInterval); // Congela o cronômetro ao responder
        }

        function startCountdown(seconds) {
            countdownElement.classList.remove('hidden');
            let timeLeft = seconds;
            countdownElement.innerHTML = `Tempo restante: ${timeLeft}s`;

            countdownInterval = setInterval(function() {
                timeLeft--;
                countdownElement.innerHTML = `Tempo restante: ${timeLeft}s`;
                if (timeLeft <= 0) {
                    clearInterval(countdownInterval);
                    showResult(false);
                }
            }, 1000);
        }

        nextRoundButton.onclick = startRound;

        startRound(); // Inicia o jogo
    </script>
</body>
</html>
