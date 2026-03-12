<!DOCTYPE html>
<html lang="de">
<head>
<meta charset="UTF-8">
<title>Quiz Game</title>

<style>
body{
    font-family: Arial;
    text-align:center;
}

.level{
    margin:5px;
    padding:10px 20px;
}

#answers button{
    display:block;
    margin:10px auto;
    padding:10px 20px;
}

.hidden{
    display:none;
}
</style>

</head>
<body>

<h1>Quiz Spiel</h1>

<div id="levels"></div>

<div id="quiz" class="hidden">
    <h2 id="question"></h2>
    <div id="answers"></div>
</div>

<script>

const allQuestions = [

{
question:"Was ist die Hauptstadt von Deutschland?",
answers:["Berlin","Hamburg","München","Köln"],
correct:0
},

{
question:"Welches Tier ist das schnellste Landtier?",
answers:["Löwe","Gepard","Tiger","Pferd"],
correct:1
},

{
question:"Wie viele Kontinente gibt es?",
answers:["5","6","7","8"],
correct:2
},

{
question:"Welcher Planet ist der Sonne am nächsten?",
answers:["Merkur","Mars","Erde","Venus"],
correct:0
},

{
question:"Welches Tier kann fliegen?",
answers:["Elefant","Adler","Hund","Kuh"],
correct:1
},

{
question:"Wie viele Tage hat eine Woche?",
answers:["5","6","7","8"],
correct:2
},

{
question:"Welche Farbe entsteht aus Blau + Gelb?",
answers:["Grün","Rot","Orange","Lila"],
correct:0
},

{
question:"Welches Meer liegt zwischen Europa und Afrika?",
answers:["Nordsee","Mittelmeer","Ostsee","Atlantik"],
correct:1
},

{
question:"Wie viele Beine hat eine Spinne?",
answers:["6","8","10","12"],
correct:1
},

{
question:"Welches Tier lebt im Wasser?",
answers:["Hai","Tiger","Löwe","Elefant"],
correct:0
},

{
question:"Welches Land ist bekannt für Pizza?",
answers:["Spanien","Italien","Frankreich","Portugal"],
correct:1
},

{
question:"Wie viele Monate hat ein Jahr?",
answers:["10","11","12","13"],
correct:2
},

{
question:"Welches Tier miaut?",
answers:["Hund","Katze","Kuh","Pferd"],
correct:1
},

{
question:"Welcher Planet ist der größte?",
answers:["Mars","Erde","Jupiter","Venus"],
correct:2
},

{
question:"Welche Farbe hat eine Banane?",
answers:["Blau","Gelb","Rot","Grün"],
correct:1
}

]

const LEVEL_COUNT = 30
const QUESTIONS_PER_LEVEL = 5

let unlockedLevel = 1
let currentLevel = 0
let questionIndex = 0
let score = 0

function generateLevels(){

let html=""

for(let i=1;i<=LEVEL_COUNT;i++){

html += `<button class="level" onclick="startLevel(${i})" ${i>unlockedLevel?'disabled':''}>
Level ${i}
</button>`

}

document.getElementById("levels").innerHTML = html

}

function startLevel(level){

if(level>unlockedLevel) return

currentLevel = level
questionIndex = 0
score = 0

document.getElementById("levels").style.display="none"
document.getElementById("quiz").classList.remove("hidden")

showQuestion()

}

function showQuestion(){

if(questionIndex>=QUESTIONS_PER_LEVEL){

finishLevel()
return

}

let q = allQuestions[Math.floor(Math.random()*allQuestions.length)]

window.currentQuestion=q

document.getElementById("question").innerText=q.question

let html=""

q.answers.forEach((a,i)=>{

html+=`<button onclick="answer(${i})">${a}</button>`

})

document.getElementById("answers").innerHTML=html

}

function answer(i){

if(i===currentQuestion.correct){

score++

}

questionIndex++

showQuestion()

}

function finishLevel(){

alert("Du hattest "+score+" richtige Antworten")

if(score>=3){

alert("Level geschafft!")

if(unlockedLevel<LEVEL_COUNT){

unlockedLevel++

}

}else{

alert("Level nicht geschafft. Versuche es nochmal.")

}

document.getElementById("quiz").classList.add("hidden")
document.getElementById("levels").style.display="block"

generateLevels()

}

generateLevels()

</script>

</body>
</html>
