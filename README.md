<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Quiz App</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background: #f4f4f4;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
        }
        .container {
            background: white;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1);
            text-align: center;
            width: 350px;
        }
        h1 {
            margin-bottom: 20px;
        }
        .options {
            display: flex;
            flex-direction: column;
        }
        .option-btn {
            background: #007bff;
            color: white;
            border: none;
            padding: 10px;
            margin: 5px;
            cursor: pointer;
            border-radius: 5px;
            transition: 0.3s;
        }
        .option-btn:hover {
            background: #0056b3;
        }
        .correct {
            background: #28a745 !important;
        }
        .wrong {
            background: #dc3545 !important;
        }
        #next-btn, #restart-btn {
            background: #007bff;
            color: white;
            border: none;
            padding: 10px;
            margin-top: 15px;
            cursor: pointer;
            border-radius: 5px;
            display: none;
        }
        #timer {
            font-size: 18px;
            font-weight: bold;
            margin-top: 15px;
        }
    </style>
</head>
<body>

    <div class="container">
        <h1>Quiz App</h1>
        <p id="question"></p>
        <div id="options" class="options"></div>
        <p id="timer"></p>
        <button id="next-btn">Next</button>
        <button id="restart-btn">Restart</button>
    </div>

    <script>
        document.addEventListener("DOMContentLoaded", () => {
            const quizData = [
                {
                    question: "What does HTML stand for?",
                    options: ["Hyper Text Markup Language", "Hyper Transfer Markup Language", "High Text Machine Learning", "None of the above"],
                    answer: "Hyper Text Markup Language"
                },
                {
                    question: "What does CSS stand for?",
                    options: ["Computer Style Sheets", "Cascading Style Sheets", "Colorful Style Sheets", "Creative Style Sheets"],
                    answer: "Cascading Style Sheets"
                },
                {
                    question: "Which language is used for web development?",
                    options: ["Python", "Java", "JavaScript", "C++"],
                    answer: "JavaScript"
                }
            ];

            let currentQuestionIndex = 0;
            let score = 0;
            let timer;
            let timeLeft = 15;

            const questionElement = document.getElementById("question");
            const optionsElement = document.getElementById("options");
            const nextButton = document.getElementById("next-btn");
            const restartButton = document.getElementById("restart-btn");
            const timerElement = document.getElementById("timer");

            function startQuiz() {
                currentQuestionIndex = 0;
                score = 0;
                showQuestion();
                restartButton.style.display = "none";
            }

            function showQuestion() {
                resetState();
                const currentQuestion = quizData[currentQuestionIndex];
                questionElement.innerText = currentQuestion.question;

                currentQuestion.options.forEach(option => {
                    const button = document.createElement("button");
                    button.innerText = option;
                    button.classList.add("option-btn");
                    button.addEventListener("click", () => selectAnswer(button, currentQuestion.answer));
                    optionsElement.appendChild(button);
                });

                startTimer();
            }

            function selectAnswer(button, correctAnswer) {
                clearInterval(timer);

                if (button.innerText === correctAnswer) {
                    button.classList.add("correct");
                    score++;
                } else {
                    button.classList.add("wrong");
                    document.querySelectorAll(".option-btn").forEach(btn => {
                        if (btn.innerText === correctAnswer) {
                            btn.classList.add("correct");
                        }
                    });
                }

                document.querySelectorAll(".option-btn").forEach(btn => btn.disabled = true);
                nextButton.style.display = "block";
            }

            function resetState() {
                optionsElement.innerHTML = "";
                nextButton.style.display = "none";
                timeLeft = 15;
            }

            function nextQuestion() {
                currentQuestionIndex++;
                if (currentQuestionIndex < quizData.length) {
                    showQuestion();
                } else {
                    endQuiz();
                }
            }

            function startTimer() {
                timerElement.innerText = `Time Left: ${timeLeft}s`;
                timer = setInterval(() => {
                    timeLeft--;
                    timerElement.innerText = `Time Left: ${timeLeft}s`;

                    if (timeLeft === 0) {
                        clearInterval(timer);
                        nextQuestion();
                    }
                }, 1000);
            }

            function endQuiz() {
                clearInterval(timer);
                document.querySelector(".container").innerHTML = `
                    <h1>Quiz Completed!</h1>
                    <p>Your score: ${score} / ${quizData.length}</p>
                    <button id="restart-btn" onclick="restartQuiz()">Restart Quiz</button>
                `;
            }

            function restartQuiz() {
                document.querySelector(".container").innerHTML = `
                    <h1>Quiz App</h1>
                    <p id="question"></p>
                    <div id="options" class="options"></div>
                    <p id="timer"></p>
                    <button id="next-btn" style="display: none;">Next</button>
                    <button id="restart-btn" style="display: none;" onclick="restartQuiz()">Restart Quiz</button>
                `;

                startQuiz();
            }

            nextButton.addEventListener("click", nextQuestion);
            startQuiz();
        });
    </script>

</body>
</html>
