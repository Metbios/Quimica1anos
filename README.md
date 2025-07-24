<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <title>Escape Room Atômico</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #1e1e2f;
      color: #fff;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      height: 100vh;
      margin: 0;
    }
    .game-container {
      width: 90%;
      max-width: 800px;
      padding: 20px;
      border-radius: 15px;
      background: #2c2c40;
      box-shadow: 0 0 20px rgba(0,0,0,0.5);
    }
    .question {
      margin-bottom: 20px;
      font-size: 1.2em;
    }
    .options button {
      display: block;
      width: 100%;
      margin: 10px 0;
      padding: 10px;
      font-size: 1em;
      background: #4a4a6a;
      color: #fff;
      border: none;
      border-radius: 5px;
      cursor: pointer;
    }
    .options button:hover {
      background: #6262a5;
    }
    #scoreboard {
      position: absolute;
      top: 10px;
      right: 20px;
      background: #343456;
      padding: 10px 15px;
      border-radius: 10px;
      font-weight: bold;
    }
    #result {
      margin-top: 15px;
      font-weight: bold;
    }
    #restart {
      display: none;
      margin-top: 20px;
      padding: 10px 15px;
      font-size: 1em;
      background: #ff4d4d;
      color: white;
      border: none;
      border-radius: 8px;
      cursor: pointer;
    }
  </style>
