<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Kite Blockchain Quiz</title>
  <style>
    body {
      font-family: "Poppins", sans-serif;
      background: linear-gradient(135deg, #5e5ce6, #9b8cf3);
      height: 100vh;
      display: flex;
      align-items: center;
      justify-content: center;
      color: #333;
      margin: 0;
    }

    .quiz-container {
      background: white;
      border-radius: 20px;
      box-shadow: 0 10px 30px rgba(0, 0, 0, 0.15);
      width: 90%;
      max-width: 600px;
      padding: 40px;
      text-align: center;
      transition: 0.4s ease;
    }

    h1 {
      color: #5e5ce6;
      margin-bottom: 10px;
    }

    p {
      color: #666;
      font-size: 15px;
    }

    button {
      background-color: #6d63ff;
      border: none;
      color: white;
      padding: 12px 25px;
      border-radius: 10px;
      font-size: 16px;
      cursor: pointer;
      transition: 0.3s;
    }

    button:hover {
      background-color: #4a41d6;
    }

    .option {
      background: #f4f3ff;
      border: 2px solid transparent;
      padding: 12px;
      border-radius: 12px;
      margin: 10px 0;
      cursor: pointer;
      transition: 0.3s;
    }

    .option:hover {
      background: #e8e6ff;
    }

    .option.correct {
      background: #d4edda;
      border-color: #28a745;
    }

    .option.wrong {
      background: #f8d7da;
      border-color: #dc3545;
    }

    .hidden {
      display: none;
    }

    .question {
      font-weight: 600;
      color: #333;
      margin-bottom: 20px;
    }

    .result {
      font-size: 20px;
      font-weight: bold;
      color: #5e5ce6;
    }

    .emoji {
      font-size: 26px;
    }
  </style>
</head>
<body>
  <div class="quiz-container" id="quiz">
    <h1>KITE</h1>
    <p>Blockchain for the Agentic Internet 🚀</p>

    <div id="start-screen">
      <h2>Welcome to the Kite Quiz!</h2>
      <p>Ready to see how much you know about the internet built for AI agents? Let's go!</p>
      <button id="start-btn">Start Quiz</button>
    </div>

    <div id="question-screen" class="hidden">
      <p id="question-number"></p>
      <p class="question" id="question-text"></p>
      <div id="options-container"></div>
      <button id="next-btn" class="hidden">Next Question</button>
    </div>

    <div id="result-screen" class="hidden">
      <h2>🎉 Quiz Completed!</h2>
      <p class="result" id="score-text"></p>
      <p id="reaction"></p>
      <button id="restart-btn">Restart Quiz</button>
    </div>
  </div>

  <script>
    const quizData = [
      {
        question: "🌐 What does Kite aim to bring to the internet?",
        options: [
          "More memes and cat videos 😸",
          "Identity, trust, and scalable payments for AI agents",
          "Faster web browsers",
          "Social media for robots"
        ],
        correct: 1
      },
      {
        question: "🤖 What makes Kite different from traditional blockchains?",
        options: [
          "It focuses on AI agents and the agentic internet",
          "It uses emojis to verify transactions 😂",
          "It runs on quantum computers only",
          "It has no need for smart contracts"
        ],
        correct: 0
      },
      {
        question: "🧩 What does 'EVM-compatible' mean in Kite?",
        options: [
          "It can play video games",
          "It supports Ethereum smart contracts",
          "It replaces Ethereum entirely",
          "It has its own app store"
        ],
        correct: 1
      },
      {
        question: "🔐 Agent Native Identity allows agents to...",
        options: [
          "Share their owner’s reputation securely",
          "Forget passwords daily",
          "Clone themselves infinitely",
          "Create memes autonomously"
        ],
        correct: 0
      },
      {
        question: "💳 Kite’s Agent Native Payments make it possible to...",
        options: [
          "Pay machines instantly with stablecoins",
          "Buy NFTs on Mars",
          "Pay using only Bitcoin",
          "Send payments via email"
        ],
        correct: 0
      },
      {
        question: "🛠️ What’s unique about Kite’s architecture?",
        options: [
          "It’s modular and developers can pick components freely",
          "It only works on old browsers",
          "It uses a single fixed structure for all projects",
          "It doesn’t allow SDK integrations"
        ],
        correct: 0
      },
      {
        question: "⚙️ Proof of AI in Kite helps with...",
        options: [
          "Validating agent contributions and rewards",
          "Detecting human errors",
          "Mining using physical gold",
          "Running 3D games"
        ],
        correct: 0
      },
      {
        question: "🎯 Why should developers build on Kite?",
        options: [
          "Because it has AI-first infrastructure and low latency",
          "Because it mines crypto faster",
          "Because it gives free pizza",
          "Because it doesn’t use smart contracts"
        ],
        correct: 0
      },
      {
        question: "👥 Who can benefit from the Kite ecosystem?",
        options: [
          "AI users, researchers, and blockchain developers",
          "Only gamers",
          "Only government agencies",
          "Only NFT traders"
        ],
        correct: 0
      },
      {
        question: "🚀 The goal of Kite is to build...",
        options: [
          "An AI-native, trustworthy, and scalable internet",
          "A gaming console",
          "A social network for pets",
          "A meme-based blockchain"
        ],
        correct: 0
      }
    ];

    const startBtn = document.getElementById("start-btn");
    const questionScreen = document.getElementById("question-screen");
    const startScreen = document.getElementById("start-screen");
    const resultScreen = document.getElementById("result-screen");
    const questionNumber = document.getElementById("question-number");
    const questionText = document.getElementById("question-text");
    const optionsContainer = document.getElementById("options-container");
    const nextBtn = document.getElementById("next-btn");
    const scoreText = document.getElementById("score-text");
    const restartBtn = document.getElementById("restart-btn");
    const reaction = document.getElementById("reaction");

    let currentQuestion = 0;
    let score = 0;

    startBtn.addEventListener("click", startQuiz);
    nextBtn.addEventListener("click", nextQuestion);
    restartBtn.addEventListener("click", restartQuiz);

    function startQuiz() {
      startScreen.classList.add("hidden");
      questionScreen.classList.remove("hidden");
      loadQuestion();
    }

    function loadQuestion() {
      const current = quizData[currentQuestion];
      questionNumber.textContent = `Question ${currentQuestion + 1} of ${quizData.length}`;
      questionText.textContent = current.question;
      optionsContainer.innerHTML = "";

      current.options.forEach((option, index) => {
        const button = document.createElement("div");
        button.classList.add("option");
        button.textContent = option;
        button.addEventListener("click", () => selectOption(index));
        optionsContainer.appendChild(button);
      });
    }

    function selectOption(selected) {
      const current = quizData[currentQuestion];
      const options = document.querySelectorAll(".option");

      options.forEach((option, index) => {
        option.style.pointerEvents = "none";
        if (index === current.correct) {
          option.classList.add("correct");
        } else if (index === selected && index !== current.correct) {
          option.classList.add("wrong");
        }
      });

      if (selected === current.correct) score++;
      nextBtn.classList.remove("hidden");
    }

    function nextQuestion() {
      currentQuestion++;
      nextBtn.classList.add("hidden");

      if (currentQuestion < quizData.length) {
        loadQuestion();
      } else {
        showResult();
      }
    }

    function showResult() {
      questionScreen.classList.add("hidden");
      resultScreen.classList.remove("hidden");
      scoreText.textContent = `Your Score: ${score} / ${quizData.length}`;

      if (score === quizData.length) reaction.textContent = "🌟 Perfect! You're a true Kite master!";
      else if (score >= 7) reaction.textContent = "🔥 Great job! You understand Kite really well!";
      else if (score >= 4) reaction.textContent = "✨ Not bad! A few more reads and you’ll be a pro!";
      else reaction.textContent = "😅 You might need to revisit the basics of Kite!";
    }

    function restartQuiz() {
      currentQuestion = 0;
      score = 0;
      resultScreen.classList.add("hidden");
      startScreen.classList.remove("hidden");
    }
  </script>
</body>
</html>
