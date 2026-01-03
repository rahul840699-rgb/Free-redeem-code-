<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Premium Scratch & Redeem</title>

<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600;700&display=swap" rel="stylesheet">

<style>
*{margin:0;padding:0;box-sizing:border-box;font-family:'Poppins',sans-serif}
body{
    min-height:100vh;
    display:flex;
    align-items:center;
    justify-content:center;
    background:
      radial-gradient(circle at top,#1e293b,#020617 60%),
      linear-gradient(135deg,#0f172a,#020617);
    overflow:hidden;
    color:#fff;
}
.bg-glow{
    position:absolute;
    width:500px;
    height:500px;
    background:radial-gradient(circle,#38bdf8,transparent 70%);
    filter:blur(120px);
    opacity:.25;
}
.wrapper{
    width:100%;
    max-width:420px;
    padding:15px;
    z-index:2;
}
.ad{
    background:rgba(255,255,255,.04);
    border:1px dashed rgba(255,255,255,.15);
    border-radius:14px;
    padding:16px;
    text-align:center;
    font-size:13px;
    color:#cbd5f5;
    margin-bottom:15px;
    backdrop-filter:blur(10px);
}
.container{
    background:linear-gradient(180deg,rgba(255,255,255,.08),rgba(255,255,255,.02));
    border:1px solid rgba(255,255,255,.15);
    backdrop-filter:blur(18px);
    padding:24px;
    border-radius:22px;
    box-shadow:0 30px 80px rgba(0,0,0,.6);
    text-align:center;
}
.logo{
    font-size:22px;
    font-weight:700;
    letter-spacing:.5px;
}
.tagline{
    font-size:13px;
    color:#cbd5f5;
    margin-bottom:18px;
}
.card{
    position:relative;
    width:100%;
    height:170px;
    border-radius:18px;
    overflow:hidden;
    background:linear-gradient(135deg,#fde68a,#f59e0b);
    box-shadow:inset 0 0 0 2px rgba(255,255,255,.4);
}
.code{
    position:absolute;
    inset:0;
    display:flex;
    align-items:center;
    justify-content:center;
    font-size:26px;
    font-weight:700;
    color:#111;
    letter-spacing:3px;
}
canvas{
    position:absolute;
    inset:0;
    cursor:pointer;
}
.info{
    margin:14px 0;
    font-size:13px;
    color:#cbd5f5;
}
.actions{
    display:none;
    gap:10px;
}
.btn{
    flex:1;
    padding:12px;
    border:none;
    border-radius:12px;
    font-size:14px;
    font-weight:600;
    cursor:pointer;
    transition:.3s;
}
.copy{
    background:linear-gradient(135deg,#22c55e,#16a34a);
    color:#052e16;
}
.new{
    background:linear-gradient(135deg,#3b82f6,#2563eb);
    color:#fff;
}
.btn:hover{
    transform:translateY(-2px);
}
.footer{
    margin-top:14px;
    font-size:11px;
    color:#94a3b8;
}
.confetti{
    position:fixed;
    width:8px;
    height:8px;
    background:red;
    top:-10px;
    animation:fall linear forwards;
}
@keyframes fall{
    to{
        transform:translateY(110vh) rotate(720deg);
        opacity:0;
    }
}
</style>
</head>

<body>

<div class="bg-glow"></div>

<div class="wrapper">

    <div class="ad">Ad Space (Top Banner)</div>

    <div class="container">
        <div class="logo">Premium Rewards</div>
        <div class="tagline">Scratch & unlock your exclusive code</div>

        <div class="card">
            <div class="code" id="redeemCode">K9X7-Q2P8-MZ4</div>
            <canvas id="scratch"></canvas>
        </div>

        <div class="info">Scratch to reveal & celebrate 🎉</div>

        <div class="actions" id="actions">
            <button class="btn copy" onclick="copyCode()">Copy Code</button>
            <button class="btn new" onclick="resetScratch()">New Code</button>
        </div>

        <div class="footer">© 2026 Premium Scratch</div>
    </div>

    <div class="ad">Ad Space (Bottom)</div>

</div>

<script>
const canvas=document.getElementById("scratch");
const ctx=canvas.getContext("2d");
const card=document.querySelector(".card");
const actions=document.getElementById("actions");

function resizeCanvas(){
    canvas.width=card.offsetWidth;
    canvas.height=card.offsetHeight;
    ctx.globalCompositeOperation="source-over";
    ctx.fillStyle="#94a3b8";
    ctx.fillRect(0,0,canvas.width,canvas.height);
    ctx.globalCompositeOperation="destination-out";
    actions.style.display="none";
}
resizeCanvas();
window.addEventListener("resize",resizeCanvas);

let drawing=false;
let scratched=0;
let popped=false;

canvas.addEventListener("mousedown",()=>drawing=true);
canvas.addEventListener("mouseup",()=>drawing=false);
canvas.addEventListener("mousemove",scratch);

canvas.addEventListener("touchstart",()=>drawing=true);
canvas.addEventListener("touchend",()=>drawing=false);
canvas.addEventListener("touchmove",scratch);

function scratch(e){
    if(!drawing) return;
    const rect=canvas.getBoundingClientRect();
    const x=(e.clientX||e.touches[0].clientX)-rect.left;
    const y=(e.clientY||e.touches[0].clientY)-rect.top;
    ctx.beginPath();
    ctx.arc(x,y,20,0,Math.PI*2);
    ctx.fill();
    scratched++;
    if(scratched>100 && !popped){
        popped=true;
        actions.style.display="flex";
        popConfetti();
    }
}

function popConfetti(){
    for(let i=0;i<80;i++){
        const c=document.createElement("div");
        c.className="confetti";
        c.style.left=Math.random()*100+"vw";
        c.style.background=`hsl(${Math.random()*360},90%,60%)`;
        c.style.animationDuration=2+Math.random()*2+"s";
        document.body.appendChild(c);
        setTimeout(()=>c.remove(),4000);
    }
}

function copyCode(){
    navigator.clipboard.writeText(document.getElementById("redeemCode").innerText);
    alert("Code copied successfully!");
}

function resetScratch(){
    const chars="ABCDEFGHJKLMNPQRSTUVWXYZ23456789";
    let code="";
    for(let i=0;i<12;i++){
        code+=chars[Math.floor(Math.random()*chars.length)];
        if(i==3||i==7) code+="-";
    }
    document.getElementById("redeemCode").innerText=code;
    scratched=0;
    popped=false;
    resizeCanvas();
}
</script>

</body>
</html>
