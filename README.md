<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Quiz Village Game</title>
    <style>
        body { font-family: Arial, sans-serif; text-align: center; }
        .hidden { display: none; }
        #village { margin-top: 20px; }
        #level-selection { margin-top: 20px; }
        .level { margin: 5px; }
        #answers button { margin: 5px; padding: 10px 20px; }
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
        let levels = Array(9).fill().map((_, index) => ({
            level: index + 1,
            questions: generateQuestions(index + 1),
            unlocked: false
        }));
        levels[0].unlocked = true;

        let currentLevel = 0;
        let currentQuestion = 0;
        let score = 0;

        function generateQuestions(level) {
            return [
                { question: `What is ${level + 1} + ${level + 1}?`, answers: [2 * (level + 1), 2 + level, level + 1, level], correct: 0 },
                { question: `What is ${level} - 1?`, answers: [level - 1, level + 1, level, 0], correct: 0 },
                { question: `How many levels are there?`, answers: [9, 8, level, level + 1], correct: 0 },
                { question: `What does 1 + 1 equal?`, answers: [1, level, 0, 2], correct: 3 },
                { question: `What is ${level} * 2?`, answers: [level * 2, level + 2, level, level - 1], correct: 0 }
            ];
        }

        function startLevel(levelIndex) {
            if (!levels[levelIndex].unlocked) return alert("Level not unlocked");

            currentLevel = levelIndex;
            currentQuestion = 0;
            score = 0;

            document.getElementById("levels").style.display = "none";
            document.getElementById("quiz").classList.remove("hidden");

            displayQuestion();
        }

        function displayQuestion() {
            const questions = levels[currentLevel].questions;

            if (currentQuestion < questions.length) {
                document.getElementById("question").innerText = questions[currentQuestion].question;

                document.getElementById("answers").innerHTML =
                    questions[currentQuestion].answers.map((answer, index) =>
                        `<button onclick="checkAnswer(${index})">${answer}</button>`
                    ).join("");
            } else {
                completeLevel();
            }
        }

        function checkAnswer(index) {
            const questions = levels[currentLevel].questions;

            if (index === questions[currentQuestion].correct) score++;

            currentQuestion++;
            displayQuestion();
        }

        function completeLevel() {
            alert(`Level completed! You answered ${score} questions correctly.`);

            if (score >= 3 && currentLevel + 1 < levels.length) {
                levels[currentLevel + 1].unlocked = true;
                alert("You earned money to repair your village! Next level unlocked.");
            }

            resetGame();
        }

        function resetGame() {
            document.getElementById("quiz").classList.add("hidden");
            document.getElementById("levels").style.display = "block";
            generateLevelButtons();
        }

        function generateLevelButtons() {
            document.getElementById("levels").innerHTML = levels.map((level, i) =>
                `<button class="level" onclick="startLevel(${i})" ${level.unlocked ? '' : 'disabled'}>Level ${level.level}</button>`
            ).join("");
        }

        generateLevelButtons();
    </script>
</body>
</html>