</head>
<body>
  <div id="scoreboard">Vidas: <span id="lives">5</span> | Pontos: <span id="score">0</span></div>
  <div class="game-container">
    <div class="question" id="question"></div>
    <div class="options" id="options"></div>
    <div id="result"></div>
    <button id="restart" onclick="restartGame()">Recomeçar</button>
  </div>

  <script>
    const questions = [
      {
        q: "Segundo Dalton, qual das opções melhor representa a indivisibilidade da matéria em sua teoria original?",
        options: ["A matéria é formada por moléculas que se quebram em partes menores.", "O átomo é a menor partícula da matéria e não pode ser dividido.", "Os elementos químicos podem ser transformados em outros por fusão.", "O núcleo é a menor parte da matéria."],
        answer: "O átomo é a menor partícula da matéria e não pode ser dividido."
      },
      {
        q: "No modelo de Dalton, o que justificava a existência de diferentes substâncias químicas?",
        options: ["A mistura de moléculas com massas variáveis.", "A união de átomos diferentes em proporções fixas.", "A presença de elétrons em camadas distintas.", "A combinação de núcleos atômicos com elétrons."],
        answer: "A união de átomos diferentes em proporções fixas."
      },
      {
        q: "Qual crítica é atribuída à teoria de Dalton com base nas descobertas atômicas posteriores?",
        options: ["Ele previu corretamente o conceito de nêutrons.", "Acreditava na existência de partículas subatômicas.", "Considerava os átomos indivisíveis, o que foi refutado.", "Defendia a ideia de nuvem eletrônica."],
        answer: "Considerava os átomos indivisíveis, o que foi refutado."
      },
      {
        q: "Thomson propôs um novo modelo atômico ao descobrir o elétron. Qual a principal característica do seu modelo?",
        options: ["Os elétrons orbitavam um núcleo positivo.", "O átomo era uma esfera positiva com elétrons incrustados.", "Os átomos tinham camadas eletrônicas quantizadas.", "Os átomos eram compostos apenas de nêutrons."],
        answer: "O átomo era uma esfera positiva com elétrons incrustados."
      },
      {
        q: "O experimento da lâmina de ouro, realizado por Rutherford, levou à descoberta de qual estrutura atômica?",
        options: ["Elétrons distribuídos em nuvens.", "Níveis de energia.", "Núcleo atômico concentrado e denso.", "Isótopos de um mesmo elemento."],
        answer: "Núcleo atômico concentrado e denso."
      },
      {
        q: "No modelo de Rutherford, qual foi a principal explicação para a maior parte das partículas atravessarem a lâmina de ouro sem desvio?",
        options: ["Os átomos eram compactos e sólidos.", "A maior parte do átomo era espaço vazio.", "As partículas tinham massa muito pequena.", "Os elétrons repeliam as partículas."],
        answer: "A maior parte do átomo era espaço vazio."
      },
      {
        q: "O que diferencia o modelo de Bohr dos modelos anteriores?",
        options: ["Introduziu o conceito de núcleo com prótons e nêutrons.", "Apresentou níveis energéticos quantizados para os elétrons.", "Negou a existência dos elétrons.", "Propôs que o átomo era uma esfera maciça e neutra."],
        answer: "Apresentou níveis energéticos quantizados para os elétrons."
      },
      {
        q: "Quando um elétron absorve energia e salta para um nível mais externo, esse processo é chamado de:",
        options: ["Ionização.", "Reação nuclear.", "Excitação eletrônica.", "Fissão atômica."],
        answer: "Excitação eletrônica."
      },
      {
        q: "O que ocorre quando um elétron retorna para um nível mais interno após ser excitado?",
        options: ["Ele emite energia na forma de luz.", "Ele desaparece do átomo.", "Ele perde massa.", "Ele libera nêutrons."],
        answer: "Ele emite energia na forma de luz."
      },
      {
        q: "Qual das alternativas representa corretamente a evolução dos modelos atômicos até Bohr?",
        options: ["Dalton → Rutherford → Thomson → Bohr", "Dalton → Thomson → Rutherford → Bohr", "Bohr → Rutherford → Thomson → Dalton", "Thomson → Dalton → Bohr → Rutherford"],
        answer: "Dalton → Thomson → Rutherford → Bohr"
      },
      {
        q: "Qual evidência levou Thomson a propor a existência dos elétrons?",
        options: ["Experimentos com a radioatividade.", "Resultados com lâminas metálicas.", "Experimentos com raios catódicos.", "Estudo da luz visível."],
        answer: "Experimentos com raios catódicos."
      },
      {
        q: "Bohr baseou seu modelo em qual fenômeno observado em espectros atômicos?",
        options: ["Emissão contínua de luz branca.", "Desvio de elétrons por campos magnéticos.", "Linhas espectrais discretas.", "Propagação ondulatória dos prótons."],
        answer: "Linhas espectrais discretas."
      },
      {
        q: "Na visão atual, o modelo de Bohr é considerado...",
        options: ["Completamente superado, sem utilidade.", "Válido apenas para elementos pesados.", "Parcialmente correto e útil para átomos simples.", "O modelo mais aceito até hoje."],
        answer: "Parcialmente correto e útil para átomos simples."
      },
    
      {
        q: "Qual dessas afirmações está de acordo com o modelo atômico aceito atualmente?",
        options: ["O átomo é indivisível.", "Os elétrons giram em órbitas circulares fixas.", "Não se pode determinar com precisão a posição de um elétron.", "O átomo é uma esfera homogênea."],
        answer: "Não se pode determinar com precisão a posição de um elétron."
      }
    ];

    let current = 0;
    let score = 0;
    let lives = 5;

    const questionEl = document.getElementById("question");
    const optionsEl = document.getElementById("options");
    const resultEl = document.getElementById("result");
    const scoreEl = document.getElementById("score");
    const livesEl = document.getElementById("lives");

    function loadQuestion() {
      const q = questions[current];
      questionEl.textContent = q.q;
      optionsEl.innerHTML = "";
      resultEl.textContent = "";
      q.options.forEach(option => {
        const btn = document.createElement("button");
        btn.textContent = option;
        btn.onclick = () => checkAnswer(option);
        optionsEl.appendChild(btn);
      });
    }

    function checkAnswer(choice) {
      if (choice === questions[current].answer) {
        score += 10;
        scoreEl.textContent = score;
        resultEl.textContent = "Correto!";
        current++;
        if (current < questions.length) {
          setTimeout(loadQuestion, 1000);
        } else {
          questionEl.textContent = "Parabéns! Você completou o desafio!";
          optionsEl.innerHTML = "";
          resultEl.textContent = `Pontuação final: ${score}`;
        }
      } else {
        lives--;
        livesEl.textContent = lives;
        resultEl.textContent = "Incorreto! Tente novamente.";
        if (lives <= 0) {
          questionEl.textContent = "Game Over. Você perdeu todas as vidas.";
          optionsEl.innerHTML = "";
          document.getElementById("restart").style.display = "block";
        }
      }
    }

    function restartGame() {
      current = 0;
      score = 0;
      lives = 5;
      scoreEl.textContent = score;
      livesEl.textContent = lives;
      document.getElementById("restart").style.display = "none";
      loadQuestion();
    }

    loadQuestion();
  </script>
</body>
</html>
