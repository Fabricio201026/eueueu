# eueueu

<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <title>Jogo de Matemática</title>
  <style>
    body { font-family: Arial, sans-serif; text-align: center; background-color: #f4f4f4; }
    .game-container { margin-top: 50px; }
    .question { font-size: 32px; margin: 20px 0; }
    .feedback { font-size: 24px; margin: 10px; height: 40px; }
    .score, .timer { font-size: 20px; margin: 10px; }
    input { font-size: 24px; padding: 5px; width: 100px; }
    button { font-size: 20px; padding: 10px 20px; }
  </style>
</head>
<body>
  <div class="game-container">
    <h1>Desafio Matemágico!</h1>
    <div class="score">Pontuação: <span id="score">0</span></div>
    <div class="timer">Tempo: <span id="timer">10</span>s</div>
    <div class="question" id="question">3 + 4 = ?</div>
    <input type="number" id="answerInput" autocomplete="off" />
    <button onclick="checkAnswer()">Responder</button>
    <div class="feedback" id="feedback"></div>
    <audio id="correctSound" src="https://www.soundjay.com/button/beep-07.wav"></audio>
    <audio id="wrongSound" src="https://www.soundjay.com/button/beep-10.wav"></audio>
  </div>

  <script>
    let score = 0;
    let timeLeft = 10;
    let timer;
    let currentAnswer;
    let difficulty = 1;

    const questionEl = document.getElementById('question');
    const scoreEl = document.getElementById('score');
    const timerEl = document.getElementById('timer');
    const feedbackEl = document.getElementById('feedback');
    const correctSound = document.getElementById('correctSound');
    const wrongSound = document.getElementById('wrongSound');

    function startGame() {
      score = 0;
      difficulty = 1;
      nextQuestion();
    }

    function generateQuestion() {
      const operations = ['+', '-', 'x'];
      const op = operations[Math.floor(Math.random() * Math.min(operations.length, 1 + difficulty / 2))];
      let a = Math.floor(Math.random() * 10 * difficulty);
      let b = Math.floor(Math.random() * 10 * difficulty);

      let question = `${a} ${op} ${b}`;

      if (op === '+') currentAnswer = a + b;
      else if (op === '-') currentAnswer = a - b;
      else if (op === 'x') currentAnswer = a * b;

      return question;
    }

    function nextQuestion() {
      const newQuestion = generateQuestion();
      questionEl.textContent = `${newQuestion} = ?`;
      feedbackEl.textContent = '';
      document.getElementById('answerInput').value = '';
      document.getElementById('answerInput').focus();
      resetTimer();
    }

    function resetTimer() {
      clearInterval(timer);
      timeLeft = 10;
      timerEl.textContent = timeLeft;
      timer = setInterval(() => {
        timeLeft--;
        timerEl.textContent = timeLeft;
        if (timeLeft <= 0) {
          clearInterval(timer);
          handleWrong();
        }
      }, 1000);
    }

    function checkAnswer() {
      const userAnswer = parseInt(document.getElementById('answerInput').value);
      if (userAnswer === currentAnswer) {
        handleCorrect();
      } else {
        handleWrong();
      }
    }

    function handleCorrect() {
      score++;
      difficulty = Math.floor(score / 3) + 1;
      scoreEl.textContent = score;
      feedbackEl.textContent = 'Muito bem! ✔️';
      feedbackEl.style.color = 'green';
      correctSound.play();
      nextQuestion();
    }

    function handleWrong() {
      feedbackEl.textContent = `Ops! Era ${currentAnswer} ❌`;
      feedbackEl.style.color = 'red';
      wrongSound.play();
      setTimeout(() => nextQuestion(), 2000);
    }

    startGame();
  </script>
</body>
</html>
