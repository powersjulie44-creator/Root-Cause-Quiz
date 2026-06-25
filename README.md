[index.html](https://github.com/user-attachments/files/29313969/index.html)
[Uploading index.h<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>The Root-Cause Quiz</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Fraunces:ital,opsz,wght@0,9..144,400;0,9..144,500;1,9..144,400&family=Inter:wght@400;500;600&display=swap" rel="stylesheet">
<style>
  :root {
    --cream:      #F6EFE3;
    --cream-card: #FCF8F0;
    --cream-line: #E7DCC8;
    --navy:       #1F3A52;
    --navy-deep:  #16293A;
    --terra:      #C8654A;
    --terra-soft: #E5B6A6;
    --ink:        #2C2A26;
    --muted:      #7C7363;
  }
  * { box-sizing: border-box; }
  html, body { margin: 0; padding: 0; }
  body {
    background: var(--cream);
    color: var(--ink);
    font-family: 'Inter', system-ui, sans-serif;
    min-height: 100vh;
    display: flex;
    justify-content: center;
    padding: 28px 18px 56px;
    -webkit-font-smoothing: antialiased;
  }
  .wrap { width: 100%; max-width: 540px; }

  /* ── shared type ── */
  .eyebrow {
    font-family: 'Inter', sans-serif;
    letter-spacing: 0.24em; text-transform: uppercase;
    font-size: 11px; font-weight: 600; color: var(--terra);
  }
  h1.display, h2.display, h3.display {
    font-family: 'Fraunces', Georgia, serif;
    font-weight: 400; line-height: 1.1; margin: 0;
  }
  em { font-style: italic; }

  /* ── buttons ── */
  .btn-primary {
    background: var(--navy); color: var(--cream); border: none;
    padding: 16px 36px; border-radius: 100px;
    font-family: 'Inter', sans-serif; font-size: 15px; font-weight: 600;
    letter-spacing: 0.01em; cursor: pointer;
    transition: transform .18s ease, background .18s ease, box-shadow .18s ease;
    box-shadow: 0 8px 22px -12px rgba(31,58,82,.55);
  }
  .btn-primary:hover { background: var(--navy-deep); transform: translateY(-2px); box-shadow: 0 14px 28px -14px rgba(31,58,82,.6); }
  .btn-primary:active { transform: translateY(0); }

  .option {
    text-align: left; padding: 17px 18px; border-radius: 14px;
    border: 1.5px solid var(--cream-line); background: var(--cream-card);
    font-family: 'Inter', sans-serif; font-size: 15.5px; line-height: 1.4;
    color: var(--ink); cursor: pointer; width: 100%;
    display: flex; align-items: center; gap: 13px;
    transition: border-color .16s ease, background .16s ease, transform .12s ease;
  }
  .option:hover { border-color: var(--terra); background: #fff; transform: translateX(3px); }
  .option:focus-visible { outline: 3px solid var(--navy); outline-offset: 2px; }
  .option .dot {
    flex-shrink: 0; width: 18px; height: 18px; border-radius: 50%;
    border: 2px solid var(--cream-line); transition: border-color .16s ease, background .16s ease;
  }
  .option:hover .dot { border-color: var(--terra); }

  .linkbtn {
    background: none; border: none; cursor: pointer;
    font-family: 'Inter', sans-serif; padding: 0;
  }
  .linkbtn:focus-visible { outline: 2px solid var(--navy); outline-offset: 3px; border-radius: 3px; }

  /* ── progress ── */
  .progress-track {
    height: 5px; background: var(--cream-line); border-radius: 100px; overflow: hidden;
  }
  .progress-fill {
    height: 100%; background: var(--terra); border-radius: 100px;
    width: 0%; transition: width .45s cubic-bezier(.4,0,.2,1);
  }

  /* ── stage animation ── */
  .stage { animation: fadeUp .42s cubic-bezier(.22,.61,.36,1) both; }
  .stage.leaving { animation: fadeDown .22s ease both; }
  @keyframes fadeUp   { from { opacity: 0; transform: translateY(14px); } to { opacity: 1; transform: translateY(0); } }
  @keyframes fadeDown { from { opacity: 1; transform: translateY(0); }    to { opacity: 0; transform: translateY(-10px); } }

  /* staggered option entrance */
  .option { animation: optIn .4s ease both; }
  .option:nth-child(1){animation-delay:.04s}
  .option:nth-child(2){animation-delay:.10s}
  .option:nth-child(3){animation-delay:.16s}
  .option:nth-child(4){animation-delay:.22s}
  @keyframes optIn { from { opacity:0; transform: translateY(8px);} to {opacity:1; transform:translateY(0);} }

  /* result bars animate width via inline transition */
  .bar-fill { height: 100%; width: 0; border-radius: 100px; transition: width .8s cubic-bezier(.4,0,.2,1); }

  @media (prefers-reduced-motion: reduce) {
    *, .stage, .option { animation: none !important; transition: none !important; }
  }
</style>
</head>
<body>
<div class="wrap"><div id="app"></div></div>

<script>
"use strict";

const BOOKING_URL = "https://app.themomentummakerinc.com/widget/booking/0OoGasonQ9TFCcRxsT3y";

const C = {
  cream:"#F6EFE3", creamCard:"#FCF8F0", creamLine:"#E7DCC8",
  navy:"#1F3A52", navyDeep:"#16293A", terra:"#C8654A", terraSoft:"#E5B6A6",
  ink:"#2C2A26", muted:"#7C7363"
};

// Each driver keeps a distinct hue drawn from the cream/navy/terracotta family.
const TYPES = {
  bloodSugar: {
    name:"The Blood Sugar Driver", tag:"Insulin & glucose swings", color:"#C8654A",
    blurb:"Your weight gain is most likely tied to how your body is handling carbohydrates right now. As estrogen drops, cells get more insulin resistant — so the same meals spike your blood sugar harder and store more around the middle. The good news: this is one of the most responsive levers you have.",
    moves:["Build meals around 30g+ protein so glucose rises gently","Eat protein, fat and fiber before the carbs on your plate","Walk for 10 minutes after meals to blunt the spike"] },
  cortisol: {
    name:"The Stress & Cortisol Driver", tag:"The over-doing trap", color:"#9A6A86",
    blurb:"Your body reads chronic stress as an emergency — and responds by holding onto fat, especially around the belly. Ironically, under-eating and over-exercising make this worse, not better. Your work here isn't to push harder. It's to lower the alarm.",
    moves:["Stop the under-eat / over-train cycle — it raises cortisol","Anchor your day with a few minutes of nervous-system downshift","Trade one punishing workout a week for a walk or strength session"] },
  muscle: {
    name:"The Muscle-Loss Driver", tag:"Metabolism & strength", color:"#4E7C6B",
    blurb:"Starting in your 40s, muscle quietly slips away — and with it, the engine that burns energy all day. Less muscle means a slower metabolism, more fragile blood sugar, and a body that gains more easily. This is the lever with the longest payoff.",
    moves:["Strength train 2–3x a week — this is non-negotiable now","Hit a real protein target (aim ~30g per meal) to rebuild","Prioritize progressive overload over endless cardio"] },
  sleep: {
    name:"The Sleep-Deprived Driver", tag:"Recovery & cravings", color:"#1F3A52",
    blurb:"Poor sleep is quietly running the show. One rough night raises hunger hormones, deepens cravings, and worsens insulin sensitivity the very next day. Fix sleep and a surprising amount of the rest gets easier on its own.",
    moves:["Protect a consistent wind-down and wake time","Steady your evening blood sugar so you don't wake at 3am","Treat sleep as a metabolic intervention, not a luxury"] },
  inflammation: {
    name:"The Inflammation Driver", tag:"Whole-body, low-grade fire", color:"#B5413A",
    blurb:"Low-grade, chronic inflammation can quietly stall fat loss, drive water retention, and leave you puffy, achy and stuck despite doing the right things. As estrogen — which has an anti-inflammatory effect — declines, this fire gets harder to keep cool. Calming it tends to unlock everything else.",
    moves:["Crowd the plate with colorful, anti-inflammatory whole foods","Notice puffiness or aches after specific foods, and adjust","Build in real recovery — inflammation thrives on no rest"] },
  gut: {
    name:"The Gut Health Driver", tag:"Digestion & the microbiome", color:"#7C7A3D",
    blurb:"Your gut helps regulate hormones, hunger signals, and how you metabolize food — and the menopause transition reshapes the microbiome itself. Bloating, irregularity or feeling 'off' after eating are clues that this is your starting point. Tend the gut and absorption, cravings and mood often follow.",
    moves:["Aim for 30+ different plants a week to feed your microbiome","Prioritize fiber and fermented foods to restore balance","Slow down at meals — digestion starts before the first bite"] },
};

const QUESTIONS = [
  { q:"Where do you notice the change in your body most?", options:[
    { label:"Right around my middle — it's new", w:{bloodSugar:2,cortisol:1} },
    { label:"Everywhere feels softer, less firm", w:{muscle:2} },
    { label:"Puffy and bloated more than actually heavier", w:{inflammation:2,gut:1} },
    { label:"It piled on during a stressful stretch", w:{cortisol:2} } ] },
  { q:"Mid-afternoon, you most often feel…", options:[
    { label:"A crash — I need something sweet", w:{bloodSugar:2} },
    { label:"Wired but tired, can't switch off", w:{cortisol:2} },
    { label:"Drained, like I never recovered from last night", w:{sleep:2} },
    { label:"Weak — stairs and bags feel harder lately", w:{muscle:2} } ] },
  { q:"How does your body feel after meals?", options:[
    { label:"Bloated or gassy, even with 'healthy' food", w:{gut:2} },
    { label:"Sleepy and foggy, like I need a nap", w:{bloodSugar:2} },
    { label:"Puffy, or my rings and joints feel tight", w:{inflammation:2} },
    { label:"Generally fine, no real pattern", w:{muscle:1,sleep:1} } ] },
  { q:"How would you describe your sleep over the last month?", options:[
    { label:"Waking around 2–4am, then lying there", w:{sleep:2,cortisol:1} },
    { label:"Fine hours, but I wake up unrefreshed", w:{sleep:2} },
    { label:"Hard to wind down — mind won't stop", w:{cortisol:2,sleep:1} },
    { label:"Honestly, sleep's not my main issue", w:{bloodSugar:1,gut:1} } ] },
  { q:"Be honest about your typical breakfast:", options:[
    { label:"Coffee, maybe toast or a pastry", w:{bloodSugar:2} },
    { label:"I often skip it / not hungry", w:{cortisol:1,bloodSugar:1} },
    { label:"Something quick, probably low protein", w:{muscle:2,bloodSugar:1} },
    { label:"Eggs or protein — I'm pretty solid here", w:{gut:1} } ] },
  { q:"Your relationship with exercise right now:", options:[
    { label:"Lots of cardio, but not seeing results", w:{muscle:2,cortisol:1} },
    { label:"I push hard — rest feels like cheating", w:{cortisol:2,inflammation:1} },
    { label:"Too wiped out to be consistent", w:{sleep:2} },
    { label:"Little to no strength training", w:{muscle:2} } ] },
  { q:"How's your digestion these days?", options:[
    { label:"Bloating or irregularity I didn't used to have", w:{gut:2} },
    { label:"Certain foods clearly don't agree with me", w:{gut:1,inflammation:1} },
    { label:"Sluggish when I'm stressed", w:{cortisol:1,gut:1} },
    { label:"Pretty regular, no complaints", w:{bloodSugar:1,muscle:1} } ] },
  { q:"Beyond the scale, what bothers you most?", options:[
    { label:"Aches, stiffness or joint pain", w:{inflammation:2} },
    { label:"Brain fog and low mood", w:{gut:1,inflammation:1} },
    { label:"Energy that spikes and crashes", w:{bloodSugar:2} },
    { label:"Feeling weak and losing tone", w:{muscle:2} } ] },
  { q:"Cravings tend to hit you…", options:[
    { label:"An hour or two after eating carbs", w:{bloodSugar:2} },
    { label:"Hardest on days I slept badly", w:{sleep:2} },
    { label:"When I'm stressed or stretched thin", w:{cortisol:2} },
    { label:"Late at night, even after dinner", w:{sleep:1,bloodSugar:1} } ] },
  { q:"Which frustration sounds most like you?", options:[
    { label:"\u201CI'm eating less than ever and still gaining\u201D", w:{cortisol:2,bloodSugar:1} },
    { label:"\u201CI feel puffy and inflamed no matter what\u201D", w:{inflammation:2} },
    { label:"\u201CMy gut's been off and nothing feels right\u201D", w:{gut:2} },
    { label:"\u201CMy metabolism just died overnight\u201D", w:{muscle:2} } ] },
];

// ── state ──
let answers = [];
let current = 0;
const incomingParams = new URLSearchParams(window.location.search);
const app = document.getElementById("app");

function esc(s){ return String(s).replace(/[&<>"]/g,c=>({"&":"&amp;","<":"&lt;",">":"&gt;",'"':"&quot;"}[c])); }

function scores(){
  const s = {bloodSugar:0,cortisol:0,muscle:0,sleep:0,inflammation:0,gut:0};
  answers.forEach(w=>{ if(w) for(const k in w) s[k]+=w[k]; });
  return s;
}

function buildBookingUrl(primaryKey){
  const u = new URL(BOOKING_URL);
  for (const [k,v] of incomingParams.entries()) u.searchParams.set(k,v);
  u.searchParams.set("driver", primaryKey);
  return u.toString();
}

// swap content with a brief leaving animation
function swap(html, after){
  const cur = app.querySelector(".stage");
  const mount = () => { app.innerHTML = html; if (after) after(); };
  if (cur && !matchMedia("(prefers-reduced-motion: reduce)").matches){
    cur.classList.add("leaving");
    setTimeout(mount, 200);
  } else { mount(); }
}

function renderIntro(){
  swap(`
    <div class="stage" style="text-align:center;padding-top:30px;">
      <p class="eyebrow" style="margin-bottom:22px;">A 2-minute root-cause quiz</p>
      <h1 class="display" style="font-size:42px;margin-bottom:20px;">
        You're eating less<br>and gaining more.<br><em style="color:var(--terra);">Here's why.</em>
      </h1>
      <p style="font-size:16.5px;line-height:1.6;color:var(--muted);max-width:430px;margin:0 auto 34px;">
        In perimenopause, weight gain isn't about willpower or calories. It usually traces back to one of six hidden drivers. Answer 10 quick questions and find yours.
      </p>
      <button class="btn-primary" id="start">Find my driver →</button>
      <p style="font-size:11.5px;color:#A89B85;margin-top:26px;letter-spacing:.02em;">
        Educational only — not medical advice or a diagnosis. No data collected.
      </p>
    </div>`, () => {
      document.getElementById("start").onclick = () => { current=0; renderQuestion(); };
    });
}

function renderQuestion(){
  const Q = QUESTIONS[current];
  const pct = (current / QUESTIONS.length) * 100;
  const opts = Q.options.map((o,i)=>`
    <button class="option" data-i="${i}">
      <span class="dot"></span><span>${esc(o.label)}</span>
    </button>`).join("");
  swap(`
    <div class="stage">
      <div style="display:flex;justify-content:space-between;align-items:center;font-size:11.5px;letter-spacing:.12em;color:var(--muted);margin-bottom:11px;font-weight:600;">
        <span>QUESTION ${current+1} / ${QUESTIONS.length}</span>
        <button class="linkbtn" id="over" style="color:var(--muted);font-size:11.5px;letter-spacing:.12em;font-weight:600;">START OVER</button>
      </div>
      <div class="progress-track" style="margin-bottom:30px;">
        <div class="progress-fill" id="pf"></div>
      </div>
      <h2 class="display" style="font-size:27px;line-height:1.25;margin-bottom:26px;min-height:68px;">${esc(Q.q)}</h2>
      <div style="display:flex;flex-direction:column;gap:11px;">${opts}</div>
      ${current>0 ? `<button class="linkbtn" id="back" style="margin-top:22px;color:var(--navy);font-size:13px;font-weight:500;">← Back</button>` : ""}
    </div>`, () => {
      // animate progress to current step after mount
      requestAnimationFrame(()=>{ document.getElementById("pf").style.width = pct + "%"; });
      app.querySelectorAll(".option").forEach(b=>{
        b.onclick = () => {
          const i = +b.dataset.i;
          answers[current] = Q.options[i].w;
          // fill the dot briefly for feedback
          b.querySelector(".dot").style.background = "var(--terra)";
          b.querySelector(".dot").style.borderColor = "var(--terra)";
          setTimeout(()=>{
            if (current < QUESTIONS.length-1){ current++; renderQuestion(); }
            else { renderResult(); }
          }, 160);
        };
      });
      document.getElementById("over").onclick = () => { answers=[]; current=0; renderIntro(); };
      const back = document.getElementById("back");
      if (back) back.onclick = () => { current--; renderQuestion(); };
    });
}

function renderResult(){
  const s = scores();
  const ranked = Object.entries(s).sort((a,b)=>b[1]-a[1]);
  const total = Object.values(s).reduce((a,b)=>a+b,0) || 1;
  const primaryKey = ranked[0][0];
  const primary = TYPES[primaryKey];
  const secondary = TYPES[ranked[1][0]];
  const scored = ranked.filter(([,v])=>v>0);
  const bookingUrl = buildBookingUrl(primaryKey);

  const bars = scored.map(([k,v])=>`
    <div style="margin-bottom:11px;">
      <div style="display:flex;justify-content:space-between;font-size:12px;color:var(--muted);margin-bottom:5px;font-weight:500;">
        <span>${esc(TYPES[k].name.replace("The ",""))}</span><span>${Math.round((v/total)*100)}%</span>
      </div>
      <div style="height:9px;background:var(--cream-line);border-radius:100px;overflow:hidden;">
        <div class="bar-fill" data-w="${(v/total)*100}" style="background:${TYPES[k].color};"></div>
      </div>
    </div>`).join("");

  const moves = primary.moves.map((m,i)=>`
    <div style="display:flex;gap:13px;align-items:flex-start;margin-bottom:13px;">
      <span style="flex-shrink:0;width:23px;height:23px;border-radius:50%;background:${primary.color};color:#fff;font-size:12px;font-weight:600;display:flex;align-items:center;justify-content:center;">${i+1}</span>
      <span style="font-size:15px;line-height:1.5;">${esc(m)}</span>
    </div>`).join("");

  swap(`
    <div class="stage" style="padding-top:8px;">
      <div class="progress-track" style="margin-bottom:26px;"><div class="progress-fill" style="width:100%;"></div></div>
      <p class="eyebrow" style="color:var(--muted);text-align:center;display:block;margin-bottom:9px;">Your primary driver</p>
      <h1 class="display" style="font-size:34px;text-align:center;margin-bottom:6px;color:${primary.color};">${esc(primary.name)}</h1>
      <p style="text-align:center;font-size:12.5px;letter-spacing:.06em;color:var(--muted);margin-bottom:26px;font-weight:500;">${esc(primary.tag)}</p>

      <div style="margin-bottom:26px;">${bars}</div>

      <p style="font-size:16px;line-height:1.65;color:#403B33;">${esc(primary.blurb)}</p>
      <p style="font-size:14.5px;line-height:1.6;color:var(--muted);margin-top:14px;">
        You also lean toward <strong style="color:${secondary.color};">${esc(secondary.name.replace("The ",""))}</strong>. Most women are a blend of two or three drivers — which is exactly why a one-size-fits-all plan tends to fall flat for you.
      </p>

      <div style="background:var(--cream-card);border:1px solid var(--cream-line);border-radius:16px;padding:22px 24px;margin-top:24px;">
        <p class="eyebrow" style="display:block;color:var(--muted);margin-bottom:15px;">Your 3 first moves</p>
        ${moves}
      </div>

      <div style="background:var(--navy);border-radius:16px;padding:30px 26px;margin-top:22px;text-align:center;">
        <p class="eyebrow" style="display:block;color:var(--terra-soft);margin-bottom:12px;">Your next step</p>
        <h3 class="display" style="color:var(--cream);font-size:24px;line-height:1.25;margin-bottom:12px;">Let's build your custom roadmap together</h3>
        <p style="color:#C5D2DC;font-size:14.5px;line-height:1.6;margin:0 0 22px;">Your results are the starting point, not the whole picture. On a free call, we'll map your unique blend of drivers into a clear, personalized plan — no restriction, no guilt, no calorie-counting.</p>
        <a href="${esc(bookingUrl)}" target="_blank" rel="noopener noreferrer" style="display:inline-block;background:var(--terra);color:#fff;text-decoration:none;padding:15px 34px;border-radius:100px;font-size:14.5px;font-weight:600;letter-spacing:.01em;">Book my free roadmap call →</a>
        <p style="font-size:11.5px;color:#8FA3B2;margin:16px 0 0;letter-spacing:.02em;">30 minutes · no pressure · bring your results</p>
      </div>

      <button class="linkbtn" id="retake" style="display:block;margin:24px auto 0;color:var(--navy);font-size:13px;font-weight:500;">↺ Retake the quiz</button>
    </div>`, () => {
      requestAnimationFrame(()=>{
        app.querySelectorAll(".bar-fill").forEach(el=>{ el.style.width = el.dataset.w + "%"; });
      });
      document.getElementById("retake").onclick = () => { answers=[]; current=0; renderIntro(); };
    });
}

renderIntro();
</script>
</body>
</html>

