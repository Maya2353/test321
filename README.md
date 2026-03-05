<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Quiz Village Game</title>
    <style>
        body { font-family: Arial, sans-serif; background: #f4f4f4; }
        #gameContainer { width: 80%; margin: auto; padding: 20px; background: white; border-radius: 8px; box-shadow: 0 0 10px rgba(0,0,0,0.1); }
        #quiz { margin-top: 20px; }
        .question { font-size: 1.5em; }
        .answers { list-style: none; padding: 0; }
        .answer { padding: 10px; background: #007bff; color: white; margin: 10px 0; cursor: pointer; border-radius: 5px; }
        .result { font-size: 2em; margin-top: 20px; }
        #village { margin-top: 30px; }
        .building { padding: 15px; background: #28a745; color: white; border-radius: 5px; cursor: pointer; }
        #money { font-size: 1.5em; }
    </style>
</head>
<body>
    <div id="gameContainer">
        <h1>Quiz Village Game</h1>
        <div id="money">Money: $0</div>
        <div id="quiz">
            <div class="question"></div>
            <ul class="answers"></ul>
            <div class="result"></div>
        </div>
        <div id="village">
            <h2>Build Your Village</h2>
            <div class="building">Build House - Cost: $50</div>
            <div class="building">Build Farm - Cost: $100</div>
            <div class="building">Build School - Cost: $200</div>
        </div>
    </div>

    <script>
        const quizQuestions = [
            { question: "What is the capital of France?", answers: ["Berlin", "Madrid", "Paris", "Lisbon"], correct: 2 },
            { question: "What is 2 + 2?", answers: ["3", "4", "5", "6"], correct: 1 },
            { question: "Who wrote 'Hamlet'?", answers: ["Mark Twain", "Charles Dickens", "William Shakespeare", "Leo Tolstoy"], correct: 2 }
        ];

        let currentQuestionIndex = 0;
        let money = 0;

        function loadQuestion() {
            const currentQuestion = quizQuestions[currentQuestionIndex];
            document.querySelector('.question').innerText = currentQuestion.question;
            const answersContainer = document.querySelector('.answers');
            answersContainer.innerHTML = '';
            currentQuestion.answers.forEach((answer, index) => {
                const answerElement = document.createElement('li');
                answerElement.innerText = answer;
                answerElement.classList.add('answer');
                answerElement.onclick = () => selectAnswer(index);
                answersContainer.appendChild(answerElement);
            });
        }

        function selectAnswer(index) {
            const currentQuestion = quizQuestions[currentQuestionIndex];
            if (index === currentQuestion.correct) {
                money += 10;
                document.querySelector('.result').innerText = "Correct!"
            } else {
                document.querySelector('.result').innerText = "Wrong!"
            }
            document.getElementById('money').innerText = `Money: $${money}`;
            currentQuestionIndex++;
            if (currentQuestionIndex < quizQuestions.length) {
                loadQuestion();
            } else {
                document.querySelector('.quiz').style.display = 'none';
                document.querySelector('.result').innerText += ' Quiz completed!';
            }
        }

        document.querySelectorAll('.building').forEach((building, index) => {
            building.onclick = () => build(index);
        });

        function build(index) {
            const costs = [50, 100, 200];
            if (money >= costs[index]) {
                money -= costs[index];
                document.getElementById('money').innerText = `Money: $${money}`;
                building.innerText = 'Built!';
                building.style.backgroundColor = 'gray';
            } else {
                alert('Not enough money!');
            }
        }

        loadQuestion();
    </script>
</body>
</html>
