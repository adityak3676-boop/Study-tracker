# Study-tracker
index.html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Discipline OS V5</title>

<style>

/* ===============================
LEGENDARY UI SYSTEM
=============================== */

body{
margin:0;
background:#000;
color:white;
font-family:Arial;
text-align:center;
transition:.4s;
}

h1{letter-spacing:3px}

.card{
background:#111;
margin:15px auto;
padding:15px;
width:320px;
border-radius:12px;
}

button{
padding:12px 20px;
margin:8px;
border:none;
background:red;
color:white;
cursor:pointer;
border-radius:6px;
}

/* Progress Ring */
.progress{
width:170px;
height:170px;
border-radius:50%;
margin:auto;
display:flex;
align-items:center;
justify-content:center;
font-size:22px;
background:conic-gradient(red 0deg,#222 0deg);
}

/* MONK MODE */
body.monk{
background:black;
cursor:none;
}

/* WAR MODE */
body.war{
background:linear-gradient(135deg,#000,#300);
}

</style>
</head>

<body>

<h1>DISCIPLINE OS V5</h1>

<div class="progress" id="ring">0 XP</div>

<div class="card">
<p id="coachText">Booting Legendary Mode...</p>
</div>

<button id="studyBtn">Study Session</button>
<button onclick="OS.toggleMonk()">Monk Mode</button>

<script>

/* ==================================================
DISCIPLINE OS V5 LEGENDARY ENGINE
================================================== */

const OS={

user:{
name:"Adii",
mission:"NDA",
goal:"Best Percentage"
},

state:{
xp:Number(localStorage.xp)||0,
level:Number(localStorage.level)||1,
streak:Number(localStorage.streak)||0,
discipline:100,
idle:0,
activeSeconds:Number(localStorage.active)||0,
monk:false
},

modules:{},

/* ---------- SYSTEM BOOT ---------- */

init(){

this.identityBoot();
this.installModules();
this.updateRing();
this.startLoops();

},

/* ---------- MODULE SYSTEM ---------- */

register(name,fn){
this.modules[name]=fn;
},

installModules(){
for(const m in this.modules)
this.modules[m](this);
},

/* ---------- VOICE ENGINE ---------- */

speak(text){

document.getElementById("coachText").innerText=text;

const s=new SpeechSynthesisUtterance(text);
s.rate=.95;
s.pitch=.9;

speechSynthesis.cancel();
speechSynthesis.speak(s);
},

/* ---------- IDENTITY BOOT ---------- */

identityBoot(){
setTimeout(()=>{
this.speak(
`Welcome ${this.user.name}.
Mission ${this.user.mission}.`
);
},1500);
},

/* ---------- XP SYSTEM ---------- */

addXP(v){

this.state.xp+=v;

if(this.state.xp>100){
this.state.level++;
this.state.xp=0;
this.speak("LEVEL UP.");
}

localStorage.xp=this.state.xp;
localStorage.level=this.state.level;

this.updateRing();
},

updateRing(){

let deg=this.state.xp*3.6;

document.getElementById("ring")
.style.background=
`conic-gradient(red ${deg}deg,#222 ${deg}deg)`;

document.getElementById("ring")
.innerText=`Lv.${this.state.level}`;
},

/* ---------- MONK MODE ---------- */

toggleMonk(){

this.state.monk=!this.state.monk;

if(this.state.monk){
document.body.classList.add("monk");
document.documentElement.requestFullscreen();
this.speak("Deep Work Activated.");
}else{
document.body.classList.remove("monk");
}
},

/* ---------- MAIN LOOPS ---------- */

startLoops(){

/* Activity Detection */

["mousemove","keydown","click"]
.forEach(e=>{
document.addEventListener(e,()=>this.state.idle=0);
});

setInterval(()=>{

this.state.idle++;

if(this.state.idle===120)
this.speak("Focus slipping.");

if(this.state.idle===300){
this.state.discipline-=5;
this.speak("Discipline decreasing.");
}

},1000);

/* Study Tracking */

setInterval(()=>{
this.state.activeSeconds++;
localStorage.active=this.state.activeSeconds;
},1000);

/* AI Coach */

const msgs=[
"NDA warriors train daily.",
"Consistency builds rank.",
"Silence equals progress.",
"Comfort destroys potential."
];

setInterval(()=>{
if(this.state.idle<60){
this.speak(
msgs[Math.floor(Math.random()*msgs.length)]
);
}
},45000);

/* Exam War Mode */

const exam="2026-05-01";

setInterval(()=>{

const days=(new Date(exam)-Date.now())/86400000;

if(days<60)
document.body.classList.add("war");

},60000);

}

};

/* ===============================
LEGENDARY MODULES
=============================== */

/* Daily Mission Generator */

OS.register("mission",(OS)=>{

const missions=[
"2 Deep Study Sessions",
"Revise Weak Chapter",
"Practice PYQs",
"90 min Focus Block"
];

setTimeout(()=>{
OS.speak("Today's mission: "+
missions[Math.floor(Math.random()*missions.length)]);
},4000);

});

/* Achievement System */

OS.register("achievements",(OS)=>{

if(OS.state.level===5)
OS.speak("Achievement unlocked: Rising Warrior");

});

/* Study Button */

document.getElementById("studyBtn")
.onclick=()=>OS.addXP(15);

/* ===============================
START OS
=============================== */

window.onload=()=>OS.init();

</script>

</body>
</html>


