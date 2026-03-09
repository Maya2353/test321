<!DOCTYPE html>
<html lang="de">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Quiz Village Game</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: Arial, sans-serif;
            background-color: #87CEEB;
            height: 100vh;
            overflow: hidden;
        }

        .container {
            width: 100%;
            height: 100vh;
            display: flex;
        }

        /* DORF ANSICHT */
        .village-view {
            width: 100%;
            height: 100%;
            background: linear-gradient(to bottom, #87CEEB 0%, #90EE90 100%);
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            position: relative;
        }

        .village-view.hidden {
            display: none;
        }

        .village-title {
            font-size: 36px;
            font-weight: bold;
            color: #333;
            margin-bottom: 30px;
        }

        .village-scene {
            display: flex;
            align-items: flex-end;
            gap: 40px;
            margin-bottom: 40px;
        }

        .hut {
            width: 150px;
            height: 150px;
            background-color: #8B4513;
            border: 3px solid #654321;
            border-radius: 50% 50% 0 0;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 24px;
            color: white;
            box-shadow: 0 5px 15px rgba(0,0,0,0.3);
        }

        .hut.broken {
            opacity: 0.6;
            background-color: #6B3410;
        }

        .hut.repaired {
            background-color: #A0522D;
            opacity: 1;
        }

        .repair-bar {
            width: 200px;
            height: 30px;
            background-color: #ddd;
            border: 2px solid #333;
            border-radius: 5px;
            margin-top: 20px;
            overflow: hidden;
        }

        .repair-progress {
            height: 100%;
            background-color: #4CAF50;
            width: 0%;
            transition: width 0.3s;
        }

        .village-stats {
            position: absolute;
            top: 20px;
            left: 20px;
            background-color: rgba(255, 255, 255, 0.9);
            padding: 15px 25px;
            border-radius: 10px;
            font-size: 18px;
            font-weight: bold;
            color: #333;
            box-shadow: 0 3px 10px rgba(0,0,0,0.2);
        }

        .village-stats div {
            margin: 5px 0;
        }

        .money-display {
            color: #FFD700;
            font-size: 20px;
        }

        .nav-button {
            background-color: #FF6B6B;
            color: white;
            border: none;
            padding: 15px 30px;
            font-size: 16px;
            border-radius: 8px;
            cursor: pointer;
            margin: 10px;
            transition: 0.3s;
            box-shadow: 0 3px 10px rgba(0,0,0,0.2);
        }

        .nav-button:hover {
            background-color: #FF5252;
            transform: scale(1.05);
        }

        .nav-button:active {
            transform: scale(0.98);
        }

        /* LEVEL MENÜ */
        .levels-view {
            width: 100%;
            height: 100%;
            background: linear-gradient(to bottom, #667eea 0%, #764ba2 100%);
            display: none;
            flex-direction: column;
            padding: 40px;
            overflow-y: auto;
            position: relative;
        }

        .levels-view.active {
            display: flex;
        }

        .levels-header {
            text-align: center;
            color: white;
            margin-bottom: 40px;
        }

        .levels-header h1 {
            font-size: 40px;
            margin-bottom: 10px;
        }

        .levels-grid {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 30px;
            max-width: 1000px;
            margin: 0 auto;
            padding-bottom: 50px;
        }

        .level-card {
            aspect-ratio: 1;
            border-radius: 15px;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            font-size: 24px;
            font-weight: bold;
            cursor: pointer;
            transition: 0.3s;
            box-shadow: 0 5px 20px rgba(0,0,0,0.3);
            position: relative;
            border: 3px solid transparent;
        }

        .level-card.unlocked {
            background: linear-gradient(135deg, #4CAF50, #45a049);
            color: white;
        }

        .level-card.unlocked:hover {
            transform: scale(1.08);
            box-shadow: 0 8px 25px rgba(0,0,0,0.4);
        }

        .level-card.locked {
            background: linear-gradient(135deg, #999, #777);
            color: #333;
            cursor: not-allowed;
        }

        .level-card.completed {
            background: linear-gradient(135deg, #FFD700, #FFA500);
            color: white;
            border: 3px solid #FF6B6B;
        }

        .lock-icon {
            font-size: 40px;
            margin-bottom: 10px;
        }

        .level-number {
            font-size: 32px;
            margin-bottom: 5px;
        }

        .level-reward {
            font-size: 14px;
            opacity: 0.8;
        }

        .back-button {
            position: absolute;
            top: 20px;
            left: 20px;
            background-color: #FF6B6B;
            color: white;
            border: none;
            padding: 12px 24px;
            font-size: 16px;
            border-radius: 8px;
            cursor: pointer;
            transition: 0.3s;
        }

        .back-button:hover {
            background-color: #FF5252;
        }

        /* QUIZ ANSICHT */
        .quiz-view {
            width: 100%;
            height: 100%;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            display: none;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            padding: 20px;
            position: relative;
        }

        .quiz-view.active {
            display: flex;
        }

        .quiz-container {
            background-color: white;
            border-radius: 15px;
            padding: 40px;
            max-width: 600px;
            width: 100%;
            box-shadow: 0 10px 40px rgba(0,0,0,0.3);
        }

        .quiz-header {
            text-align: center;
            margin-bottom: 30px;
            color: #333;
        }

        .quiz-header h2 {
            font-size: 28px;
            margin-bottom: 10px;
        }

        .progress-bar {
            width: 100%;
            height: 8px;
            background-color: #ddd;
            border-radius: 5px;
            margin-bottom: 20px;
            overflow: hidden;
        }

        .progress-fill {
            height: 100%;
            background-color: #667eea;
            transition: width 0.3s;
        }

        .question-text {
            font-size: 20px;
            color: #333;
            margin-bottom: 30px;
            font-weight: bold;
        }

        .answers-container {
            display: flex;
            flex-direction: column;
            gap: 12px;
        }

        .answer-button {
            padding: 15px 20px;
            font-size: 16px;
            border: 2px solid #667eea;
            background-color: white;
            color: #667eea;
            border-radius: 8px;
            cursor: pointer;
            transition: 0.3s;
            font-weight: bold;
        }

        .answer-button:hover {
            background-color: #f0f0f0;
            transform: translateX(5px);
        }

        .answer-button:disabled {
            cursor: not-allowed;
        }

        .answer-button.correct {
            background-color: #4CAF50;
            color: white;
            border-color: #4CAF50;
        }

        .answer-button.incorrect {
            background-color: #FF6B6B;
            color: white;
            border-color: #FF6B6B;
        }

        .feedback {
            text-align: center;
            font-size: 18px;
            font-weight: bold;
            margin-top: 20px;
            min-height: 30px;
        }

        .feedback.correct {
            color: #4CAF50;
        }

        .feedback.incorrect {
            color: #FF6B6B;
        }

        .results-container {
            text-align: center;
            background-color: white;
            border-radius: 15px;
            padding: 40px;
            max-width: 500px;
            width: 100%;
            box-shadow: 0 10px 40px rgba(0,0,0,0.3);
        }

        .results-container h2 {
            font-size: 32px;
            color: #333;
            margin-bottom: 20px;
        }

        .results-stats {
            font-size: 20px;
            color: #667eea;
            margin-bottom: 30px;
            font-weight: bold;
        }

        .results-stats div {
            margin: 10px 0;
        }

        .results-buttons {
            display: flex;
            gap: 15px;
            justify-content: center;
        }

        .results-button {
            padding: 12px 30px;
            font-size: 16px;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            font-weight: bold;
            transition: 0.3s;
        }

        .results-button.retry {
            background-color: #FF6B6B;
            color: white;
        }

        .results-button.retry:hover {
            background-color: #FF5252;
        }

        .results-button.back {
            background-color: #667eea;
            color: white;
        }

        .results-button.back:hover {
            background-color: #5568d3;
        }

        .quiz-back-button {
            position: absolute;
            top: 20px;
            left: 20px;
            background-color: #FF6B6B;
            color: white;
            border: none;
            padding: 12px 24px;
            font-size: 16px;
            border-radius: 8px;
            cursor: pointer;
            transition: 0.3s;
        }

        .quiz-back-button:hover {
            background-color: #FF5252;
        }
    </style>
</head>
<body>
    <div class="container">
        <!-- DORF -->
        <div class="village-view" id="villageView">
            <div class="village-stats">
                <div>💰 <span class="money-display">Geld: <span id="villageMoneyDisplay">0</span>€</span></div>
                <div>🏘️ Dorf-Level: <span id="villageLevel">1</span></div>
            </div>

            <h1 class="village-title">Mein Dorf</h1>

            <div class="village-scene">
                <div class="hut broken" id="hut">
                    <span>🏠</span>
                </div>
                <div>
                    <div class="repair-bar">
                        <div class="repair-progress" id="repairProgress"></div>
                    </div>
                    <div style="text-align: center; color: #333; font-weight: bold;">
                        Reparatur: <span id="repairText">0%</span>
                    </div>
                </div>
            </div>

            <button class="nav-button" onclick="goToLevels()">📚 Zu den Quiz-Leveln</button>
        </div>

        <!-- LEVEL MENÜ -->
        <div class="levels-view" id="levelsView">
            <button class="back-button" onclick="goToVillage()">← Zurück zum Dorf</button>
            <div class="levels-header">
                <h1>Quiz Level</h1>
                <p>Klicke auf ein Level, um zu spielen!</p>
            </div>
            <div class="levels-grid" id="levelsGrid"></div>
        </div>

        <!-- QUIZ -->
        <div class="quiz-view" id="quizView">
            <button class="quiz-back-button" onclick="goToLevels()">← Zurück zu Leveln</button>
            <div class="quiz-container" id="quizContainer"></div>
        </div>
    </div>

    <script>
        // GAME STATE
        let gameState = {
            money: 0,
            villageLevel: 1,
            hutRepairProgress: 0,
            completedLevels: [],
            currentLevel: null,
            currentQuestionIndex: 0,
            correctAnswers: 0
        };

        // QUIZ DATEN
        const quizLevels = [
            {
                id: 1,
                name: "Level 1: Anfänger",
                reward: 100,
                questions: [
                    { type: "truefalse", question: "Paris ist die Hauptstadt von Frankreich.", correct: true },
                    { type: "multiple", question: "Wie viele Kontinente gibt es?", answers: ["5", "6", "7", "8"], correct: 2 },
                    { type: "truefalse", question: "Der Mond ist größer als die Erde.", correct: false },
                    { type: "multiple", question: "Welcher Planet ist der größte?", answers: ["Saturn", "Jupiter", "Neptun"], correct: 1 },
                    { type: "truefalse", question: "Wasser kocht bei 100°C.", correct: true }
                ]
            },
            {
                id: 2,
                name: "Level 2: Fortgeschritten",
                reward: 150,
                questions: [
                    { type: "multiple", question: "Wer schrieb 'Faust'?", answers: ["Schiller", "Goethe", "Heine"], correct: 1 },
                    { type: "truefalse", question: "Die Chinesische Mauer ist von Weltraum sichtbar.", correct: false },
                    { type: "multiple", question: "Welches ist das kleinste Land der Welt?", answers: ["Monaco", "Vatikanstadt", "Liechtenstein"], correct: 1 },
                    { type: "truefalse", question: "Honig verdirbt nie.", correct: true },
                    { type: "multiple", question: "Wie viele Saiten hat eine Gitarre normalerweise?", answers: ["5", "6", "7"], correct: 1 }
                ]
            },
            {
                id: 3,
                name: "Level 3: Experte",
                reward: 200,
                questions: [
                    { type: "multiple", question: "In welchem Jahr fiel die Berliner Mauer?", answers: ["1987", "1988", "1989"], correct: 2 },
                    { type: "truefalse", question: "Alle Schlangen sind giftig.", correct: false },
                    { type: "multiple", question: "Wer war der erste Präsident der USA?", answers: ["Jefferson", "Washington", "Lincoln"], correct: 1 },
                    { type: "truefalse", question: "Quecksilber ist das einzige Metall, das flüssig ist.", correct: true },
                    { type: "multiple", question: "Wie viele Zähne hat ein erwachsener Mensch?", answers: ["28", "32", "36"], correct: 1 }
                ]
            },
            {
                id: 4,
                name: "Level 4: Meister",
                reward: 250,
                questions: [
                    { type: "multiple", question: "Welcher Wissenschaftler erhielt zwei Nobelpreise?", answers: ["Marie Curie", "Albert Einstein", "Niels Bohr"], correct: 0 },
                    { type: "truefalse", question: "Das Licht ist schneller als der Schall.", correct: true },
                    { type: "multiple", question: "Wie viele Strings hat eine Violine?", answers: ["3", "4", "5"], correct: 1 },
                    { type: "truefalse", question: "Bananen wachsen auf Bäumen.", correct: false },
                    { type: "multiple", question: "Welcher Berg ist der höchste der Welt?", answers: ["K2", "Mount Everest", "Kangchenjunga"], correct: 1 }
                ]
            },
            {
                id: 5,
                name: "Level 5: Legende",
                reward: 300,
                questions: [
                    { type: "multiple", question: "Wie heißt die Kunstströmung von Salvador Dalí?", answers: ["Kubismus", "Surrealismus", "Expressionismus"], correct: 1 },
                    { type: "truefalse", question: "Die Antarktis ist ein Land.", correct: false },
                    { type: "multiple", question: "Wer komponierte die 9. Sinfonie?", answers: ["Mozart", "Bach", "Beethoven"], correct: 2 },
                    { type: "truefalse", question: "Koalas sind Bären.", correct: false },
                    { type: "multiple", question: "Wie viele Kammern hat das menschliche Herz?", answers: ["2", "3", "4"], correct: 2 }
                ]
            }
        ];

        // INITIALISIERUNG
        function init() {
            loadGameState();
            renderVillage();
            renderLevels();
        }

        function loadGameState() {
            const saved = localStorage.getItem('quizVillageGameState');
            if (saved) {
                gameState = JSON.parse(saved);
            }
        }

        function saveGameState() {
            localStorage.setItem('quizVillageGameState', JSON.stringify(gameState));
        }

        // VILLAGE
        function renderVillage() {
            const villageView = document.getElementById('villageView');
            const quizView = document.getElementById('quizView');
            const levelsView = document.getElementById('levelsView');

            villageView.classList.remove('hidden');
            quizView.classList.remove('active');
            levelsView.classList.remove('active');

            document.getElementById('villageMoneyDisplay').textContent = gameState.money;
            document.getElementById('villageLevel').textContent = gameState.villageLevel;

            const progress = gameState.hutRepairProgress;
            document.getElementById('repairProgress').style.width = progress + '%';
            document.getElementById('repairText').textContent = progress + '%';

            if (progress === 100) {
                document.getElementById('hut').classList.remove('broken');
                document.getElementById('hut').classList.add('repaired');
            }
        }

        // LEVELS
        function renderLevels() {
            const grid = document.getElementById('levelsGrid');
            grid.innerHTML = '';

            quizLevels.forEach((level, index) => {
                const isCompleted = gameState.completedLevels.includes(level.id);
                const isUnlocked = index === 0 || gameState.completedLevels.includes(quizLevels[index - 1].id);

                const card = document.createElement('div');
                card.className = 'level-card';

                if (isCompleted) {
                    card.classList.add('completed');
                } else if (isUnlocked) {
                    card.classList.add('unlocked');
                } else {
                    card.classList.add('locked');
                }

                let content = `
                    <div class="level-number">${level.id}</div>
                    <div>${level.name}</div>
                    <div class="level-reward">💰 ${level.reward}€</div>
                `;

                if (!isUnlocked) {
                    content = `<div class="lock-icon">🔒</div>${content}`;
                }

                if (isCompleted) {
                    content += '<div style="margin-top: 10px; font-size: 20px;">✅</div>';
                }

                card.innerHTML = content;

                if (isUnlocked) {
                    card.onclick = () => startQuiz(level.id);
                }

                grid.appendChild(card);
            });
        }

        function goToLevels() {
            const levelsView = document.getElementById('levelsView');
            const villageView = document.getElementById('villageView');
            const quizView = document.getElementById('quizView');

            villageView.classList.add('hidden');
            quizView.classList.remove('active');
            levelsView.classList.add('active');

            renderLevels();
        }

        function goToVillage() {
            saveGameState();
            const villageView = document.getElementById('villageView');
            const levelsView = document.getElementById('levelsView');

            levelsView.classList.remove('active');
            villageView.classList.remove('hidden');

            renderVillage();
        }

        // QUIZ
        function startQuiz(levelId) {
            gameState.currentLevel = quizLevels.find(l => l.id === levelId);
            gameState.currentQuestionIndex = 0;
            gameState.correctAnswers = 0;

            const quizView = document.getElementById('quizView');
            const levelsView = document.getElementById('levelsView');

            levelsView.classList.remove('active');
            quizView.classList.add('active');

            showQuestion();
        }

        function showQuestion() {
            const level = gameState.currentLevel;
            const question = level.questions[gameState.currentQuestionIndex];
            const container = document.getElementById('quizContainer');

            const progress = ((gameState.currentQuestionIndex + 1) / level.questions.length) * 100;

            let answersHTML = '';

            if (question.type === 'truefalse') {
                answersHTML = `
                    <button class="answer-button" onclick="answerQuestion(true)">✅ Wahr</button>
                    <button class="answer-button" onclick="answerQuestion(false)">❌ Falsch</button>
                `;
            } else if (question.type === 'multiple') {
                question.answers.forEach((answer, index) => {
                    answersHTML += `<button class="answer-button" onclick="answerQuestion(${index})">${answer}</button>`;
                });
            }

            container.innerHTML = `
                <div class="quiz-header">
                    <h2>${level.name}</h2>
                    <p>Frage ${gameState.currentQuestionIndex + 1} von ${level.questions.length}</p>
                </div>
                <div class="progress-bar">
                    <div class="progress-fill" style="width: ${progress}%"></div>
                </div>
                <div class="question-text">${question.question}</div>
                <div class="answers-container">
                    ${answersHTML}
                </div>
                <div class="feedback" id="feedback"></div>
            `;
        }

        function answerQuestion(selectedAnswer) {
            const level = gameState.currentLevel;
            const question = level.questions[gameState.currentQuestionIndex];
            const isCorrect = selectedAnswer === question.correct;

            const feedback = document.getElementById('feedback');
            const buttons = document.querySelectorAll('.answer-button');

            buttons.forEach((button, index) => {
                button.disabled = true;

                if (isCorrect) {
                    if (question.type === 'truefalse') {
                        if ((selectedAnswer === true && index === 0) || (selectedAnswer === false && index === 1)) {
                            button.classList.add('correct');
                        }
                    } else {
                        if (index === selectedAnswer) {
                            button.classList.add('correct');
                        }
                    }
                } else {
                    if (question.type === 'truefalse') {
                        if ((question.correct === true && index === 0) || (question.correct === false && index === 1)) {
                            button.classList.add('correct');
                        }
                        if ((selectedAnswer === true && index === 0) || (selectedAnswer === false && index === 1)) {
                            button.classList.add('incorrect');
                        }
                    } else {
                        if (index === question.correct) {
                            button.classList.add('correct');
                        }
                        if (index === selectedAnswer) {
                            button.classList.add('incorrect');
                        }
                    }
                }
            });

            if (isCorrect) {
                feedback.textContent = '✅ Richtig!';
                feedback.classList.add('correct');
                gameState.correctAnswers++;
            } else {
                feedback.textContent = '❌ Falsch! Die richtige Antwort ist markiert.';
                feedback.classList.add('incorrect');
            }

            // NÄCHSTE FRAGE NACH 1,5 SEKUNDEN
            setTimeout(() => {
                gameState.currentQuestionIndex++;

                if (gameState.currentQuestionIndex < level.questions.length) {
                    showQuestion();
                } else {
                    showResults();
                }
            }, 1500);
        }

        function showResults() {
            const level = gameState.currentLevel;
            const passed = gameState.correctAnswers >= 3;
            const container = document.getElementById('quizContainer');

            let resultHTML = `
                <div class="results-container">
                    <h2>${passed ? '🎉 Level bestanden!' : '❌ Level nicht bestanden!'}</h2>
                    <div class="results-stats">
                        <div>Richtige Antworten: <strong>${gameState.correctAnswers}/5</strong></div>
                        <div>Verdientes Geld: <strong>${passed ? level.reward : 0}€</strong></div>
                    </div>
                    <div class="results-buttons">
                        <button class="results-button retry" onclick="startQuiz(${level.id})">🔄 Wiederholen</button>
                        <button class="results-button back" onclick="goToLevels()">← Zurück zu Leveln</button>
                    </div>
                </div>
            `;

            container.innerHTML = resultHTML;

            if (passed) {
                if (!gameState.completedLevels.includes(level.id)) {
                    gameState.completedLevels.push(level.id);
                    gameState.money += level.reward;
                    gameState.hutRepairProgress = Math.min(100, gameState.hutRepairProgress + 20);

                    saveGameState();
                }
            }
        }

        // START
        init();
    </script>
</body>
</html>
