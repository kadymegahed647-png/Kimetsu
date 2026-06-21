<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
<title>قاتل الشياطين: غضب القلعة اللانهائية المطور</title>
<style>
@import url('https://fonts.googleapis.com/css2?family=Cairo:wght@400;700;900&family=Amiri:wght@400;700&display=swap');
*{margin:0;padding:0;box-sizing:border-box}
body{background:#050005;overflow:hidden;width:100vw;height:100vh;font-family:'Cairo',sans-serif;user-select:none;-webkit-user-select:none}
canvas{position:fixed;top:0;left:0;z-index:1;display:block}

.screen{position:fixed;inset:0;z-index:100;display:none;flex-direction:column;align-items:center;justify-content:center;padding:20px;text-align:center}
.screen.active{display:flex}

/* شاشات البداية والتسجيل */
#s-login{background:radial-gradient(circle, #200010 0%, #000 100%)}
.input-gate{background:rgba(255,255,255,0.04);border:2px solid rgba(231,76,60,0.3);padding:30px;border-radius:15px;box-shadow:0 0 30px rgba(0,0,0,0.7);max-width:400px;width:100%}
.inp{width:100%;padding:12px;background:rgba(0,0,0,0.6);border:1px solid rgba(255,255,255,0.2);border-radius:6px;color:#fff;font-size:1.1rem;text-align:center;margin-bottom:15px;outline:none}
.inp:focus{border-color:#e74c3c;box-shadow:0 0 10px #e74c3c}

/* MENU & STORY */
#s-menu{background:radial-gradient(ellipse at 50% 30%,#1a0202 0%,#050005 100%)}
.menu-title{font-size:clamp(2.5rem,7vw,4.5rem);color:#fff;text-shadow:0 0 30px #e74c3c;font-weight:900;line-height:1.2}

#s-story{background:#000}
.story-text{font-family:'Amiri',serif;font-size:clamp(1.4rem,3vw,2.2rem);line-height:1.8;color:#f39c12;min-height:120px}

/* CHAR SELECT */
#s-charsel{background:#05000a}
#char-scroll{display:flex;gap:15px;overflow-x:auto;max-width:90vw;padding:15px;justify-content:center;flex-wrap:wrap}
.ccard{border:2px solid rgba(255,255,255,0.1);border-radius:12px;width:140px;background:rgba(255,255,255,0.02);cursor:pointer;transition:all 0.3s;padding:12px}
.ccard .c-avatar{font-size:2.5rem;margin-bottom:5px}
.ccard .cn{font-size:1rem;font-weight:bold;color:#fff}
.ccard.sel{border-color:#e74c3c;background:rgba(231,76,60,0.15);box-shadow:0 0 15px #e74c3c}

/* HUD المطور */
#hud{position:fixed;inset:0;z-index:20;pointer-events:none;display:none}
#hud-top{position:absolute;top:0;left:0;right:0;padding:15px;display:flex;justify-content:space-between;background:linear-gradient(to bottom,rgba(0,0,0,0.85),transparent)}
.bwrap{display:flex;flex-direction:column;gap:5px}
.brow{display:flex;align-items:center;gap:8px}
.blbl{font-size:0.75rem;color:#ccc;width:45px;text-align:right}
.bbg{height:10px;border-radius:5px;background:rgba(255,255,255,0.1);overflow:hidden}
#bhp{width:160px}#bxp{width:120px}
#fhp{height:100%;background:#e74c3c;transition:width 0.2s}
#fxp{height:100%;background:#9b59b6}
#hud-right{text-align:left;color:#fff;font-size:0.85rem}
#sc-money{font-size:1.4rem;color:#f1c40f;font-weight:bold}

/* شريط صحة الـ BOSS العلوي */
#boss-hud{position:fixed;top:75px;left:50%;transform:translateX(-50%);width:70vw;max-width:500px;display:none;z-index:25;text-align:center}
#boss-name{color:#ff3f34;font-size:0.9rem;font-weight:bold;text-shadow:0 0 5px #000;margin-bottom:3px}
.boss-bbg{width:100%;height:14px;background:rgba(0,0,0,0.6);border:1px solid #ff3f34;border-radius:7px;overflow:hidden}
#boss-fhp{height:100%;background:linear-gradient(90deg, #960000, #ff0055);width:100%;transition:width 0.1s}

/* عناصر الشبكة والغرف والمشاركة والتحويل المالي للمطور */
#room-controls{position:fixed;top:85px;right:15px;z-index:80;display:none;flex-direction:column;gap:8px;pointer-events:all}
.r-btn{background:rgba(46,204,113,0.2);border:1px solid #2ecc71;color:#2ecc71;padding:6px 12px;border-radius:20px;font-size:0.75rem;cursor:pointer;transition:all 0.2s}
.r-btn:hover{background:#2ecc71;color:#fff}
.shop-btn{background:rgba(241,196,15,0.2);border:1px solid #f1c40f;color:#f1c40f}
.shop-btn:hover{background:#f1c40f;color:#000}

/* نافذة الشات المدمجة */
#chat-box{position:fixed;bottom:20px;right:15px;width:240px;height:160px;background:rgba(0,0,0,0.85);border:1px solid rgba(255,255,255,0.15);border-radius:8px;z-index:80;display:none;flex-direction:column;pointer-events:all;padding:5px}
#chat-msg{flex:1;overflow-y:auto;color:#fff;font-size:0.75rem;text-align:right;padding:5px;display:flex;flex-direction:column;gap:3px}
#chat-input{width:100%;background:#111;border:1px solid #333;color:#fff;padding:5px;font-size:0.75rem;border-radius:4px;outline:none}

/* CONTROLS & SKILLS */
#skillbar{position:fixed;bottom:15px;left:50%;transform:translateX(-50%);display:none;gap:10px;z-index:20;pointer-events:all}
.sk{width:52px;height:52px;border:2px solid rgba(255,255,255,0.15);border-radius:10px;background:#000;display:flex;align-items:center;justify-content:center;cursor:pointer;position:relative}
.sk-ico{font-size:1.4rem}
.locked-skill::after{content:'🔒';position:absolute;top:-5px;right:-5px;font-size:0.8rem;background:#000;border-radius:50%;padding:2px}
#breathbar{position:fixed;bottom:80px;left:50%;transform:translateX(-50%);display:none;z-index:20;pointer-events:all;background:rgba(0,0,0,0.7);padding:4px 15px;border-radius:15px;color:#fff;font-size:0.85rem;cursor:pointer}

/* JOYSTICK */
#joystick-zone{position:fixed;bottom:30px;left:30px;width:110px;height:110px;z-index:99;display:none}
.joystick-base{width:100%;height:100%;background:rgba(255,255,255,0.05);border:2px solid rgba(255,255,255,0.15);border-radius:50%;position:relative}
.joystick-stick{width:45px;height:45px;background:#e74c3c;border-radius:50%;position:absolute;top:32.5px;left:32.5px}

.btn{font-size:0.9rem;font-weight:bold;padding:10px 30px;border-radius:6px;cursor:pointer;transition:all 0.3s;border:2px solid;margin:8px}
.btn-r{background:#c0392b;border-color:#e74c3c;color:#fff}
.btn-r:hover{background:#e74c3c;box-shadow:0 0 15px #e74c3c}

#s-gameover{background:#1a0000}
#s-upgrade{background:rgba(0,0,0,0.96)}
#upgrid{display:flex;flex-direction:column;gap:12px;width:90vw;max-width:350px;margin-top:15px}
.ucard{border:1px solid #f1c40f;border-radius:10px;padding:12px;color:#fff;cursor:pointer;background:rgba(241,196,15,0.03)}
.ucard:hover{background:rgba(241,196,15,0.15)}
</style>
</head>
<body>
<canvas id="c"></canvas>

<!-- 1. شاشة التسجيل -->
<div class="screen active" id="s-login">
  <div class="input-gate">
    <h2 style="color:#fff;margin-bottom:15px;font-size:1.4rem">سجل اسمك أيها الفيلق!</h2>
    <input type="text" id="p-name-input" class="inp" placeholder="اسم فيلق قتلة الشياطين..." maxlength="15" value="تانجيرو الأسطوري">
    <button class="btn btn-r" id="btn-submit-name" style="width:100%;margin:0">تأكيد وركوب القطار ⚔️</button>
  </div>
</div>

<!-- MENU -->
<div class="screen" id="s-menu">
  <div class="menu-title">قاتل الشياطين<br><span style="font-size:0.4em;color:#f1c40f">القلعة اللانهائية: الأقمار الستة</span></div>
  <button class="btn btn-r" id="btn-start">ابدأ الملحمة القديمة ⚔></button>
</div>

<!-- STORY -->
<div class="screen" id="s-story">
  <div class="story-text" id="story-text"></div>
  <button class="btn btn-r" id="btn-story-next" style="margin-top:30px">التالي ➔</button>
</div>

<!-- CHAR SELECT -->
<div class="screen" id="s-charsel">
  <div style="color:#fff;margin-bottom:10px">اختر هيئتك القتالية العظمى</div>
  <div id="char-scroll"></div>
  <button class="btn btn-r" id="btn-play">اعبر البوابة نحو التحدي 🩸</button>
</div>

<!-- UPGRADE -->
<div class="screen" id="s-upgrade">
  <div style="font-size:1.5rem;color:#f1c40f;font-weight:bold" id="up-title">انتهت المرحلة بنجاح!</div>
  <div style="color:#aaa;margin-bottom:10px">طور نصل النيشيرين الخاص بك باستخدام غنائمك</div>
  <div id="upgrid"></div>
</div>

<!-- GAME OVER -->
<div class="screen" id="s-gameover">
  <div style="font-size:3rem;color:#e74c3c;font-weight:bold">موت بطولي</div>
  <div id="go-stats" style="color:rgba(255,255,255,0.6);margin:20px 0"></div>
  <button class="btn btn-r" id="btn-retry">إعادة إشعال اللهب 🔥</button>
</div>

<!-- HUD -->
<div id="hud">
  <div id="hud-top">
    <div class="bwrap">
      <div class="brow"><span class="blbl">الصحة</span><div class="bbg" id="bhp"><div id="fhp" style="width:100%"></div></div></div>
      <div class="brow"><span class="blbl">الخبرة</span><div class="bbg" id="bxp"><div id="fxp" style="width:0%"></div></div></div>
    </div>
    <div id="hud-right">
      <div id="p-display-name" style="font-weight:bold;color:#e74c3c">المقاتل</div>
      <div id="sc-money">💴 0</div>
      <div id="game-stage-view">المرحلة: التدريب البدائي</div>
    </div>
  </div>
</div>

<!-- شريط البوس -->
<div id="boss-hud">
  <div id="boss-name">اسم القمر الشيطان</div>
  <div class="boss-bbg"><div id="boss-fhp"></div></div>
</div>

<!-- عناصر الغرفة والشبكة المدمجة والشات -->
<div id="room-controls">
  <button class="r-btn" id="btn-invite">🔗 انسخ رابط الغرفة وادعُ صديقاً</button>
  <button class="r-btn shop-btn" id="btn-shop">🛒 متجر تنفس الشمس (10 ريال)</button>
</div>

<div id="chat-box">
  <div id="chat-msg"><div>النظام: تم دخول القلعة اللانهائية بنجاح.</div></div>
  <input type="text" id="chat-input" placeholder="اكتب رسالة للفيلق واضغط Enter...">
</div>

<!-- CONTROLS UI -->
<div id="skillbar">
  <div class="sk" id="sk0"><span class="sk-ico">⚔️</span></div>
  <div class="sk" id="sk1"><span class="sk-ico">💫</span></div>
  <div class="sk" id="sk2"><span class="sk-ico">⚡</span></div>
  <div class="sk locked-skill" id="sk3"><span class="sk-ico">☀️</span></div>
</div>
<div id="breathbar"><div id="breathname">تنفس الماء</div></div>

<div id="joystick-zone">
  <div class="joystick-base"><div class="joystick-stick" id="j-stick"></div></div>
</div>

<script>
// --- AUDIO GENERATOR ---
const audioCtx = new (window.AudioContext || window.webkitAudioContext)();
function playSound(type) {
    if (audioCtx.state === 'suspended') audioCtx.resume();
    const osc = audioCtx.createOscillator(); const gain = audioCtx.createGain();
    osc.connect(gain); gain.connect(audioCtx.destination);
    if (type==='water'){ osc.frequency.setValueAtTime(200, audioCtx.currentTime); gain.gain.setValueAtTime(0.3, audioCtx.currentTime); }
    else if (type==='fire'){ osc.type='sawtooth'; osc.frequency.setValueAtTime(100, audioCtx.currentTime); gain.gain.setValueAtTime(0.4, audioCtx.currentTime); }
    else if (type==='sun'){ osc.type='sawtooth'; osc.frequency.setValueAtTime(450, audioCtx.currentTime); gain.gain.setValueAtTime(0.5, audioCtx.currentTime); }
    else if (type==='hit'){ osc.type='triangle'; osc.frequency.setValueAtTime(120, audioCtx.currentTime); gain.gain.setValueAtTime(0.2, audioCtx.currentTime); }
    else { osc.frequency.setValueAtTime(300, audioCtx.currentTime); gain.gain.setValueAtTime(0.1, audioCtx.currentTime); }
    gain.gain.exponentialRampToValueAtTime(0.01, audioCtx.currentTime + 0.4);
    osc.start(); osc.stop(audioCtx.currentTime + 0.4);
}

// --- CONFIG & GAME DATA ---
const canvas = document.getElementById('c'); const ctx = canvas.getContext('2d');
function resize() { canvas.width = window.innerWidth; canvas.height = window.innerHeight; }
window.addEventListener('resize', resize); resize();

let playerName = "المحارب";
let gameState = 'login';
let money = 0; let currentStageIdx = 0; let level = 1;
let player, enemies = [], particles = [], dNums = [], currentBoss = null;
let keys = {}; let joyActive = false; let joyStartPos = {x:0,y:0}; let joyVector = {x:0,y:0};

// حالة شراء تنفس الشمس
let isSunBreathPurchased = false;

// نظام المراحل والأقمار
const stages = [
    { name: "المرحلة 0: تدريب تانجيرو على قطع الصخرة الأسطورية", isTraining: true, targetRockHp: 300, bossName: "الصخرة الضخمة الصلبة", reward: 200 },
    { name: "المرحلة 1: مواجهة القمر الأدنى السادس (روي)", isTraining: false, bossName: "روي (Lower Moon 6)", bossHp: 500, minEnemies: 8, reward: 500 },
    { name: "المرحلة 2: مواجهة القمر الأدنى الأول (إينمو)", isTraining: false, bossName: "إينمو (Lower Moon 1)", bossHp: 800, minEnemies: 10, reward: 800 },
    { name: "المرحلة 3: هجوم القمر الأعلى الثالث (أكازا)", isTraining: false, bossName: "أكازا (Upper Moon 3)", bossHp: 1500, minEnemies: 12, reward: 1500 },
    { name: "المرحلة 4: قتال سياف القمر الأعلى الأول (كوكوشيبو)", isTraining: false, bossName: "كوكوشيبو (Upper Moon 1)", bossHp: 2500, minEnemies: 15, reward: 3000 },
    { name: "المرحلة 5: المعركة النهائية الكبرى ضد ملك الشياطين (موزان)", isTraining: false, bossName: "كيبوتسوجي موزان (The Demon King)", bossHp: 5000, minEnemies: 20, reward: 10000 }
];

const breaths = [
    { name: 'تنفس الماء 🌊', color: '#3498db', type: 'water' },
    { name: 'تنفس النار 🔥', color: '#e74c3c', type: 'fire' },
    { name: 'تنفس الشمس الأسطوري ☀️', color: '#f1c40f', type: 'sun' }
];
let currentBreathIdx = 0;

const characters = [
    { name: "تانجيرو كامادو", desc: "سليل الوعاء الحامل لإرادة الهانافودا", color: "#16a085", avatar:"🎴" },
    { name: "رينغوكو كيوجورو", desc: "هاشيرا اللهب المشتعل", color: "#e74c3c", avatar:"🔥" }
];
let selectedChar = characters[0];

const storyChapters = [
    "مرحباً بك في أروقة 'القلعة اللانهائية' المتغيرة جغرافياً.. حيث الزمن والمكان لا قيمة لهما أمام تعطش الشياطين.",
    "قبل أن ترفع سيفك في وجه الأقمار الاثني عشر، عليك اجتياز تدريب تانجيرو القاسي لقطع الصخرة الصماء واستخلاص القوة النرجسية الكامنة داخلك!"
];
let storyIdx = 0;

// --- MULTIPLAYER LOBBY SYSTEM & CHAT MOCKUP ---
const urlParams = new URLSearchParams(window.location.search);
const roomID = urlParams.get('room') || 'Room_' + Math.floor(1000 + Math.random() * 9000);

document.getElementById('btn-invite').onclick = () => {
    const inviteLink = window.location.origin + window.location.pathname + '?room=' + roomID;
    navigator.clipboard.writeText(inviteLink);
    alert(`📥 تم نسخ رابط الغرفة بنجاح!\nأرسله لصديقك ليدخل معك نفس الروم في القلعة اللانهائية:\n${inviteLink}`);
    addChatMessage("النظام", "لقد قمت بنسخ رابط دعوة الغرفة الموحدة.");
};

// زر شراء تنفس الشمس بـ 10 ريال (توجيه ودعم المطور)
document.getElementById('btn-shop').onclick = () => {
    let confirmPurchase = confirm("🛒 متجر تنفس الشمس الأسطوري ☀️\n\nقيمة هذه الميزة الخارقة هي 10 ريالات تذهب مباشرة لدعم مطور اللعبة الأسطوري واقتصاده الخاص.\n\nهل تريد تفعيل تنفس الشمس الآن في هذه الجلسة؟");
    if(confirmPurchase) {
        isSunBreathPurchased = true;
        document.getElementById('sk3').classList.remove('locked-skill');
        alert("☀️ تم تفعيل تنفس الشمس بنجاح! يمكنك الآن الضغط على زر الشمس (القدرة الرابعة) لتدمير الأقمار العليا بسرعة هائلة!");
        addChatMessage("النظام", "🔥 قام اللاعب بتفعيل تنفس الشمس الخارق بعد دعم المطور بـ 10 ريالات!");
    }
};

// شات اللعبة داخل الروم
const chatInput = document.getElementById('chat-input');
chatInput.addEventListener('keydown', (e) => {
    if (e.key === 'Enter' && chatInput.value.trim() !== "") {
        addChatMessage(playerName, chatInput.value);
        setTimeout(() => {
            const replies = ["أنا معك في ظهرك بالقلعة اللانهائية!", "تنفس النار جاهز معي لتدمير موزان!", "واو! هل قمت بتفعيل تنفس الشمس من المتجر؟ ضرباته مرعبة!"];
            addChatMessage("صديق الغرفة ⚔️", replies[Math.floor(Math.random() * replies.length)]);
        }, 1500);
        chatInput.value = "";
    }
});

function addChatMessage(sender, text) {
    const box = document.getElementById('chat-msg');
    const div = document.createElement('div');
    div.innerHTML = `<b style="color:#f1c40f">${sender}:</b> ${text}`;
    box.appendChild(div);
    box.scrollTop = box.scrollHeight;
}

// --- UI TRIGGERS ---
document.getElementById('btn-submit-name').onclick = () => {
    const val = document.getElementById('p-name-input').value.trim();
    if(val) playerName = val;
    document.getElementById('p-display-name').innerText = playerName;
    document.getElementById('s-login').classList.remove('active');
    document.getElementById('s-menu').classList.add('active');
};

document.getElementById('btn-start').onclick = () => {
    document.getElementById('s-menu').classList.remove('active');
    document.getElementById('s-story').classList.add('active');
    showStory();
};

function showStory() {
    document.getElementById('story-text').innerText = storyChapters[storyIdx];
}

document.getElementById('btn-story-next').onclick = () => {
    storyIdx++;
    if(storyIdx < storyChapters.length) { showStory(); } 
    else {
        document.getElementById('s-story').classList.remove('active');
        document.getElementById('s-charsel').classList.add('active');
        setupCharSelect();
    }
};

function setupCharSelect() {
    const sc = document.getElementById('char-scroll'); sc.innerHTML = '';
    characters.forEach(char => {
        let card = document.createElement('div');
        card.className = `ccard ${char.name===selectedChar.name?'sel':''}`;
        card.innerHTML = `<div class="c-avatar">${char.avatar}</div><div class="cn">${char.name}</div>`;
        card.onclick = () => {
            document.querySelectorAll('.ccard').forEach(c=>c.classList.remove('sel'));
            card.classList.add('sel'); selectedChar = char;
        };
        sc.appendChild(card);
    });
}

document.getElementById('btn-play').onclick = () => {
    document.getElementById('s-charsel').classList.remove('active');
    initGame();
};

// --- CLASSES ---
class Player {
    constructor() {
        this.x = canvas.width / 2; this.y = canvas.height / 2;
        this.radius = 22; this.speed = 5; this.angle = 0;
        this.hp = 100; this.maxHp = 100; this.xp = 0; this.maxXp = 100;
        this.isAttacking = false; this.attackProgress = 0;
    }
    update() {
        let dx=0; let dy=0;
        if(keys['KeyA']||keys['ArrowLeft']) dx=-1; if(keys['KeyD']||keys['ArrowRight']) dx=1;
        if(keys['KeyW']||keys['ArrowUp']) dy=-1; if(keys['KeyS']||keys['ArrowDown']) dy=1;
        if(joyActive){ dx=joyVector.x; dy=joyVector.y; }
        if(dx!==0||dy!==0){
            let len = Math.sqrt(dx*dx+dy*dy);
            this.x += (dx/len)*this.speed; this.y += (dy/len)*this.speed;
        }
        this.x = Math.max(this.radius, Math.min(canvas.width-this.radius, this.x));
        this.y = Math.max(this.radius, Math.min(canvas.height-this.radius, this.y));
    }
    draw() {
        ctx.save(); ctx.translate(this.x, this.y); ctx.rotate(this.angle);
        ctx.shadowBlur = 20; ctx.shadowColor = selectedChar.color;
        ctx.fillStyle = selectedChar.color; ctx.beginPath(); ctx.arc(0,0,this.radius,0,Math.PI*2); ctx.fill();
        ctx.fillStyle = '#fff'; ctx.fillRect(0, -3, this.radius+15, 6); ctx.restore();
    }
}

class RockTrainingTarget {
    constructor() {
        this.x = canvas.width / 2; this.y = canvas.height / 2 - 100;
        this.radius = 45;
        this.hp = stages[0].targetRockHp; this.maxHp = this.hp;
    }
    draw() {
        ctx.save(); ctx.shadowBlur = 15; ctx.shadowColor = '#7f8c8d';
        ctx.fillStyle = '#7f8c8d'; ctx.strokeStyle = '#bdc3c7'; ctx.lineWidth = 4;
        ctx.beginPath(); ctx.arc(this.x, this.y, this.radius, 0, Math.PI * 2); ctx.fill(); ctx.stroke();
        ctx.strokeStyle = '#333'; ctx.lineWidth = 3; ctx.beginPath(); ctx.moveTo(this.x, this.y - 30); ctx.lineTo(this.x, this.y + 30); ctx.stroke();
        ctx.restore();
    }
}

class Enemy {
    constructor(isBoss = false) {
        this.isBoss = isBoss;
        this.radius = isBoss ? 40 : 18;
        this.x = Math.random() * canvas.width; this.y = isBoss ? 150 : (Math.random()*150);
        this.hp = isBoss ? stages[currentStageIdx].bossHp : 20 + (currentStageIdx*10);
        this.maxHp = this.hp;
        this.speed = isBoss ? 1.2 : 1.5 + (currentStageIdx*0.2);
    }
    update() {
        let dx = player.x - this.x; let dy = player.y - this.y;
        let dist = Math.sqrt(dx*dx+dy*dy);
        if(dist > 0){ this.x += (dx/dist)*this.speed; this.y += (dy/dist)*this.speed; }
        if(dist < player.radius + this.radius) {
            player.hp -= this.isBoss ? 0.6 : 0.25;
            if(player.hp <= 0) endGame();
        }
    }
    draw() {
        ctx.save(); ctx.fillStyle = this.isBoss ? '#8b0000' : '#111';
        ctx.strokeStyle = this.isBoss ? '#f1c40f' : '#ff3f34'; ctx.lineWidth = this.isBoss ? 4 : 2;
        ctx.beginPath(); ctx.arc(this.x, this.y, this.radius, 0, Math.PI*2); ctx.fill(); ctx.stroke();
        ctx.restore();
    }
}

class Particle {
    constructor(x,y,color) { this.x=x;this.y=y;this.color=color;this.alpha=1;this.vx=(Math.random()-0.5)*8;this.vy=(Math.random()-0.5)*8; }
    update() { this.x+=this.vx;this.y+=this.vy;this.alpha-=0.03; }
    draw() { ctx.save();ctx.globalAlpha=this.alpha;ctx.fillStyle=this.color;ctx.beginPath();ctx.arc(this.x,this.y,4,0,Math.PI*2);ctx.fill();ctx.restore(); }
}

// --- LOGIC MECHANICS ---
function initGame() {
    player = new Player(); enemies = []; particles = []; dNums = []; currentBoss = null;
    document.querySelectorAll('.screen').forEach(s=>s.classList.remove('active'));
    document.getElementById('hud').style.display = 'block';
    document.getElementById('skillbar').style.display = 'flex';
    document.getElementById('breathbar').style.display = 'block';
    document.getElementById('room-controls').style.display = 'flex';
    document.getElementById('chat-box').style.display = 'flex';
    if('ontouchstart' in window) document.getElementById('joystick-zone').style.display='block';
    
    startStage(currentStageIdx);
    gameState = 'play';
}

function startStage(idx) {
    enemies = []; currentBoss = null;
    const currentStage = stages[idx];
    document.getElementById('game-stage-view').innerText = currentStage.name;
    addChatMessage("النظام", `لقد دخلت: ${currentStage.name}`);

    if (currentStage.isTraining) {
        currentBoss = new RockTrainingTarget();
        document.getElementById('boss-hud').style.display = 'block';
        document.getElementById('boss-name').innerText = currentStage.bossName;
    } else {
        document.getElementById('boss-hud').style.display = 'block';
        document.getElementById('boss-name').innerText = currentStage.bossName + " (القمر الحاكم)";
        currentBoss = new Enemy(true);
        for(let i=0; i < currentStage.minEnemies; i++) { enemies.push(new Enemy(false)); }
    }
}

function triggerAttack(skIdx) {
    // إذا اختار القدرة الرابعة (تنفس الشمس) ولم يشتريها بعد
    if(skIdx === 3 && !isSunBreathPurchased) {
        alert("🔒 هذه القدرة مقفلة! تنفس الشمس الأسطوري يتطلب الدعم بـ 10 ريالات للمطور لتفعيله من زر المتجر بالأعلى 🛒");
        return;
    }

    let activeBreath = breaths[skIdx === 3 ? 2 : currentBreathIdx];
    
    player.isAttacking = true;
    playSound(activeBreath.type);
    for(let i=0;i<15;i++) particles.push(new Particle(player.x,player.y,activeBreath.color));

    // حساب الضرر (تنفس الشمس يعطي ضرر مضاعف مرتين 💥)
    let baseDmg = (18 + level * 4);
    if(skIdx === 3) baseDmg *= 3; // قوة الشمس المدمرة

    if(currentBoss) {
        let dx = currentBoss.x - player.x; let dy = currentBoss.y - player.y;
        let dist = Math.sqrt(dx*dx+dy*dy);
        if(dist <= player.radius + currentBoss.radius + 60) {
            currentBoss.hp -= baseDmg; playSound('hit');
            money += Math.floor(baseDmg * 0.5); 
            
            dNums.push({x:currentBoss.x, y:currentBoss.y-20, txt:`-${Math.floor(baseDmg)} 💴+`, color:activeBreath.color});
            
            if(currentBoss.hp <= 0) {
                money += stages[currentStageIdx].reward;
                addChatMessage("النظام", `🎉 تم سحق ${stages[currentStageIdx].bossName}! المكافأة: 💴 ${stages[currentStageIdx].reward}`);
                stageCleared();
                return;
            }
        }
    }

    enemies.forEach((en, i) => {
        let dx = en.x - player.x; let dy = en.y - player.y;
        let dist = Math.sqrt(dx*dx+dy*dy);
        if(dist <= player.radius + en.radius + 50) {
            en.hp -= baseDmg;
            money += 10; 
            if(en.hp <= 0) { enemies.splice(i,1); player.xp += 20; if(player.xp >= player.maxXp) { player.xp=0; level++; } }
        }
    });
}

function stageCleared() {
    gameState = 'upgrade';
    document.getElementById('s-upgrade').classList.add('active');
    document.getElementById('up-title').innerText = `اكتملت ${stages[currentStageIdx].name}!`;
    
    const grid = document.getElementById('upgrid'); grid.innerHTML = '';
    let d = document.createElement('div');
    d.className = 'ucard'; d.innerHTML = `🌟 اضغط هنا للانتقال للمرحلة التالية وتطوير نصلك`;
    d.onclick = () => {
        document.getElementById('s-upgrade').classList.remove('active');
        currentStageIdx++;
        if(currentStageIdx >= stages.length) {
            alert("🏆 أسطوري! لقد قضيت على موزان وجميع الأقمار وحررت نيزوكو بالكامل!");
            currentStageIdx = 0; money = 0;
            document.getElementById('s-menu').classList.add('active');
        } else {
            initGame();
        }
    };
    grid.appendChild(d);
}

function switchBreath() {
    currentBreathIdx = (currentBreathIdx+1) % 2; // التبديل التلقائي العادي بين الماء والنار فقط
    document.getElementById('breathname').innerText = breaths[currentBreathIdx].name;
}

function endGame() {
    gameState = 'gameover';
    document.getElementById('hud').style.display='none';
    document.getElementById('boss-hud').style.display='none';
    document.getElementById('s-gameover').classList.add('active');
    document.getElementById('go-stats').innerText = `الأموال المجمعة داخل اللعبة: 💴 ${money} | الليفل: ${level}`;
}

// --- CONTROL LISTENERS ---
window.addEventListener('keydown', e => {
    keys[e.code] = true;
    if(e.code==='KeyE') switchBreath();
    if(['Digit1','Digit2','Digit3','Digit4'].includes(e.code)) triggerAttack(parseInt(e.code.slice(-1))-1);
});
window.addEventListener('keyup', e => keys[e.code] = false);
window.addEventListener('mousemove', e => { if(gameState==='play') player.angle = Math.atan2(e.clientY-player.y, e.clientX-player.x); });
window.addEventListener('mousedown', e => { if(gameState==='play'&& e.clientX > 150) triggerAttack(0); });

// Joystick للموبايل
const jStick = document.getElementById('j-stick');
window.addEventListener('touchstart', e => {
    for(let t of e.changedTouches) { if(t.clientX < window.innerWidth/2) { joyActive=true; joyStartPos={x:t.clientX,y:t.clientY}; break; } }
});
window.addEventListener('touchmove', e => {
    if(!joyActive) return;
    for(let t of e.touches) {
        if(t.clientX < window.innerWidth/2) {
            let dx=t.clientX-joyStartPos.x; let dy=t.clientY-joyStartPos.y;
            let dist=Math.sqrt(dx*dx+dy*dy); let maxD=35;
            if(dist>maxD){ dx=(dx/dist)*maxD; dy=(dy/dist)*maxD; }
            jStick.style.transform = `translate(${dx}px,${dy}px)`;
            joyVector={x:dx/maxD, y:dy/maxD};
        }
    }
});
window.addEventListener('touchend', e => { joyActive=false; jStick.style.transform='translate(0px,0px)'; joyVector={x:0,y:0}; });

document.getElementById('btn-retry').onclick = () => { document.getElementById('s-gameover').classList.remove('active'); initGame(); };
document.getElementById('breathbar').onclick = switchBreath;
document.querySelectorAll('.sk').forEach((btn, i) => btn.onclick = () => triggerAttack(i));

// --- LOOP ---
function loop() {
    ctx.clearRect(0,0,canvas.width,canvas.height);
    if(gameState==='play') {
        ctx.strokeStyle='rgba(231,76,60,0.08)'; ctx.lineWidth=2;
        for(let i=0; i<canvas.width; i+=40){ ctx.beginPath();ctx.moveTo(i,0);ctx.lineTo(i,canvas.height);ctx.stroke(); }
        
        player.update(); player.draw();

        if(currentBoss) {
            if(typeof currentBoss.update === 'function') currentBoss.update();
            currentBoss.draw();
            document.getElementById('boss-fhp').style.width = `${(currentBoss.hp / currentBoss.maxHp)*100}%`;
        }

        enemies.forEach(en => { en.update(); en.draw(); });

        for (let i = particles.length - 1; i >= 0; i--) {
            particles[i].update(); if(particles[i].alpha<=0) particles.splice(i,1); else particles[i].draw();
        }

        for (let i = dNums.length - 1; i >= 0; i--) {
            dNums[i].y -= 1; dNums[i].alpha -= 0.02;
            if(dNums[i].alpha<=0) dNums.splice(i,1);
            else { ctx.fillStyle=dNums[i].color; ctx.font='bold 14px Cairo'; ctx.fillText(dNums[i].txt, dNums[i].x, dNums[i].y); }
        }

        document.getElementById('fhp').style.width = `${player.hp}%`;
        document.getElementById('fxp').style.width = `${player.xp}%`;
        document.getElementById('sc-money').innerText = `💴 ${money}`;
    }
    requestAnimationFrame(loop);
}
requestAnimationFrame(loop);
</script>
</body>
</html>
