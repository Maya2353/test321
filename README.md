<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Quiz Village Game</title>
    <style>
        body { 
            font-family: Arial, sans-serif; 
            text-align: center; 
            background: #f4f4f4;
        }
        .hidden { display: none; }
        #village { margin-top: 20px; }
        #level-selection { margin-top: 20px; }
        .level { 
            margin: 5px; 
            padding: 10px 20px; 
            cursor: pointer; 
        }
        #quiz { 
            margin-top: 30px; 
            background: #fff; 
            display: inline-block; 
            padding: 20px 30px; 
            border-radius: 8px; 
            box-shadow: 0 2px 6px rgba(0,0,0,0.15);
        }
        #answers { margin-top: 15px; }
        #answers button { 
            margin: 5px; 
            padding: 10px 20px; 
            cursor: pointer; 
        }
    </style>
</head>
<body>
    <h1>Welcome to the Quiz Village Game!</h1>

    <div id="village">
        <h2>Your Village</h2>
        <p>Repair your village by completing quiz levels!</p>
    </div>

    <div id="level-selection">
        <h2>Select a Level</h2>
        <div id="levels"></div>
    </div>
    
    <div id="quiz" class="hidden">
        <h2 id="question"></h2>
        <div id="answers"></div>
    </div>

    <script>
        // --- Datenstruktur für Levels und Fragen ---
        let levels = Array(9).fill().map((_, index) => ({
            level: index + 1,
            questions: generateQuestions(index + 1),
            unlocked: false
        }));
        levels[0].unlocked = true; // Level 1 freigeschaltet

        let currentLevel = 0;
        let currentQuestion = 0;
        let score = 0;

        // Jede Frage hat genau EINE richtige Antwort (correct = Index der richtigen Antwort)
        function generateQuestions(level) {
            return [
                { 
                    question: `Level ${level}: What is ${level} + 1?`, 
                    answers: [level + 1, level + 2, level, level - 1], 
                    correct: 1 // nur diese ist richtig
                },
                { 
                    question: `Level ${level}: What is ${level} * 2?`, 
                    answers: [level, level * 2, level + 3, level - 2], 
                    correct: 1 
                },
                { 
                    question: `Level ${level}: What does 1 + 1 equal?`, 
                    answers: [1, 3, 2, 0], 
                    correct: 2 
                }
            ];
        }

        // --- Level starten ---
        function startLevel(levelIndex) {
            if (!levels[levelIndex].unlocked) {
                alert("Level not unlocked");
                return;
            }

            currentLevel = levelIndex;
            currentQuestion = 0;
            score = 0;

            document.getElementById("levels").style.display = "none";
            document.getElementById("quiz").classList.remove("hidden");

            displayQuestion();
        }

        // --- Frage anzeigen ---
        function displayQuestion() {
            const questions = levels[currentLevel].questions;

            if (currentQuestion >= questions.length) {
                completeLevel();
                return;
            }

            const q = questions[currentQuestion];
            const questionEl = document.getElementById("question");
            const answersEl = document.getElementById("answers");

            questionEl.textContent = q.question;
            answersEl.innerHTML = "";

            // Buttons erzeugen und Event Listener anhängen
            q.answers.forEach((answerText, index) => {
                const btn = document.createElement("button");
                btn.textContent = answerText;
                btn.addEventListener("click", () => {
                    checkAnswer(index);
                });
                answersEl.appendChild(btn);
            });
        }

        // --- Antwort prüfen ---
        function checkAnswer(selectedIndex) {
            const questions = levels[currentLevel].questions;
            const q = questions[currentQuestion];

            // genau eine richtige Antwort
            if (selectedIndex === q.correct) {
                score++;
                alert("Correct!");
            } else {
                alert("Wrong!");
            }

            currentQuestion++;
            displayQuestion();
        }

        // --- Level abschließen ---
        function completeLevel() {
            alert(`Level ${levels[currentLevel].level} completed! You answered ${score} questions correctly.`);

            // Nächstes Level freischalten, wenn mind. 2 richtige Antworten
            if (score >= 2 && currentLevel + 1 < levels.length) {
                levels[currentLevel + 1].unlocked = true;
                alert("You earned money to repair your village! Next level unlocked.");
            }

            resetGame();
        }

        // --- Zurück zur Levelauswahl ---
        function resetGame() {
            document.getElementById("quiz").classList.add("hidden");
            document.getElementById("levels").style.display = "block";
            generateLevelButtons();
        }

        // --- Level-Buttons erzeugen ---
        function generateLevelButtons() {
            const levelsContainer = document.getElementById("levels");
            levelsContainer.innerHTML = "";

            levels.forEach((level, i) => {
                const btn = document.createElement("button");
                btn.textContent = `Level ${level.level}`;
                btn.className = "level";
                btn.disabled = !level.unlocked;
                btn.addEventListener("click", () => startLevel(i));
                levelsContainer.appendChild(btn);
            });
        }

        // Initiale Buttons erzeugen
        generateLevelButtons();
    </script>
</body>
</html>
