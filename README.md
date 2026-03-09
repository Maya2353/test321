<!DOCTYPE html>
<html>
<head>
    <title>Quiz Village Game</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <h1>Quiz Village Game</h1>
    <div id="game"></div>
    <script>
        const levels = [
            { level: 1, questions: [
                { question: 'What is the capital of France?', options: ['Berlin', 'Madrid', 'Paris', 'Rome'], answer: 'Paris' },
                { question: 'What is the fastest land animal?', options: ['Cheetah', 'Lion', 'Tiger', 'Leopard'], answer: 'Cheetah' },
                { question: 'Who was the first president of the United States?', options: ['George Washington', 'Abraham Lincoln', 'Thomas Jefferson', 'John Adams'], answer: 'George Washington' },
                { question: 'What is the chemical symbol for water?', options: ['H2O', 'CO2', 'O2', 'NaCl'], answer: 'H2O' },
                { question: 'In which continent is Egypt located?', options: ['Asia', 'Africa', 'Europe', 'America'], answer: 'Africa' }
            ] },
            // Similar structure for levels 2 to 30
        ];

        function loadLevel(level) {
            const levelData = levels.find(l => l.level === level);
            // Logic to display questions for the level
        }
        
        // Initialize game
        function startGame() {
            loadLevel(1);
        }
        
        startGame();
    </script>
</body>
</html>
