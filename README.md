<!DOCTYPE html>
<html lang="th">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>S.P.P. Detective Unit — คดีภาชนะแตก</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Kanit:wght@500;600;700&family=Sarabun:wght@400;500;600;700&display=swap" rel="stylesheet">
<style>
  :root{
    --bg:#12151c;
    --board:#1b2029;
    --board-line: rgba(255,255,255,0.04);
    --paper:#f4ecd8;
    --paper-dim:#e9ddc0;
    --ink:#28221a;
    --ink-soft:#6b5d49;
    --amber:#d99a3f;
    --amber-deep:#b9762a;
    --red:#b23a2f;
    --teal:#3f8f7a;
    --thread: rgba(178,58,47,0.55);
    --radius: 3px;
  }
  *{ box-sizing:border-box; }
  html{ scroll-behavior:smooth; }
  body{
    margin:0;
    background:
      radial-gradient(ellipse at 20% -10%, rgba(217,154,63,0.08), transparent 45%),
      radial-gradient(ellipse at 90% 10%, rgba(63,143,122,0.06), transparent 40%),
      var(--bg);
    color:var(--paper);
    font-family:'Sarabun', sans-serif;
    line-height:1.65;
    -webkit-font-smoothing:antialiased;
  }
  @media (prefers-reduced-motion: reduce){
    *{ animation-duration:0.001ms !important; transition-duration:0.001ms !important; }
  }

  h1,h2,h3,.stamp,.num{ font-family:'Kanit', sans-serif; }

  a{ color:inherit; }
  button{ font-family:inherit; cursor:pointer; }
  button:disabled{ cursor:not-allowed; opacity:0.45; }
  button:focus-visible, input:focus-visible, textarea:focus-visible{
    outline:2px solid var(--amber); outline-offset:2px;
  }

  .wrap{ max-width:760px; margin:0 auto; padding:0 20px 90px; }

  /* ---------- sticky score rail ---------- */
  .rail{
    position:sticky; top:0; z-index:30;
    background:rgba(18,21,28,0.92);
    backdrop-filter: blur(6px);
    border-bottom:1px solid var(--board-line);
    padding:12px 20px;
  }
  .rail-inner{
    max-width:760px; margin:0 auto;
    display:flex; align-items:center; justify-content:space-between; gap:14px;
    font-size:14px;
  }
  .rail-case{ color:var(--amber); font-family:'Kanit'; font-weight:600; letter-spacing:0.02em; }
  .rail-stats{ display:flex; gap:18px; align-items:center; }
  .rail-stat{ display:flex; flex-direction:column; align-items:flex-end; line-height:1.2; }
  .rail-stat .n{ font-family:'Kanit'; font-weight:700; font-size:17px; color:var(--paper); }
  .rail-stat .l{ font-size:11px; color:#8b93a3; }

  /* ---------- hero ---------- */
  .hero{ padding:44px 0 8px; }
  .badge-row{ display:flex; align-items:center; gap:10px; margin-bottom:18px; }
  .badge{
    font-family:'Kanit'; font-size:12px; font-weight:600; color:var(--amber);
    border:1px solid rgba(217,154,63,0.4); padding:4px 10px; border-radius:20px;
  }
  h1{
    font-size:32px; font-weight:700; margin:0 0 14px; color:var(--paper);
    line-height:1.3;
  }
  .lede{ color:#c7cbd4; font-size:16px; max-width:62ch; }
  .team-field{
    margin-top:24px; display:flex; align-items:center; gap:10px; flex-wrap:wrap;
  }
  .team-field label{ font-size:13px; color:#9098a8; }
  .team-field input{
    background:transparent; border:none; border-bottom:1px solid rgba(255,255,255,0.25);
    color:var(--paper); font-family:'Kanit'; font-size:16px; padding:4px 2px; width:220px;
  }
  .team-field input::placeholder{ color:#5f6779; }

  .case-note{
    background:var(--paper); color:var(--ink); border-radius:var(--radius);
    padding:22px 24px; margin-top:26px; box-shadow:0 14px 30px -14px rgba(0,0,0,0.55);
    position:relative;
  }
  .case-note::before{
    content:""; position:absolute; top:-9px; left:26px; width:16px; height:16px;
    border-radius:50%; background:radial-gradient(circle at 35% 30%, #e6746a, var(--red));
    box-shadow:0 3px 5px rgba(0,0,0,0.4);
  }
  .case-note p{ margin:0 0 10px; font-size:15px; color:var(--ink); }
  .case-note p:last-child{ margin-bottom:0; }
  .case-note strong{ color:var(--amber-deep); }

  /* ---------- section headers ---------- */
  .section{ margin-top:56px; }
  .section-head{ display:flex; align-items:baseline; gap:10px; margin-bottom:6px; }
  .section-num{ font-family:'Kanit'; color:var(--amber); font-weight:700; font-size:14px; }
  .section h2{ font-size:22px; margin:0; color:var(--paper); }
  .section-sub{ color:#8b93a3; font-size:14px; margin:6px 0 24px; max-width:60ch; }

  /* ---------- suspects ---------- */
  .suspects{ display:grid; grid-template-columns:repeat(2,1fr); gap:14px; }
  @media (max-width:560px){ .suspects{ grid-template-columns:1fr; } }
  .suspect{
    background:var(--paper); color:var(--ink); border-radius:var(--radius);
    padding:16px 18px; box-shadow:0 10px 22px -14px rgba(0,0,0,0.5);
    transform:rotate(var(--tilt,0deg));
  }
  .suspect:nth-child(1){ --tilt:-0.6deg; }
  .suspect:nth-child(2){ --tilt:0.5deg; }
  .suspect:nth-child(3){ --tilt:-0.4deg; }
  .suspect:nth-child(4){ --tilt:0.6deg; }
  .suspect-top{ display:flex; align-items:center; gap:10px; margin-bottom:8px; }
  .suspect-code{
    width:30px; height:30px; border-radius:50%; background:var(--ink);
    color:var(--paper); font-family:'Kanit'; font-weight:700; font-size:14px;
    display:flex; align-items:center; justify-content:center; flex:none;
  }
  .suspect-name{ font-family:'Kanit'; font-weight:600; font-size:16px; }
  .suspect-quote{ font-size:14px; color:var(--ink-soft); font-style:italic; }

  /* ---------- evidence timeline ---------- */
  .timeline{ position:relative; padding-left:26px; }
  .timeline::before{
    content:""; position:absolute; left:9px; top:6px; bottom:6px; width:2px;
    background:repeating-linear-gradient(to bottom, var(--thread) 0 6px, transparent 6px 12px);
  }
  .ev{ position:relative; margin-bottom:16px; }
  .ev-dot{
    position:absolute; left:-26px; top:18px; width:20px; height:20px; border-radius:50%;
    background:var(--board); border:2px solid #3a4152; display:flex; align-items:center; justify-content:center;
    font-family:'Kanit'; font-size:10px; font-weight:700; color:#8b93a3;
  }
  .ev.is-open .ev-dot, .ev.is-done .ev-dot{ border-color:var(--amber); color:var(--amber); }
  .ev.is-done .ev-dot{ background:var(--amber); color:var(--ink); border-color:var(--amber); }

  .ev-card{
    background:var(--board); border:1px solid var(--board-line); border-radius:var(--radius);
    padding:16px 18px;
  }
  .ev-head{ display:flex; align-items:center; justify-content:space-between; gap:10px; }
  .ev-title{ font-family:'Kanit'; font-weight:600; font-size:15px; color:var(--paper); }
  .ev-open-btn{
    background:transparent; border:1px solid rgba(217,154,63,0.5); color:var(--amber);
    font-size:13px; font-weight:600; padding:7px 14px; border-radius:20px;
  }
  .ev-open-btn:hover:not(:disabled){ background:rgba(217,154,63,0.12); }
  .ev-body{ margin-top:14px; display:none; }
  .ev.is-open .ev-body, .ev.is-done .ev-body{ display:block; }
  .ev-body p{ font-size:14.5px; color:#c7cbd4; margin:0 0 12px; }
  .ev-body p:last-of-type{ margin-bottom:14px; }

  .quiz{ background:rgba(255,255,255,0.03); border:1px solid var(--board-line); border-radius:var(--radius); padding:12px 14px; }
  .quiz-q{ font-size:13.5px; color:var(--amber); margin:0 0 10px; font-weight:600; }
  .opts{ display:flex; flex-wrap:wrap; gap:8px; }
  .opt-btn{
    background:transparent; border:1px solid rgba(255,255,255,0.16); color:#dfe3ea;
    font-size:13.5px; padding:7px 12px; border-radius:6px;
  }
  .opt-btn:hover:not(:disabled){ border-color:rgba(255,255,255,0.4); }
  .opt-btn.correct{ border-color:var(--teal); background:rgba(63,143,122,0.18); color:#a6e6d4; }
  .opt-btn.wrong{ border-color:var(--red); background:rgba(178,58,47,0.16); color:#f0b6ae; }
  .chk-row{ display:flex; flex-wrap:wrap; gap:8px; margin-bottom:10px; }
  .chk{
    display:flex; align-items:center; gap:6px; font-size:13.5px; color:#dfe3ea;
    border:1px solid rgba(255,255,255,0.16); padding:6px 10px; border-radius:6px;
  }
  .num-row{ display:flex; align-items:center; gap:8px; }
  .num-row input{
    width:90px; background:rgba(255,255,255,0.05); border:1px solid rgba(255,255,255,0.18);
    color:var(--paper); border-radius:6px; padding:7px 10px; font-family:'Kanit'; font-size:14px;
  }
  .go-btn{
    background:var(--amber); color:var(--ink); border:none; font-weight:700; font-size:13.5px;
    padding:7px 14px; border-radius:6px;
  }
  .go-btn:hover:not(:disabled){ background:#e6ab54; }
  .feedback{ margin-top:10px; font-size:13.5px; padding:8px 10px; border-radius:6px; display:none; }
  .feedback.show{ display:block; }
  .feedback.ok{ background:rgba(63,143,122,0.14); color:#a6e6d4; }
  .feedback.no{ background:rgba(178,58,47,0.12); color:#f0b6ae; }
  .feedback.info{ background:rgba(217,154,63,0.12); color:#f0d6a6; }
  .pts-tag{ font-family:'Kanit'; font-size:11px; color:#8b93a3; margin-left:8px; }

  /* ---------- report table ---------- */
  .report{ display:grid; gap:10px; }
  .report-row{
    background:var(--board); border:1px solid var(--board-line); border-radius:var(--radius);
    padding:12px 14px;
  }
  .report-row label{ display:block; font-size:13px; color:var(--amber); margin-bottom:6px; font-weight:600; }
  .report-row textarea{
    width:100%; min-height:44px; resize:vertical; background:rgba(255,255,255,0.03);
    border:1px solid rgba(255,255,255,0.14); border-radius:6px; color:var(--paper);
    font-family:'Sarabun'; font-size:14px; padding:9px 10px;
  }
  .locked-note{
    font-size:13px; color:#8b93a3; border:1px dashed rgba(255,255,255,0.18);
    border-radius:var(--radius); padding:14px 16px;
  }

  /* ---------- accusation ---------- */
  .warn-banner{
    display:none; font-size:13.5px; background:rgba(178,58,47,0.14); color:#f0b6ae;
    border:1px solid rgba(178,58,47,0.4); border-radius:6px; padding:10px 12px; margin-bottom:16px;
  }
  .warn-banner.show{ display:block; }
  .accuse-grid{ display:grid; grid-template-columns:repeat(4,1fr); gap:10px; margin-bottom:18px; }
  @media (max-width:560px){ .accuse-grid{ grid-template-columns:repeat(2,1fr); } }
  .accuse-btn{
    background:var(--paper); color:var(--ink); border:3px solid transparent; border-radius:var(--radius);
    padding:12px 8px; text-align:center; font-family:'Kanit'; font-weight:600; font-size:14px;
  }
  .accuse-btn .accuse-code{
    width:26px; height:26px; border-radius:50%; background:var(--ink); color:var(--paper);
    display:flex; align-items:center; justify-content:center; margin:0 auto 6px; font-size:12px;
  }
  .accuse-btn.picked{ border-color:var(--amber); }
  textarea.reason{
    width:100%; min-height:100px; resize:vertical; background:rgba(255,255,255,0.03);
    border:1px solid rgba(255,255,255,0.14); border-radius:6px; color:var(--paper);
    font-family:'Sarabun'; font-size:14.5px; padding:12px 14px; margin-bottom:14px;
  }
  .submit-btn{
    background:var(--red); color:#fff; border:none; font-family:'Kanit'; font-weight:600;
    font-size:15px; padding:11px 22px; border-radius:6px;
  }
  .submit-btn:hover:not(:disabled){ background:#c7473b; }

  /* ---------- verdict ---------- */
  .verdict{ display:none; margin-top:56px; }
  .verdict.show{ display:block; }
  .verdict-stamp{
    display:inline-block; font-family:'Kanit'; font-weight:700; font-size:13px;
    color:var(--teal); border:2px solid var(--teal); border-radius:6px; padding:5px 12px;
    transform:rotate(-3deg); margin-bottom:16px;
  }
  .verdict-card{
    background:var(--paper); color:var(--ink); border-radius:var(--radius); padding:24px 26px;
    box-shadow:0 16px 34px -16px rgba(0,0,0,0.55);
  }
  .verdict-card h3{ font-size:19px; margin:0 0 10px; }
  .verdict-card p{ font-size:15px; margin:0 0 12px; }
  .score-table{ width:100%; border-collapse:collapse; margin-top:16px; font-size:14px; }
  .score-table td{ padding:7px 4px; border-bottom:1px solid rgba(40,34,26,0.14); }
  .score-table td:last-child{ text-align:right; font-family:'Kanit'; font-weight:600; }
  .score-total td{ border-bottom:none; padding-top:12px; font-size:16px; font-weight:700; }
  .score-total td:last-child{ color:var(--amber-deep); }
  .reset-row{ text-align:center; margin-top:26px; }
  .reset-btn{
    background:transparent; border:1px solid rgba(255,255,255,0.25); color:#c7cbd4;
    font-family:'Kanit'; font-size:13px; padding:8px 18px; border-radius:20px;
  }
  .reset-btn:hover{ border-color:var(--amber); color:var(--amber); }

  footer{ text-align:center; color:#5f6779; font-size:12px; margin-top:70px; }
</style>
</head>
<body>

<div class="rail">
  <div class="rail-inner">
    <div class="rail-case">CASE FILE #0724</div>
    <div class="rail-stats">
      <div class="rail-stat"><span class="n" id="scoreNow">0</span><span class="l">คะแนน / 20</span></div>
      <div class="rail-stat"><span class="n" id="evNow">0/6</span><span class="l">หลักฐาน</span></div>
    </div>
  </div>
</div>

<div class="wrap">

  <section class="hero">
    <div class="badge-row"><span class="badge">S.P.P. Detective Unit</span></div>
    <h1>ใครทำภาชนะในห้องทดลองแตก?</h1>
    <p class="lede">ทีมของนักเรียนคือหน่วยสืบสวนวิทยาศาสตร์ประจำโรงเรียน ภารกิจคือตามหาความจริงจากหลักฐานเท่านั้น ห้ามเดาสุ่ม — เปิดหลักฐานทีละชิ้น วิเคราะห์ให้ครบ แล้วค่อยกล่าวหา</p>
    <div class="team-field">
      <label for="teamName">ชื่อทีมสืบสวน</label>
      <input id="teamName" type="text" placeholder="เช่น ทีมกล้องจุลทรรศน์">
    </div>
    <div class="case-note">
      <p><strong>เวลา 12.20 น.</strong> หลังพักกลางวัน นักเรียนห้อง ม.3 กลับเข้าห้องวิทยาศาสตร์และพบว่าภาชนะใส่สารตัวอย่างบนโต๊ะทดลองแตกกระจายอยู่บนพื้น มีของเหลวไหลออกมา และมีวัตถุบางอย่างตกอยู่ใกล้ ๆ</p>
      <p>ไม่มีใครยอมรับว่าเป็นคนทำ ครูจึงแต่งตั้งทุกทีมเป็นหน่วยสืบสวน เพื่อไขคดีนี้ด้วยหลักฐานทางวิทยาศาสตร์</p>
    </div>
  </section>

  <section class="section" id="suspectsSection">
    <div class="section-head"><span class="section-num">01</span><h2>ผู้ต้องสงสัย</h2></div>
    <p class="section-sub">อ่านคำให้การของแต่ละคนไว้ก่อน — ยังห้ามตัดสินใครทั้งนั้นจนกว่าหลักฐานจะครบ</p>
    <div class="suspects">
      <div class="suspect"><div class="suspect-top"><div class="suspect-code">A</div><div class="suspect-name">นัท</div></div><div class="suspect-quote">"ผมแค่วางขวดน้ำพลาสติกไว้บนโต๊ะ แล้วออกไปกินข้าวครับ"</div></div>
      <div class="suspect"><div class="suspect-top"><div class="suspect-code">B</div><div class="suspect-name">มีน</div></div><div class="suspect-quote">"หนูเอาแก้วใส่น้ำเย็นมาวางใกล้ ๆ แต่ไม่ได้แตะอุปกรณ์เลยค่ะ"</div></div>
      <div class="suspect"><div class="suspect-top"><div class="suspect-code">C</div><div class="suspect-name">ต้น</div></div><div class="suspect-quote">"ผมใช้ช้อนโลหะคนสารแล้ววางไว้ข้างโต๊ะครับ"</div></div>
      <div class="suspect"><div class="suspect-top"><div class="suspect-code">D</div><div class="suspect-name">ฟ้า</div></div><div class="suspect-quote">"หนูเทน้ำร้อนลงในภาชนะ เพราะคิดว่าภาชนะทุกชนิดทนความร้อนได้ค่ะ"</div></div>
    </div>
  </section>

  <section class="section" id="evidenceSection">
    <div class="section-head"><span class="section-num">02</span><h2>หลักฐาน</h2></div>
    <p class="section-sub">เปิดหลักฐานทีละชิ้นตามลำดับ แต่ละชิ้นมีคำถามให้วิเคราะห์ — ตอบให้ได้คะแนนสะสม</p>
    <div class="timeline" id="timeline"></div>
  </section>

  <section class="section" id="reportSection">
    <div class="section-head"><span class="section-num">03</span><h2>Detective Report</h2></div>
    <p class="section-sub">สรุปข้อสรุปของทีมเป็นคำพูดของตัวเอง ก่อนเข้าสู่การกล่าวหาขั้นสุดท้าย</p>
    <div id="reportLocked" class="locked-note">เปิดหลักฐานให้ครบ 6 ชิ้นก่อน แล้ว Detective Report จะปลดล็อกที่นี่</div>
    <div id="reportForm" class="report" style="display:none;"></div>
  </section>

  <section class="section" id="accuseSection">
    <div class="section-head"><span class="section-num">04</span><h2>FINAL ACCUSATION</h2></div>
    <p class="section-sub">เลือกผู้ต้องสงสัยที่ทีมคิดว่าเกี่ยวข้องมากที่สุด แล้วอธิบายด้วยหลักวิทยาศาสตร์ — ห้ามตอบแค่ "เพราะเทน้ำร้อน" เฉย ๆ</p>
    <div id="warnBanner" class="warn-banner">⚠️ ยังเปิดหลักฐานไม่ครบ (<span id="warnCount">0</span>/6) — หากกล่าวหาผิดตอนนี้ ทีมจะถูกหัก 3 คะแนนทันที</div>
    <div class="accuse-grid" id="accuseGrid"></div>
    <textarea class="reason" id="reasonBox" placeholder="อธิบายเหตุผลทางวิทยาศาสตร์ของทีม เช่น วัสดุคืออะไร อุณหภูมิเปลี่ยนไปเท่าไร เกิดอะไรขึ้นกับเนื้อวัสดุ..."></textarea>
    <button class="submit-btn" id="submitBtn" disabled>ยื่นข้อกล่าวหา</button>
  </section>

  <section class="verdict" id="verdictSection">
    <div class="section-head"><span class="section-num">05</span><h2>คำตัดสิน</h2></div>
    <div class="verdict-stamp" id="verdictStamp">ปิดคดี</div>
    <div class="verdict-card">
      <h3 id="verdictTitle"></h3>
      <p id="verdictBody"></p>
      <p id="verdictExplain"></p>
      <table class="score-table" id="scoreTable"></table>
    </div>
    <div class="reset-row"><button class="reset-btn" onclick="location.reload()">↺ เริ่มคดีใหม่</button></div>
  </section>

  <footer>S.P.P. Detective Unit — เกมสืบสวนวิทยาศาสตร์ประจำห้องเรียน ม.3</footer>
</div>

<script>
/* ---------------- data ---------------- */
const evidenceData = [
  {
    n:1, title:"เศษวัสดุที่พบบนพื้น",
    text:"เศษวัสดุมีผิวเรียบ โปร่งใส แข็ง และแตกเป็นชิ้นเมื่อได้รับแรงกระแทก",
    quizzes:[
      { type:"radio", key:"material", q:"วัสดุนี้น่าจะเป็นอะไร?", pts:2,
        opts:["แก้ว","พลาสติก","โลหะ","เซรามิก"], correct:"แก้ว" },
      { type:"check", key:"properties", q:"เลือกสมบัติทั้งหมดที่ตรงกับเศษวัสดุนี้ (เลือกได้หลายข้อ)", pts:3,
        opts:["ผิวเรียบ","โปร่งใส","แข็ง","แตกง่ายเมื่อกระแทก","นำไฟฟ้าได้","ยืดหยุ่นสูง"],
        correct:["ผิวเรียบ","โปร่งใส","แข็ง","แตกง่ายเมื่อกระแทก"] }
    ]
  },
  {
    n:2, title:"ภาพจากกล้องวงจรปิด",
    text:"กล้องวงจรปิดยืนยันว่า ไม่มีใครทำภาชนะตกจากโต๊ะ และไม่มีแรงกระแทกจากภายนอกเกิดขึ้นเลย",
    quizzes:[
      { type:"radio", key:"ev2", q:"ถ้าไม่ได้แตกจากการตกหรือแรงกระแทก สาเหตุอื่นที่เป็นไปได้มากที่สุดคืออะไร?", pts:2,
        opts:["การเปลี่ยนแปลงอุณหภูมิอย่างรวดเร็ว","เสียงดังในห้อง","แสงแดดส่องนาน ๆ","ความชื้นในอากาศ"],
        correct:"การเปลี่ยนแปลงอุณหภูมิอย่างรวดเร็ว" }
    ]
  },
  {
    n:3, title:"บันทึกอุณหภูมิ",
    text:"ก่อนเกิดเหตุ ภาชนะมีอุณหภูมิประมาณ 15°C จากนั้นมีของเหลวอุณหภูมิประมาณ 85°C ถูกเทลงไปทันที",
    quizzes:[
      { type:"number", key:"temp", q:"อุณหภูมิของภาชนะเปลี่ยนไปประมาณกี่องศาเซลเซียส?", pts:2, correct:70, unit:"°C" }
    ]
  },
  {
    n:4, title:"ข้อมูลจากห้องทดลอง: Thermal Shock",
    text:"แก้วบางชนิดเมื่อได้รับการเปลี่ยนแปลงอุณหภูมิอย่างรวดเร็ว ส่วนต่าง ๆ ของเนื้อแก้วจะขยายตัวไม่พร้อมกัน ทำให้เกิดความเค้นสะสมภายในเนื้อวัสดุ และอาจแตกร้าวได้ ปรากฏการณ์นี้เรียกว่า Thermal shock",
    quizzes:[
      { type:"radio", key:"thermal", q:"Thermal shock หมายถึงอะไร?", pts:2,
        opts:[
          "การเปลี่ยนแปลงอุณหภูมิฉับพลันจนวัสดุขยายตัวไม่เท่ากันและเกิดความเค้นจนเสียหาย",
          "การที่วัสดุสัมผัสไฟฟ้าแรงสูงจนละลาย",
          "การที่วัสดุเก่าจนเสื่อมสภาพตามเวลา",
          "การที่วัสดุถูกแสงแดดจนสีซีดจาง"
        ],
        correct:"การเปลี่ยนแปลงอุณหภูมิฉับพลันจนวัสดุขยายตัวไม่เท่ากันและเกิดความเค้นจนเสียหาย" }
    ]
  },
  {
    n:5, title:"จุดเริ่มต้นของรอยแตก",
    text:"ตรวจไม่พบร่องรอยการกระแทกที่จุดเริ่มต้นของรอยแตกเลย แต่รอยแตกกลับเริ่มจากบริเวณด้านในของภาชนะ",
    quizzes:[
      { type:"radio", key:"ev5", q:"รอยแตกที่เริ่มจากด้านในโดยไม่มีรอยกระแทก บ่งบอกอะไร?", pts:2,
        opts:["มีแรงกระแทกจากภายนอกที่มองไม่เห็น","เกิดความเค้นสะสมจากภายในเนื้อวัสดุเอง","ภาชนะใบนี้ผลิตไม่ได้มาตรฐาน","มีคนใช้มือบีบภาชนะ"],
        correct:"เกิดความเค้นสะสมจากภายในเนื้อวัสดุเอง" }
    ]
  },
  {
    n:6, title:"วัตถุใกล้จุดเกิดเหตุ",
    text:"พบวัตถุ 3 ชิ้นวางอยู่ใกล้จุดเกิดเหตุ: ช้อนโลหะ ขวดพลาสติก PET และเศษแก้ว — ไม่ใช่ทุกชิ้นที่พบในที่เกิดเหตุจะเป็นต้นเหตุของเรื่องนี้",
    quizzes:[
      { type:"radio", key:"ev6", q:"วัตถุใดคือสาเหตุโดยตรงที่ทำให้ภาชนะแตก?", pts:0,
        opts:["ช้อนโลหะ","ขวดพลาสติก PET","เศษแก้ว (ตัวภาชนะเอง)","ไม่มีวัตถุชิ้นใดข้างต้น — สาเหตุคือของเหลวร้อนที่ถูกเทลงไป"],
        correct:"ไม่มีวัตถุชิ้นใดข้างต้น — สาเหตุคือของเหลวร้อนที่ถูกเทลงไป",
        note:"ถูกต้อง! ช้อน ขวด และเศษแก้ว เป็นเพียงวัตถุที่พบในที่เกิดเหตุ ไม่ใช่ทุกอย่างที่พบจะเป็นต้นเหตุเสมอไป — นักสืบที่ดีต้องแยกแยะหลักฐานที่เกี่ยวข้องออกจากหลักฐานที่ไม่เกี่ยวข้อง" }
    ]
  }
];

const reportQuestions = [
  "1. วัสดุที่แตกคืออะไร",
  "2. สมบัติสำคัญของวัสดุที่ใช้วิเคราะห์",
  "3. สาเหตุที่ทำให้แตก",
  "4. หลักฐานที่สำคัญที่สุดของทีม",
  "5. ผู้ต้องสงสัยที่ทีมคาดว่าเกี่ยวข้อง",
  "6. เหตุผลทางวิทยาศาสตร์สนับสนุนข้อสรุป"
];

const suspects = [
  {code:"A", name:"นัท"},
  {code:"B", name:"มีน"},
  {code:"C", name:"ต้น"},
  {code:"D", name:"ฟ้า"}
];
const correctSuspect = "D";

const reasoningKeywords = [
  { label:"ระบุวัสดุ (แก้ว)", test:t=>/แก้ว/.test(t) },
  { label:"พูดถึงการเปลี่ยนอุณหภูมิ", test:t=>/อุณหภูมิ|70\s*°?c|70\s*องศา/i.test(t) },
  { label:"พูดถึงการขยายตัว", test:t=>/ขยายตัว/.test(t) },
  { label:"พูดถึงความเค้น", test:t=>/ความเค้น/.test(t) },
  { label:"เอ่ยถึง Thermal shock", test:t=>/thermal\s*shock|เทอร์มอล|ช็อกความร้อน/i.test(t) }
];

/* ---------------- state ---------------- */
let scores = { material:0, properties:0, ev2:0, temp:0, thermal:0, ev5:0, suspect:0, reasoning:0 };
let openedCount = 0;
let opened = {};
let pickedSuspect = null;
let submitted = false;

/* ---------------- render evidence ---------------- */
const timeline = document.getElementById('timeline');
evidenceData.forEach((ev, idx) => {
  const el = document.createElement('div');
  el.className = 'ev';
  el.id = 'ev' + ev.n;
  el.innerHTML = `
    <div class="ev-dot">${ev.n}</div>
    <div class="ev-card">
      <div class="ev-head">
        <div class="ev-title">หลักฐานที่ ${ev.n} — ${ev.title}</div>
        <button class="ev-open-btn" id="openBtn${ev.n}" ${idx===0?'':'disabled'} onclick="openEvidence(${ev.n})">เปิดหลักฐาน</button>
      </div>
      <div class="ev-body" id="evBody${ev.n}">
        <p>${ev.text}</p>
        <div id="quizHolder${ev.n}"></div>
      </div>
    </div>
  `;
  timeline.appendChild(el);
});

function openEvidence(n){
  const evEl = document.getElementById('ev' + n);
  const btn = document.getElementById('openBtn' + n);
  if(opened[n]) return;
  opened[n] = true;
  openedCount++;
  evEl.classList.add('is-open');
  btn.disabled = true;
  btn.textContent = 'เปิดแล้ว';

  const data = evidenceData.find(e => e.n === n);
  const holder = document.getElementById('quizHolder' + n);
  data.quizzes.forEach((q, qi) => holder.appendChild(buildQuiz(n, qi, q)));

  const next = document.getElementById('openBtn' + (n+1));
  if(next) next.disabled = false;

  updateRail();
  maybeUnlockReport();
}

function buildQuiz(evN, qi, q){
  const box = document.createElement('div');
  box.className = 'quiz';
  box.style.marginBottom = '10px';
  const ptsLabel = q.pts > 0 ? `<span class="pts-tag">(${q.pts} คะแนน)</span>` : `<span class="pts-tag">(ฝึกวิเคราะห์)</span>`;
  let inner = `<p class="quiz-q">${q.q}${ptsLabel}</p>`;

  if(q.type === 'radio'){
    inner += `<div class="opts">` + q.opts.map((o,i)=>
      `<button class="opt-btn" onclick="answerRadio(this,'${evN}_${qi}','${encodeURIComponent(o)}')">${o}</button>`
    ).join('') + `</div>`;
  } else if(q.type === 'check'){
    inner += `<div class="chk-row">` + q.opts.map((o,i)=>
      `<label class="chk"><input type="checkbox" data-check="${evN}_${qi}" value="${encodeURIComponent(o)}"> ${o}</label>`
    ).join('') + `</div><button class="go-btn" onclick="answerCheck(this,'${evN}_${qi}')">ตรวจคำตอบ</button>`;
  } else if(q.type === 'number'){
    inner += `<div class="num-row"><input type="number" id="num_${evN}_${qi}"><span>${q.unit||''}</span>
      <button class="go-btn" onclick="answerNumber(this,'${evN}_${qi}')">ตรวจคำตอบ</button></div>`;
  }
  inner += `<div class="feedback" id="fb_${evN}_${qi}"></div>`;
  box.innerHTML = inner;
  box.dataset.evN = evN; box.dataset.qi = qi;
  return box;
}

function showFeedback(evN, qi, ok, msg){
  const fb = document.getElementById(`fb_${evN}_${qi}`);
  fb.classList.add('show');
  fb.classList.add(ok===null ? 'info' : (ok ? 'ok' : 'no'));
  fb.textContent = msg;
}

function answerRadio(btn, id, encodedVal){
  const [evN, qi] = id.split('_');
  const q = evidenceData.find(e=>e.n==evN).quizzes[qi];
  const val = decodeURIComponent(encodedVal);
  const group = btn.parentElement.querySelectorAll('.opt-btn');
  group.forEach(b=>{ b.disabled = true; if(b.textContent === q.correct) b.classList.add('correct'); });
  if(val !== q.correct) btn.classList.add('wrong');

  if(q.note){
    showFeedback(evN, qi, null, q.note);
  } else if(val === q.correct){
    showFeedback(evN, qi, true, `ถูกต้อง! +${q.pts} คะแนน`);
  } else {
    showFeedback(evN, qi, false, `ยังไม่ถูก — คำตอบที่ถูกต้องคือ "${q.correct}"`);
  }

  if(q.key && val === q.correct){ scores[q.key] = q.pts; updateRail(); }
}

function answerCheck(btn, id){
  const [evN, qi] = id.split('_');
  const q = evidenceData.find(e=>e.n==evN).quizzes[qi];
  const box = btn.closest('.quiz');
  const checked = [...box.querySelectorAll('input[type=checkbox]:checked')].map(c=>decodeURIComponent(c.value));
  box.querySelectorAll('input[type=checkbox]').forEach(c=>c.disabled=true);
  btn.disabled = true;

  const correctSet = q.correct;
  let matches = checked.filter(c => correctSet.includes(c)).length;
  let wrongPicks = checked.filter(c => !correctSet.includes(c)).length;
  let earned = Math.max(0, Math.min(q.pts, matches - Math.floor(wrongPicks/2)));

  scores[q.key] = earned;
  updateRail();
  if(earned === q.pts){
    showFeedback(evN, qi, true, `เยี่ยม! ระบุสมบัติได้ครบถ้วน +${earned} คะแนน`);
  } else if(earned > 0){
    showFeedback(evN, qi, false, `ได้บางส่วน — สมบัติที่ควรเลือกคือ: ${correctSet.join(', ')} (+${earned} คะแนน)`);
  } else {
    showFeedback(evN, qi, false, `ยังไม่ตรง — สมบัติที่ควรเลือกคือ: ${correctSet.join(', ')}`);
  }
}

function answerNumber(btn, id){
  const [evN, qi] = id.split('_');
  const q = evidenceData.find(e=>e.n==evN).quizzes[qi];
  const input = document.getElementById(`num_${evN}_${qi}`);
  const val = parseFloat(input.value);
  input.disabled = true; btn.disabled = true;

  if(val === q.correct){
    scores[q.key] = q.pts;
    showFeedback(evN, qi, true, `ถูกต้อง! เปลี่ยนไป ${q.correct}${q.unit} → +${q.pts} คะแนน`);
  } else {
    showFeedback(evN, qi, false, `ยังไม่ถูก — คำตอบที่ถูกต้องคือ ${q.correct}${q.unit}`);
  }
  updateRail();
}

/* ---------------- score rail ---------------- */
function currentScore(){
  return Object.values(scores).reduce((a,b)=>a+b, 0);
}
function updateRail(){
  document.getElementById('scoreNow').textContent = currentScore();
  document.getElementById('evNow').textContent = openedCount + '/6';
  document.getElementById('warnCount').textContent = openedCount;
  document.getElementById('warnBanner').classList.toggle('show', openedCount < 6 && !submitted);
}

/* ---------------- report ---------------- */
function maybeUnlockReport(){
  if(openedCount < 6) return;
  document.getElementById('reportLocked').style.display = 'none';
  const form = document.getElementById('reportForm');
  if(form.dataset.built) { form.style.display = 'grid'; return; }
  form.dataset.built = '1';
  reportQuestions.forEach((q,i)=>{
    const row = document.createElement('div');
    row.className = 'report-row';
    row.innerHTML = `<label>${q}</label><textarea placeholder="พิมพ์คำตอบของทีม..."></textarea>`;
    form.appendChild(row);
  });
  form.style.display = 'grid';
}

/* ---------------- accusation ---------------- */
const accuseGrid = document.getElementById('accuseGrid');
suspects.forEach(s=>{
  const b = document.createElement('button');
  b.className = 'accuse-btn';
  b.id = 'accuse_' + s.code;
  b.innerHTML = `<div class="accuse-code">${s.code}</div>${s.name}`;
  b.onclick = () => pickSuspect(s.code);
  accuseGrid.appendChild(b);
});

function pickSuspect(code){
  if(submitted) return;
  pickedSuspect = code;
  suspects.forEach(s => document.getElementById('accuse_'+s.code).classList.remove('picked'));
  document.getElementById('accuse_'+code).classList.add('picked');
  checkSubmitReady();
}

const reasonBox = document.getElementById('reasonBox');
reasonBox.addEventListener('input', checkSubmitReady);
function checkSubmitReady(){
  const ready = pickedSuspect && reasonBox.value.trim().length >= 15;
  document.getElementById('submitBtn').disabled = !ready || submitted;
}

document.getElementById('submitBtn').addEventListener('click', submitAccusation);

function submitAccusation(){
  if(submitted) return;
  submitted = true;

  document.getElementById('submitBtn').disabled = true;
  reasonBox.disabled = true;
  suspects.forEach(s => document.getElementById('accuse_'+s.code).disabled = true);

  const correctPick = pickedSuspect === correctSuspect;
  const wasEarly = openedCount < 6;
  let penalty = 0;
  if(wasEarly && !correctPick) penalty = -3;

  if(correctPick) scores.suspect = 2;

  const text = reasonBox.value.toLowerCase();
  let reasonHits = [];
  reasoningKeywords.forEach(k=>{
    if(k.test(text)){ reasonHits.push(k.label); }
  });
  scores.reasoning = reasonHits.length; // capped at 5 by keyword count

  scores.penalty = penalty;
  updateRail();
  showVerdict(correctPick, reasonHits, penalty, wasEarly);
}

function showVerdict(correctPick, reasonHits, penalty, wasEarly){
  const total = Math.max(0, currentScore());
  document.getElementById('verdictStamp').textContent = correctPick ? 'ไขคดีสำเร็จ' : 'ปิดคดี';
  document.getElementById('verdictTitle').textContent =
    correctPick ? 'ยินดีด้วย — ทีมของคุณระบุตัวถูกต้อง' : 'ผู้ที่เกี่ยวข้องมากที่สุดคือ D — ฟ้า';
  document.getElementById('verdictBody').innerHTML =
    `ภาชนะทำจาก<strong>แก้ว</strong> ซึ่งเดิมมีอุณหภูมิประมาณ 15°C เมื่อฟ้าเทของเหลวอุณหภูมิ 85°C ลงไปอย่างรวดเร็ว อุณหภูมิเปลี่ยนไปประมาณ 70°C ทำให้เนื้อแก้วขยายตัวไม่พร้อมกัน เกิดความเค้นสะสมภายในจนแตกร้าว ปรากฏการณ์นี้เรียกว่า <strong>Thermal shock</strong>`;
  document.getElementById('verdictExplain').textContent =
    'ประเด็นสำคัญของคดีนี้ไม่ใช่แค่ "ฟ้าเทน้ำร้อนจึงแก้วแตก" แต่ต้องอธิบายด้วยหลักวิทยาศาสตร์ว่าเหตุใดการเปลี่ยนอุณหภูมิฉับพลันจึงทำให้วัสดุแตกได้';

  const rows = [
    ['ระบุชนิดวัสดุ (แก้ว)', scores.material, 2],
    ['วิเคราะห์สมบัติของวัสดุ', scores.properties, 3],
    ['วิเคราะห์หลักฐาน (กล้อง + จุดแตก)', scores.ev2 + scores.ev5, 4],
    ['หาสาเหตุ (คำนวณอุณหภูมิ + Thermal shock)', scores.temp + scores.thermal, 4],
    ['ระบุผู้เกี่ยวข้องถูกต้อง', scores.suspect, 2],
    ['อธิบายด้วยหลักวิทยาศาสตร์', scores.reasoning, 5],
  ];
  const tbl = document.getElementById('scoreTable');
  tbl.innerHTML = rows.map(r => `<tr><td>${r[0]}</td><td>${r[1]} / ${r[2]}</td></tr>`).join('');
  if(penalty !== 0){
    tbl.innerHTML += `<tr><td>หักคะแนน (กล่าวหาผิดก่อนเปิดหลักฐานครบ)</td><td>${penalty}</td></tr>`;
  }
  if(reasonHits.length){
    tbl.innerHTML += `<tr><td colspan="2" style="padding-top:10px; color:#6b5d49; font-size:12.5px;">คำอธิบายของทีมพูดถึง: ${reasonHits.join(' · ')}</td></tr>`;
  }
  tbl.innerHTML += `<tr class="score-total"><td>คะแนนรวม</td><td>${total} / 20</td></tr>`;

  document.getElementById('verdictSection').classList.add('show');
  document.getElementById('verdictSection').scrollIntoView({behavior:'smooth', block:'start'});
}

updateRail();
</script>
</body>
</html>
