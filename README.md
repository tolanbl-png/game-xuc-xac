<!DOCTYPE html>
<html lang="vi">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Xúc Xắc Đúng – Sai | Toán 9</title>
<style>
:root{--bg:#0b1020;--panel:#151d35;--panel2:#1d2948;--text:#f4f7ff;--muted:#aab6d6;--blue:#70a5ff;--green:#39d98a;--red:#ff647c;--gold:#ffd166}
*{box-sizing:border-box}
body{margin:0;min-height:100vh;background:radial-gradient(circle at top,#1b2d55,#0b1020 58%);font-family:system-ui,Segoe UI,Arial,sans-serif;color:var(--text);display:flex;justify-content:center;padding:24px}
.app{width:min(900px,100%)}
header{text-align:center;margin-bottom:20px}
h1{font-size:clamp(28px,5vw,44px);margin:4px 0 8px;letter-spacing:-1px}
header p{margin:0;color:var(--muted)}
.pill{display:inline-block;border:1px solid #34466e;border-radius:999px;padding:5px 12px;color:#c7d8ff;font-size:13px;margin-bottom:8px}
.stats{display:grid;grid-template-columns:repeat(3,1fr);gap:12px;margin-bottom:14px}
.stat,.card{background:rgba(21,29,53,.94);border:1px solid #2b3a60;border-radius:18px;padding:16px;box-shadow:0 12px 32px #0002}
.stat span{display:block;color:var(--muted);font-size:13px}.stat strong{font-size:25px}
.game{display:grid;grid-template-columns:1fr 1.35fr;gap:14px}
.dice-area{display:flex;flex-direction:column;align-items:center;justify-content:center;min-height:320px;background:linear-gradient(145deg,#1d2d52,#131b32)}
.dice-row{display:flex;gap:16px;align-items:center;justify-content:center;margin:12px 0 22px}
.die{width:90px;height:90px;border-radius:19px;background:linear-gradient(145deg,#fff,#dce6ff);box-shadow:0 9px 0 #a2b6df,0 15px 25px #0005;display:grid;grid-template:repeat(3,1fr)/repeat(3,1fr);padding:13px;gap:5px;transform:rotate(-4deg)}
.die:nth-child(2){transform:rotate(5deg)}
.dot{border-radius:50%;background:#1c2b4d;opacity:0}
.dot.on{opacity:1}
.dice-label{color:var(--muted);font-size:13px;text-align:center}
button{border:0;border-radius:12px;padding:12px 18px;font-weight:750;font-size:15px;cursor:pointer;transition:transform .12s,filter .12s}
button:hover{filter:brightness(1.08);transform:translateY(-1px)}
button:disabled{opacity:.45;cursor:not-allowed;transform:none}
.primary{background:var(--blue);color:#07142d}
.answer{flex:1;background:#243252;color:white;border:1px solid #435780}
.answer.correct{background:var(--green);color:#062617}
.answer.wrong{background:var(--red);color:#350711}
.question-card{display:flex;flex-direction:column;gap:14px}
.question-top{display:flex;justify-content:space-between;align-items:center;color:var(--muted);font-size:13px}
.question{font-size:clamp(20px,3vw,27px);line-height:1.45;font-weight:750;text-align:center;padding:18px 4px}
.rule{background:#0d152a;border:1px solid #28395f;border-radius:12px;padding:12px;color:#c8d6f7;font-size:14px}
.answers{display:flex;gap:10px}
.feedback{min-height:58px;border-radius:12px;padding:12px;background:#0d152a;color:var(--muted);line-height:1.45}
.feedback.good{color:#a3ffd0;border:1px solid #24694d}.feedback.bad{color:#ffb5c0;border:1px solid #7c3444}
.bottom{display:flex;gap:10px;justify-content:space-between;align-items:center;margin-top:14px;flex-wrap:wrap}
.secondary{background:#243252;color:#dce7ff;border:1px solid #435780}
.note{font-size:12px;color:var(--muted);margin-top:18px;text-align:center}
@keyframes tumble{0%{transform:rotate(0) scale(1)}25%{transform:rotate(95deg) scale(.8)}50%{transform:rotate(190deg) scale(1.1)}75%{transform:rotate(275deg) scale(.9)}100%{transform:rotate(360deg) scale(1)}}
.rolling{animation:tumble .55s ease}
@media(max-width:680px){body{padding:14px}.game{grid-template-columns:1fr}.dice-area{min-height:220px}.die{width:72px;height:72px;padding:10px;gap:3px}.stats{gap:7px}.stat{padding:11px}.stat strong{font-size:21px}}
</style>
</head>
<body>
<div class="app">
<header><div class="pill">🎲 MINI GAME TOÁN 9</div><h1>Xúc Xắc Đúng – Sai</h1><p>Nhìn phép biến đổi bất đẳng thức, chọn đáp án thật nhanh!</p></header>
<div class="stats">
  <div class="stat"><span>⭐ Điểm hiện tại</span><strong id="score">0</strong></div>
  <div class="stat"><span>🔥 Chuỗi đúng</span><strong id="streak">0</strong></div>
  <div class="stat"><span>🏆 Kỷ lục</span><strong id="best">0</strong></div>
</div>
<div class="game">
  <section class="card dice-area">
    <div class="dice-label">TUNG XÚC XẮC ĐỂ TẠO CÂU HỎI</div>
    <div class="dice-row"><div class="die" id="die1"></div><div class="die" id="die2"></div></div>
    <button class="primary" id="rollBtn">🎲 Tung xúc xắc</button>
    <p class="dice-label" id="diceText">Bấm tung để bắt đầu lượt mới</p>
  </section>
  <section class="card question-card">
    <div class="question-top"><span id="round">LƯỢT 1</span><span id="category">Chờ bắt đầu</span></div>
    <div class="question" id="question">Sẵn sàng thử thách kiến thức bất đẳng thức chưa?</div>
    <div class="rule" id="rule">📘 Nhớ: cộng cùng số hoặc nhân số dương thì giữ chiều; nhân số âm thì đổi chiều.</div>
    <div class="answers"><button class="answer" id="trueBtn" disabled>✅ Đúng</button><button class="answer" id="falseBtn" disabled>❌ Sai</button></div>
    <div class="feedback" id="feedback">Tung xúc xắc để nhận câu hỏi đầu tiên nhé!</div>
  </section>
</div>
<div class="bottom"><button class="secondary" id="resetBtn">↻ Chơi lại</button><span class="dice-label">Mỗi câu đúng +10 điểm · Sai không bị trừ điểm</span></div>
<div class="note">Game chạy trực tiếp trên trình duyệt, không cần cài thêm phần mềm. Kỷ lục được lưu trên máy này.</div>
</div>
<script>
const $=id=>document.getElementById(id);
const faces={1:[4],2:[0,8],3:[0,4,8],4:[0,2,6,8],5:[0,2,4,6,8],6:[0,2,3,5,6,8]};
function drawDie(el,n){el.innerHTML='';for(let i=0;i<9;i++){let d=document.createElement('div');d.className='dot'+(faces[n].includes(i)?' on':'');el.appendChild(d)}}
function rand(n){return Math.floor(Math.random()*n)}
let score=0,streak=0,round=1,answered=true,current=null,best=Number(localStorage.getItem('diceIneqBest')||0);
$('best').textContent=best;drawDie($('die1'),1);drawDie($('die2'),1);
const templates=[
 {cat:'Cộng hai vế',rule:'Cộng cùng một số vào hai vế thì chiều bất đẳng thức giữ nguyên.',make:(a,b)=>{let k=rand(9)-4;let ok=rand(2)===0;return {q:`Từ ${a} < ${b}, suy ra ${a} + (${k}) ${ok?'<':'>='} ${b} + (${k}).`,ok:ok,why:ok?'Cộng cùng một số vào hai vế nên chiều “<” được giữ nguyên.':'Chiều đúng phải là “<”, vì cộng cùng một số không làm đổi chiều.'}}},
 {cat:'Nhân số dương',rule:'Nhân hai vế với số dương thì giữ nguyên chiều.',make:(a,b)=>{let k=rand(5)+1,ok=rand(2)===0;return {q:`Từ ${a} < ${b}, suy ra ${a} × ${k} ${ok?'<': '>'} ${b} × ${k}.`,ok:ok,why:ok?'Nhân hai vế với số dương '+k+' nên giữ nguyên chiều “<”.':'Vì '+k+' là số dương, chiều đúng vẫn phải là “<”, không phải “>”.'}}},
 {cat:'Nhân số âm',rule:'Nhân hai vế với số âm thì phải đổi chiều bất đẳng thức.',make:(a,b)=>{let k=-(rand(5)+1),ok=rand(2)===0;return {q:`Từ ${a} < ${b}, suy ra ${a} × (${k}) ${ok?'>':'<'} ${b} × (${k}).`,ok:ok,why:ok?'Nhân với số âm '+k+' nên phải đổi “<” thành “>”.':'Nhân với số âm '+k+' mà không đổi chiều là sai; chiều đúng phải là “>”.'}}}
];
function update(){ $('score').textContent=score;$('streak').textContent=streak;$('best').textContent=best;$('round').textContent='LƯỢT '+round; }
function nextQuestion(d1,d2){
 const t=templates[(d1+d2-2)%templates.length];let a=rand(13)-6,b=a+rand(8)+1;
 current=t.make(a,b);$('category').textContent=t.cat;$('question').textContent=current.q;$('rule').textContent='📘 '+t.rule;
 $('feedback').className='feedback';$('feedback').textContent='Chọn Đúng hoặc Sai để trả lời.';$('trueBtn').disabled=false;$('falseBtn').disabled=false;$('trueBtn').className='answer';$('falseBtn').className='answer';answered=false;
}
function roll(){if(!answered){$('feedback').className='feedback bad';$('feedback').textContent='Trả lời câu hiện tại trước đã nhé!';return}
 $('rollBtn').disabled=true;const d1=rand(6)+1,d2=rand(6)+1;
 $('die1').classList.remove('rolling');$('die2').classList.remove('rolling');void $('die1').offsetWidth;
 $('die1').classList.add('rolling');$('die2').classList.add('rolling');$('diceText').textContent='Đang tung...';
 setTimeout(()=>{drawDie($('die1'),d1);drawDie($('die2'),d2);$('diceText').textContent=`Kết quả: ${d1} + ${d2} = ${d1+d2}`;nextQuestion(d1,d2);$('rollBtn').disabled=false;},550);
}
function answer(choice){if(answered||!current)return;answered=true;$('trueBtn').disabled=true;$('falseBtn').disabled=true;
 const correct=choice===current.ok;
 if(correct){score+=10;streak++;$('feedback').className='feedback good';$('feedback').textContent='🎉 Chính xác! '+current.why;$('trueBtn').classList.toggle('correct',choice);$('falseBtn').classList.toggle('correct',!choice)}
 else{streak=0;$('feedback').className='feedback bad';$('feedback').textContent='Chưa đúng rồi! '+current.why;$(choice?'trueBtn':'falseBtn').classList.add('wrong')}
 if(score>best){best=score;localStorage.setItem('diceIneqBest',best)}
 round++;update();
}
$('rollBtn').addEventListener('click',roll);$('trueBtn').addEventListener('click',()=>answer(true));$('falseBtn').addEventListener('click',()=>answer(false));
$('resetBtn').addEventListener('click',()=>{score=0;streak=0;round=1;answered=true;current=null;update();$('category').textContent='Chờ bắt đầu';$('question').textContent='Sẵn sàng thử thách kiến thức bất đẳng thức chưa?';$('feedback').className='feedback';$('feedback').textContent='Tung xúc xắc để nhận câu hỏi đầu tiên nhé!';$('rule').textContent='📘 Nhớ: cộng cùng số hoặc nhân số dương thì giữ chiều; nhân số âm thì đổi chiều.';$('trueBtn').disabled=true;$('falseBtn').disabled=true;$('rollBtn').disabled=false;drawDie($('die1'),1);drawDie($('die2'),1);$('diceText').textContent='Bấm tung để bắt đầu lượt mới';});
</script>
</body>
</html>
