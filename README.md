<script>

const questions = [
{question:"Was ist die Hauptstadt von Frankreich?",answers:["Berlin","Paris","Rom","Madrid"],correct:1},
{question:"Welches Tier ist das schnellste Landtier?",answers:["Löwe","Gepard","Tiger","Pferd"],correct:1},
{question:"Wie viele Kontinente gibt es?",answers:["5","6","7","8"],correct:2},
{question:"Welcher Planet ist der Sonne am nächsten?",answers:["Merkur","Mars","Erde","Venus"],correct:0},
{question:"Welches Tier kann fliegen?",answers:["Elefant","Adler","Hund","Kuh"],correct:1},
{question:"Wie viele Tage hat eine Woche?",answers:["5","6","7","8"],correct:2},
{question:"Welche Farbe entsteht aus Blau + Gelb?",answers:["Grün","Rot","Orange","Lila"],correct:0},
{question:"Welches Meer liegt zwischen Europa und Afrika?",answers:["Nordsee","Mittelmeer","Ostsee","Atlantik"],correct:1},
{question:"Wie viele Beine hat eine Spinne?",answers:["6","8","10","12"],correct:1},
{question:"Welches Tier lebt im Wasser?",answers:["Hai","Tiger","Löwe","Elefant"],correct:0}
];

let unlockedLevel = 1;
let currentQuestion = 0;
let score = 0;

function generateLevels(){

let html="";

for(let i=1;i<=30;i++){

html+=`<button onclick="startLevel(${i})" ${i>unlockedLevel?"disabled":""}>
Level ${i}
</button>`

}

document.getElementById("levels").innerHTML = html;

}

function startLevel(level){

if(level>unlockedLevel)return;

document.getElementById("levels").style.display="none";
document.getElementById("quiz").classList.remove("hidden");

currentQuestion=0;
score=0;

showQuestion();

}

function showQuestion(){

if(currentQuestion>=5){

finishLevel();
return;

}

let q = questions[Math.floor(Math.random()*questions.length)];

window.current=q;

document.getElementById("question").innerText=q.question;

let html="";

q.answers.forEach((a,i)=>{

html+=`<button onclick="answer(${i})">${a}</button>`

})

document.getElementById("answers").innerHTML=html;

}

function answer(i){

if(i===current.correct){

score++;

}

currentQuestion++;

showQuestion();

}

function finishLevel(){

alert("Du hast "+score+" richtige Antworten");

if(score>=3){

alert("Level geschafft!");

unlockedLevel++;

}else{

alert("Level nicht geschafft");

}

document.getElementById("quiz").classList.add("hidden");
document.getElementById("levels").style.display="block";

generateLevels();

}

generateLevels();

</script>
