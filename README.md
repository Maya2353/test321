<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Village Quiz Game</title>
    <style>
        body { font-family: Arial, sans-serif; }
        #village { width: 100%; height: auto; }
        #question { margin-top: 20px; }
        #answers { margin-top: 20px; }
    </style>
</head>
<body>
    <h1>Welcome to the Village Quiz Game!</h1>
    <img id="village" src="https://via.placeholder.com/600x300?text=Village" alt="Village Image">
    <div id="question"></div>
    <div id="answers"></div>
    <script>
        const questions = [
            { question: "What is the capital of France?", answers: ["Paris", "London", "Berlin"], correct: 0 },
            { question: "What is 2 + 2?", answers: ["3", "4", "5"], correct: 1 },
            { question: "What color is the sky?", answers: ["Blue", "Green", "Red"], correct: 0 }
        ];
        let currentQuestion = 0;
        
        function loadQuestion() {
            const q = questions[currentQuestion];
            document.getElementById('question').innerText = q.question;
            const answersDiv = document.getElementById('answers');
            answersDiv.innerHTML = '';
            q.answers.forEach((answer, index) => {
                const button = document.createElement('button');
                button.innerText = answer;
                button.onclick = () => checkAnswer(index);
                answersDiv.appendChild(button);
            });
        }
        
        function checkAnswer(selectedIndex) {
            if (selectedIndex === questions[currentQuestion].correct) {
                alert('Correct!');
                currentQuestion++;
                if (currentQuestion < questions.length) {
                    loadQuestion();
                } else {
                    document.body.innerHTML = '<h1>Congratulations! You completed the quiz!</h1>';
                }
            } else {
                alert('Wrong answer! Try again.');
            }
        }
        
        loadQuestion();
    </script>
</body>
</html>
