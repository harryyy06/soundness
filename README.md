<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Kite AI Quiz</title>
  <style>
    body {
      font-family: "Poppins", sans-serif;
      background-color: #f4f1ea;
      color: #3a2f2a;
      display: flex;
      align-items: center;
      justify-content: center;
      height: 100vh;
      margin: 0;
    }

    .quiz-container {
      background: #fffaf4;
      border-radius: 20px;
      box-shadow: 0 8px 20px rgba(0, 0, 0, 0.1);
      width: 90%;
      max-width: 500px;
      padding: 30px;
      text-align: center;
    }

    .logo {
      width: 80px;
      height: 80px;
      margin-bottom: 10px;
    }

    h1 {
      font-size: 28px;
      color: #4b3a2e;
      margin-bottom: 20px;
    }

    button {
      background-color: #c6a77b;
      color: white;
      border: none;
      padding: 12px 24px;
      border-radius: 12px;
      font-size: 16px;
      cursor: pointer;
      transition: all 0.2s ease;
    }

    button:hover {
      background-color: #b08c64;
    }

    .question {
      font-size: 18px;
      margin-bottom: 20px;
    }

    .option {
      display: block;
      background: #f7efe3;
      border: 1px solid #d8c7a4;
      border-radius: 12px;
      padding: 10px;
      margin: 10px 0;
      cursor: pointer;
      transition: 0.2s;
    }

    .option:hover {
      background: #e9dcc2;
    }

    .correct {
      background-color: #a8d5ba !important;
      border-color: #7fbf8e;
    }

    .incorrect {
      background-color: #e9a8a8 !important;
      border-color: #d37a7a;
    }

    .hidden {
      display: none;
    }

    .score {
      font-size: 22px;
      margin-top: 20px;
      color: #4b3a2e;
    }
  </style>
</head>
<body>
  <div class="quiz-container">
    <img src="https://i.ibb.co/9V3Z9tz/logo.png" alt="Kite Logo" class="logo" />
    <h1>KITE AI Quiz</h1>
    <div id="quiz">
      <div id="welcome-screen">
        <p>Welcome to the <b>Kite AI Knowledge Quiz!</b><br><br>
        Test how well you know about Kite — the blockchain for the agentic internet 🪶</p>
        <button id="start-btn">Start Quiz</button>
      </div>

      <div id="question-container" class="hidden">
        <div id="question" class="question"></div>
        <div id="options"></div>
        <button id="next-btn" class="hidden">Next Question</button>
      </div>

      <div id="result" class="hidden">
        <h2>🎯 Quiz Completed!</h2>
        <p class="score" id="score"></p>
        <button id="restart-btn">Restart Quiz</button>
      </div>
    </div>
  </div>

  <script>
    const quizData = [
      {
        question: "🤖 What does Kite primarily focus on?",
        options: [
          "Gaming NFTs",
          "AI and Agentic Internet",
          "Social Media Tokens",
          "DeFi Yield Farming",
        ],
        answer: 1,
      },
      {
        question: "🔐 What is 'Agent Native Identity' in Kite?",
        options: [
          "A new blockchain consensus mechanism",
          "Portable identity for agents across apps",
          "A payment wallet for humans",
          "A 2FA system for dApps",
        ],
        answer: 1,
      },
      {
        question: "⚙️ Kite is compatible with which virtual machine?",
        options: ["KiteVM", "SolanaVM", "EVM", "MoveVM"],
        answer: 2,
      },
      {
        question: "💳 What type of settlements does Kite support?",
        options: [
          "Crypto-to-fiat only",
          "High-fee token transfers",
          "Stablecoin-native instant micropayments",
          "Only BTC transactions",
        ],
        answer: 2,
      },
      {
        question: "🧠 What is Proof of AI used for in Kite?",
        options: [
          "Attribution and rewards for AI contributions",
          "Energy-efficient mining",
          "Random block generation",
          "Social reputation tracking",
        ],
        answer: 0,
      },
      {
        question: "🧩 Kite’s architecture is designed to be:",
        options: [
          "Rigid and centralized",
          "Modular and flexible",
          "Private and isolated",
          "Layer-0 only",
        ],
        answer: 1,
      },
      {
        question: "🌐 What does MCP stand for in Kite’s ecosystem?",
        options: [
          "Model Context Protocol",
          "Machine Code Platform",
          "Multi Chain Processing",
          "Main Chain Protocol",
        ],
        answer: 0,
      },
      {
        question: "⚡ What kind of blockchain is Kite?",
        options: [
          "Layer-2 Optimistic Rollup",
          "Sovereign Layer-1",
          "Sidechain of Ethereum",
          "Private Consortium Chain",
        ],
        answer: 1,
      },
      {
        question: "👥 Which audience does Kite primarily target?",
        options: [
          "AI users and blockchain developers",
          "Casual gamers",
          "Social media influencers",
          "Traditional banks",
        ],
        answer: 0,
      },
      {
        question: "💼 What kind of payments infrastructure does Kite eliminate?",
        options: [
          "Simple wallet transfers",
          "Overcomplicated, high-fee systems",
          "Mobile recharge systems",
          "Centralized fiat rails",
        ],
        answer: 1,
      },
    ];

    const startBtn = document.getElementById("start-btn");
    const questionContainer = document.getElementById("question-container");
    const questionEl = document.getElementById("question");
    const optionsEl = document.getElementById("options");
    const nextBtn = document.getElementById("next-btn");
    const resultEl = document.getElementById("result");
    const scoreEl = document.getElementById("score");
    const restartBtn = document.getElementById("restart-btn");
    const welcomeScreen = document.getElementById("welcome-screen");

    let currentQuestion = 0;
    let score = 0;

    startBtn.addEventListener("click", () => {
      welcomeScreen.classList.add("hidden");
      questionContainer.classList.remove("hidden");
      loadQuestion();
    });

    function loadQuestion() {
      const q = quizData[currentQuestion];
      questionEl.textContent = q.question;
      optionsEl.innerHTML = "";
      q.options.forEach((opt, i) => {
        const btn = document.createElement("div");
        btn.classList.add("option");
        btn.textContent = opt;
        btn.addEventListener("click", () => checkAnswer(i, btn));
        optionsEl.appendChild(btn);
      });
    }

    function checkAnswer(selected, btn) {
      const correct = quizData[currentQuestion].answer;
      const options = document.querySelectorAll(".option");
      options.forEach(o => o.style.pointerEvents = "none");

      if (selected === correct) {
        btn.classList.add("correct");
        score++;
      } else {
        btn.classList.add("incorrect");
        options[correct].classList.add("correct");
      }

      nextBtn.classList.remove("hidden");
    }

    nextBtn.addEventListener("click", () => {
      currentQuestion++;
      if (currentQuestion < quizData.length) {
        nextBtn.classList.add("hidden");
        loadQuestion();
      } else {
        showResult();
      }
    });

    function showResult() {
      questionContainer.classList.add("hidden");
      resultEl.classList.remove("hidden");
      scoreEl.textContent = `You scored ${score} out of ${quizData.length} 🎉`;
    }

    restartBtn.addEventListener("click", () => {
      currentQuestion = 0;
      score = 0;
      resultEl.classList.add("hidden");
      welcomeScreen.classList.remove("hidden");
    });
  </script>
</body>
</html>
