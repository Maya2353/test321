<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Quiz Game with Village Building Mechanics</title>
    <style>
        body { font-family: Arial, sans-serif; text-align: center; margin: 20px; }
        #quiz-container { margin: 20px; }
        #village-container { margin: 20px; border: 1px solid #ccc; padding: 10px; }
        .village { display: inline-block; margin: 10px; padding: 10px; border: 1px solid #000; }
    </style>
</head>
<body>
    <h1>Quiz Game with Village Building Mechanics</h1>
    <div id="quiz-container">
        <h2>Quiz</h2>
        <p id="question"></p>
        <button id="answer1"></button>
        <button id="answer2"></button>
        <button id="answer3"></button>
        <button id="answer4"></button>
    </div>
    <div id="village-container">
        <h2>Your Village</h2>
        <div id="villages"></div>
        <button id="build">Build a New Hut</button>
    </div>
    <script>
        const questions = [
            {
                question: "What is the capital of France?",
                answers: ["Berlin", "Madrid", "Paris", "Lisbon"],
                correct: 2
            },
            {
                question: "What is 2 + 2?",
                answers: ["3", "4", "5", "6"],
                correct: 1
            }
        ];
        let currentQuestion = 0;
        let score = 0;
        const questionEl = document.getElementById('question');
        const answerButtons = [
            document.getElementById('answer1'),
            document.getElementById('answer2'),
            document.getElementById('answer3'),
            document.getElementById('answer4')
        ];

        function showQuestion() {
            const q = questions[currentQuestion];
            questionEl.innerText = q.question;
            answerButtons.forEach((button, index) => {
                button.innerText = q.answers[index];
                button.onclick = () => checkAnswer(index);
            });
        }

        function checkAnswer(selected) {
            if (selected === questions[currentQuestion].correct) {
                score++;
                alert('Correct!');
            } else {
                alert('Wrong answer.');
            }
            currentQuestion++;
            if (currentQuestion < questions.length) {
                showQuestion();
            } else {
                alert(`Quiz finished! Your score is ${score}/${questions.length}`);
            }
        }

        const villages = [];
        const villagesContainer = document.getElementById('villages');

        document.getElementById('build').onclick = function() {
            const villageName = `Hut ${villages.length + 1}`;
            villages.push(villageName);
            refreshVillageDisplay();
        };

        function refreshVillageDisplay() {
            villagesContainer.innerHTML = '';
            villages.forEach(village => {
                const div = document.createElement('div');
                div.className = 'village';
                div.innerText = village;
                villagesContainer.appendChild(div);
            });
        }

        showQuestion();
    </script>
</body>
</html>
