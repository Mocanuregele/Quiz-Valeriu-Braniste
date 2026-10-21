
<!DOCTYPE html>
<html lang="ro">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Quiz Master</title>

<style>
* { box-sizing: border-box; }

body {
  margin: 0;
  font-family: Arial, sans-serif;
  background: linear-gradient(135deg, #10152f, #263c75);
  color: white;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 20px;
}

.card {
  background: #ffffff12;
  border: 1px solid #ffffff30;
  border-radius: 22px;
  padding: 28px;
  width: 100%;
  max-width: 520px;
  text-align: center;
  box-shadow: 0 15px 45px #0005;
}

h1 { color: #70e1ff; margin-bottom: 8px; }
.subtitle { color: #d5defb; }

.stats {
  display: flex;
  justify-content: space-between;
  margin: 24px 0;
  gap: 10px;
}

.stat {
  background: #ffffff15;
  padding: 12px;
  border-radius: 12px;
  flex: 1;
}

#question { font-size: 21px; margin: 24px 0; }

button {
  width: 100%;
  padding: 13px;
  margin: 7px 0;
  border: none;
  border-radius: 10px;
  font-size: 16px;
  font-weight: bold;
  cursor: pointer;
  transition: 0.2s;
}

.answer {
  background: #ffffff;
  color: #182449;
}

.answer:hover { background: #70e1ff; transform: scale(1.02); }

#feedback { min-height: 25px; font-weight: bold; }

#next, #restart {
  background: #70e1ff;
  color: #10152f;
}

#next:disabled { opacity: 0.4; cursor: not-allowed; }

#progress {
  height: 8px;
  background: #ffffff25;
  border-radius: 10px;
  overflow: hidden;
}

#bar {
  height: 100%;
  width: 0%;
  background: #70e1ff;
  transition: width 0.3s;
}

.hidden { display: none; }
</style>
</head>

<body>
<div class="card">
  <h1>🎮 QUIZ MASTER</h1>
  <p class="subtitle">Testează-ți inteligența!</p>

  <div class="stats">
    <div class="stat">🏆 Scor<br><b id="score">0</b></div>
    <div class="stat">❓ Întrebarea<br><b id="number">1/8</b></div>
  </div>

  <div id="progress"><div id="bar"></div></div>

  <div id="game">
    <h2 id="question"></h2>
    <div id="answers"></div>
    <p id="feedback"></p>
    <button id="next" disabled onclick="nextQuestion()">Următoarea ➜</button>
  </div>

  <div id="result" class="hidden">
    <h2>🏆 Joc terminat!</h2>
    <p id="finalScore"></p>
    <p id="message"></p>
    <button id="restart" onclick="restartGame()">🔄 Joacă din nou</button>
  </div>

  <p class="subtitle">Creat pentru educație și distracție 💡</p>
</div>

<script>
const questions = [
  {
    q: "Ce înseamnă CPU?",
    a: ["Placă video", "Unitate centrală de procesare", "Memorie externă"],
    correct: 1
  },
  {
    q: "Care dintre acestea este un browser?",
    a: ["Google Chrome", "Windows", "PowerPoint"],
    correct: 0
  },
  {
    q: "Ce înseamnă HTML?",
    a: ["Un joc video", "Un limbaj pentru structura paginilor web", "Un antivirus"],
    correct: 1
  },
  {
    q: "Care este un dispozitiv de intrare?",
    a: ["Monitorul", "Imprimanta", "Tastatura"],
    correct: 2
  },
  {
    q: "Ce înseamnă AI?",
    a: ["Inteligență artificială", "Acces internet", "Imagine animată"],
    correct: 0
  },
  {
    q: "Care dintre acestea este un sistem de operare?",
    a: ["YouTube", "Windows", "Instagram"],
    correct: 1
  },
  {
    q: "La ce folosește parola?",
    a: ["La protejarea conturilor", "La mărirea ecranului", "La încărcarea telefonului"],
    correct: 0
  },
  {
    q: "Ce este internetul?",
    a: ["Un singur calculator", "O aplicație de desen", "O rețea mondială de calculatoare"],
    correct: 2
  }
];

let current = 0;
let score = 0;
let answered = false;

function showQuestion() {
  answered = false;
  const item = questions[current];

  document.getElementById("question").textContent = item.q;
  document.getElementById("number").textContent =
    (current + 1) + "/" + questions.length;
  document.getElementById("score").textContent = score;
  document.getElementById("feedback").textContent = "";
  document.getElementById("next").disabled = true;
  document.getElementById("bar").style.width =
    (current / questions.length * 100) + "%";

  const answers = document.getElementById("answers");
  answers.innerHTML = "";

  item.a.forEach((answer, index) => {
    const button = document.createElement("button");
    button.className = "answer";
    button.textContent = answer;
    button.onclick = () => checkAnswer(index, button);
    answers.appendChild(button);
  });
}

function checkAnswer(index, button) {
  if (answered) return;
  answered = true;

  const item = questions[current];
  const buttons = document.querySelectorAll(".answer");

  buttons.forEach(b => b.disabled = true);

  if (index === item.correct) {
    score += 10;
    button.style.background = "#42e695";
    document.getElementById("feedback").textContent =
      "✅ Corect! Ai câștigat 10 puncte!";
  } else {
    button.style.background = "#ff7777";
    buttons[item.correct].style.background = "#42e695";
    document.getElementById("feedback").textContent =
      "❌ Greșit! Răspunsul corect este evidențiat.";
  }

  document.getElementById("score").textContent = score;
  document.getElementById("next").disabled = false;
}

function nextQuestion() {
  current++;

  if (current < questions.length) {
    showQuestion();
  } else {
    document.getElementById("game").classList.add("hidden");
    document.getElementById("result").classList.remove("hidden");
    document.getElementById("bar").style.width = "100%";
    document.getElementById("finalScore").textContent =
      "Scor final: " + score + " / 80 puncte";

    let message;
    if (score === 80) message = "🌟 Perfect! Ești campion!";
    else if (score >= 50) message = "👏 Foarte bine! Continuă să înveți!";
    else message = "💪 Mai exersează și încearcă din nou!";

    document.getElementById("message").textContent = message;
  }
}

function restartGame() {
  current = 0;
  score = 0;
  document.getElementById("game").classList.remove("hidden");
  document.getElementById("result").classList.add("hidden");
  showQuestion();
}

showQuestion();
</script>
</body>
</html>
