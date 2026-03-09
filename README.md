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
