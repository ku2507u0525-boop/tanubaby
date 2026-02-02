<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Valentine Envelopes</title>

<style>
*{box-sizing:border-box;margin:0;padding:0;font-family:'Segoe UI',sans-serif;}
body{
  overflow:hidden;
  background: #1e90ff; /* Solid blue background */
  cursor: not-allowed;  /* Danger cursor at the very beginning */
  transition: background 1s ease, cursor 0.3s ease;
}
h1{color:#fff;font-size:3em;text-align:center;margin:20px 0;}

/* ================= CAT OVERLAYS ================= */
.cat-overlay{
  position:fixed;
  inset:0;
  background:#1e90ff; /* Blue background for overlays */
  display:flex;
  flex-direction:column;
  justify-content:center;
  align-items:center;
  z-index:100;
  text-align:center;
}
.cat img{
  width:150px;
  height:150px;
  object-fit:contain;
  animation:catBounce 1.5s infinite;
}
@keyframes catBounce{
  0%,100%{transform:translateY(0);}
  50%{transform:translateY(-20px);}
}
.cat-overlay p{
  font-size:26px;
  color:#fff;
  margin-top:10px;
}
.next-btn{
  margin-top:25px;
  padding:12px 30px;
  border:none;
  border-radius:14px;
  background:#ff5c8a;
  color:#fff;
  font-size:18px;
  cursor:pointer;
}

/* ================= LOVE QUESTION OVERLAY ================= */
.love-question-overlay{
  background:#1e90ff; /* Blue background for question overlay */
  display:flex;
  justify-content:center;
  align-items:center;
  position:fixed;
  inset:0;
  z-index:20;
}

/* ================= CONTAINER ================= */
.container{width:90%;max-width:1200px;margin:0 auto;display:none;}

/* ================= ENVELOPES ================= */
.envelope-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:15px;}
.envelope{
  width:100%;
  padding-top:75%;
  position:relative;
  background:#fff;
  border-radius:12px;
  box-shadow:0 5px 20px rgba(0,0,0,0.3);
  cursor:pointer;
  overflow:hidden;
  transition: transform 0.6s ease, z-index 0.3s ease, opacity 0.3s ease;
  transform-origin: center center;
}
.envelope::before{
  content:"";
  position:absolute;
  top:0; left:0;
  width:100%; height:100%;
  background:#e63946;
  clip-path:polygon(50% 0,100% 50%,50% 100%,0 50%);
  transition: all 0.6s ease;
}
.envelope.open::before{
  clip-path:polygon(0 0,100% 0,100% 100%,0 100%);
}
.envelope-content{
  position:absolute;
  top:0; left:0;
  width:100%; height:100%;
  display:flex;
  opacity:0;
  pointer-events:none;
  transition: opacity 0.5s;
}
.envelope.open .envelope-content{
  opacity:1;
  pointer-events:auto;
}
.envelope-left{
  flex:1;
  padding:20px;
  background:rgba(255,255,255,0.95);
  display:flex;
  flex-direction:column;
  align-items:center;
  justify-content:center;
  animation:breathe 3s infinite;
}
.envelope-left h3{font-size:22px;color:#e63946;margin-bottom:12px;text-align:center;}
.envelope-left p{font-size:16px;color:#444;line-height:1.5;text-align:center;}
.envelope-right{flex:1;overflow:hidden;}
.envelope-right img{width:100%;height:100%;object-fit:cover;border-left:1px solid #ccc;}
@keyframes breathe{0%,100%{transform:scale(1);}50%{transform:scale(1.02);}}

/* ================= FULLSCREEN ENVELOPE ================= */
.envelope.fullscreen {
  position: fixed !important;
  top: 0 !important;
  left: 0 !important;
  width: 100vw !important;
  height: 100vh !important;
  padding-top: 0 !important;
  z-index: 500 !important;
  display: flex;
  background: #fff;
  flex-direction: row;
  border-radius: 0 !important;
  transform: none !important;
  opacity: 1 !important;
  transition: all 0.5s ease;
}
.envelope.fullscreen .envelope-content {
  display: flex;
  flex: 1;
}
.envelope.fullscreen .envelope-left,
.envelope.fullscreen .envelope-right {
  flex: 1;
  padding: 40px;
}
.envelope.fullscreen .envelope-right img {
  border-left: none;
  border-radius: 12px;
}

/* ================= FLOATING EFFECTS ================= */
.heart,.sparkle,.particle{position:absolute;pointer-events:none;z-index:200;}
.heart,.sparkle{animation:floatUp linear infinite;opacity:0.8;}
@keyframes floatUp{from{transform:translateY(0);}to{transform:translateY(-120vh);opacity:0;}}
.particle{width:6px;height:6px;background:rgba(255,255,255,0.7);border-radius:50%;
animation:drift 10s linear infinite;}
@keyframes drift{from{transform:translateY(0);}to{transform:translateY(-100vh);opacity:0;}}
</style>
</head>

<body>

<!-- INTRO IMAGE -->
<div class="cat-overlay" id="introCat">
  <div class="cat">
    <img src="C:\Users\Darshil\OneDrive\Pictures/meow.gif">
  </div>
  <p>Hey cutie… 💕</p>
  <button class="next-btn" onclick="showLoveOverlay()">Next ➡️</button>
</div>

<!-- YES VIDEO -->
<div class="cat-overlay" id="yesCat" style="display:none;">
  <div class="cat">
    <video src="C:\Users\Darshil\OneDrive\Pictures/caat.mp4" autoplay loop muted playsinline
           style="width:150px; height:150px; object-fit:contain; animation:catBounce 1.5s infinite;">
    </video>
  </div>
  <p>Yay! I KNEW IT...💖</p>
  <button class="next-btn" onclick="showMain()">Next ➡️</button>
</div>

<!-- VALENTINE QUESTION -->
<div class="love-question-overlay" id="loveOverlay" style="display:none;">
  <div style="background:#fff;padding:30px 40px;border-radius:20px;text-align:center;">
    <h2 style="color:#e63946;margin-bottom:20px;">Will you be my Valentine? ❤️</h2>
    <button onclick="showYesCat()" style="padding:10px 22px;border:none;border-radius:12px;background:#ff5c8a;color:#fff;font-size:16px;margin-right:10px;">Yes 💖</button>
    <button onclick="noLove()" style="padding:10px 22px;border:none;border-radius:12px;background:#ccc;color:#fff;font-size:16px;cursor:not-allowed;">No 😢</button>
  </div>
</div>

<!-- MAIN CONTENT -->
<div class="container">
<h1>Happy Valentine's Day</h1>

<div class="envelope-grid">
  <div class="envelope" onclick="openEnvelope(this)">
    <div class="envelope-content">
      <div class="envelope-left"><h3>Forever Starts Here</h3><p>Forever starts here with you 🎀, yes baby you bcz of you I have become this good and came this far only bcz of you ♥️, with us💞, with all the little moments that turned into something beautiful or romentic or even more romantic 😘😏. From shared smiles to silent understanding, every step we’ve taken together has led to this love that feels safe, deep, and endlessly real. On this Valentine’s Day I know baby I am not there with you but I am doing everything I can do for u and I will do in future too 👾

This isn’t just a day it’s a promise from me to you that I will be with you everytime even when sad days or any days . A promise to choose you in every season, to grow together, to laugh harder, and to hold on even tighter when life gets tough. If love is a journey, then forever starts right here hand in hand, heart to heart, with you by my side ❤️🎀.</p></div>
      <div class="envelope-right"><img src="C:\Users\Darshil\OneDrive\Pictures/first.jpeg"></div>
    </div>
  </div>

  <div class="envelope" onclick="openEnvelope(this)">
    <div class="envelope-content">
      <div class="envelope-left"><h3>Be Mine</h3><p>Be Mine not just for today, but for every sunrise and every quiet night in between. I still picture you so clearly , your curly brown hair that I wish I could play with, your warm brown eyes that feel like home even through a screen, and that confident, beautiful presence that drives me crazy in the best way💋💋. It’s been 1.5 years of loving you, learning you, choosing you every single day, and somehow my feelings haven’t settled they’ve grown deeper, stronger, and more sure💘. You’re not just my girlfriend; you’re my comfort, my excitement, and my favorite person to talk too and even my bestfriend and the reason which motivates me to work even harder ❤️‍🩹❤️‍🩹.


Being in a long-distance relationship isn’t easy, but loving you makes every mile worth it. Even when I can’t hold your hand, I feel connected to you in ways distance can’t touch✨. We’ve built something real even after going through that April phase on trust, patience, late-night talks, and dreams of finally being together without a countdown👩🏼‍❤️‍💋‍👨🏼. So today and always, I want you to Be Mine,to keep growing with me, to keep believing in us, and to turn this distance into just a chapter of a forever story we’ll one day laugh about💖😭.</p></div>
      <div class="envelope-right"><img src="C:\Users\Darshil\OneDrive\Pictures/second.jpeg"></div>
    </div>
  </div>

  <div class="envelope" onclick="openEnvelope(this)">
    <div class="envelope-content">
      <div class="envelope-left"><h3>Our First Kiss</h3><p>Our first kiss is a moment my heart goes back to again and again🤤, like a favorite memory it never gets tired of replaying🙃. I remember how time felt slower, how everything else faded until it was just you and me🥹, standing close with a thousand unspoken feelings between us. My heart was racing, my hands unsure, but the second our lips met💦, it felt calm like something that was always meant to happen finally did😭😭😭😭😭😭😭. That kiss wasn’t just soft or sweet; it was full of all the emotions we hadn’t yet learned how to say out loud🥹❤️‍🩹. On our first kiss , and it's obvious buz u are my first and last gf soo I got a bonner as it was not going bcz the moment was replaying in my mind constantly 👾🥰♥️.

That first kiss became more than a moment it became a promise that we are now unbreakable no one can break us apart even after that April incident we still manage to get through it and see now we are here at this point celebrating valantines day😭😭❤️‍🩹❤️‍🩹. A promise of love, of connection, of choosing each other even when things aren’t easy. Whenever distance feels heavy, I think about that kiss and it reminds me how real we are, how strong we are, and how deeply I’m yours👩🏼‍❤️‍💋‍👨🏼👩🏼‍❤️‍💋‍👨🏼👩🏼‍❤️‍💋‍👨🏼👩🏼‍❤️‍💋‍👨🏼. No matter how far apart we are right now, that first kiss still lives with me, warming my heart and reminding me that every wait is worth it because it’s always you💕 and only u , no one can take the place that u have in my heart and I don't want anyone to take this place in my heart bcz my heart belongs to you mylove tanubaccha❤️‍🩹😭💕.</p></div>
      <div class="envelope-right"><img src="C:\Users\Darshil\OneDrive\Pictures/third.jpeg"></div>
    </div>
  </div>

  <div class="envelope" onclick="openEnvelope(this)">
    <div class="envelope-content">
      <div class="envelope-left"><h3>Us Against The World</h3><p>Us against the world isn’t just a line for me it’s the truth of what we’ve lived and still living but now living freely. April tested us in ways I never imagined, and seeing you go through so much because of us broke my heart. Being caught, facing strict parents, the pressure, the fear, the emotional torture ,you carried it all with so much strength, even when it hurt deeply😭(tanu baby I am crying writing this para 😭😭). I know there were moments when everything felt unfair and heavy, but through it all, you never stopped being brave. I want you to know how proud I am of you🫂, and how deeply I respect the courage it took to survive that phase🫂. (I miss you now baby soo muchh😭😭😭). {Also you know a last screen short chhe april no ena pachi pari e kai didhu tu gare soo idk i am soo imotional rn idk what i have writen in this para , its pure imotion so please chalai leje}

What we have is real, and that’s why it felt threatening to the world around us💞. Even in silence, distance, and restrictions, our bond didn’t break 😭it grew stronger😭 like an old tree's roots🌳🌲. If the world stands against us, then I’ll stand beside you even firmer, even louder in my heart. You’re not alone in this, not now, not ever. We’ve already fought battles together, and that’s proof enough for me: no matter how hard it gets, it will always be you and me and even your friends betrayed you I am still with you baby and REMEMBER ITS US AGAINST THE WORLD ❤️👾♥️❤️‍🩹. ILOVEYOUUUUTANUUBABYYY💋🎀👾.</p></div>
      <div class="envelope-right"><img src="C:\Users\Darshil\OneDrive\Pictures/fourth.jpeg"></div>
    </div>
  </div>

  <div class="envelope" onclick="openEnvelope(this)">
    <div class="envelope-content">
      <div class="envelope-left"><h3>Navratri Nights</h3><p>Navratri nights with you felt like a beautiful routine I never wanted to end😭😭. But the main part was to find the ticket where we want to go it was soo much chaos bcz of the shortage of the people we had and omg the happiness after we got the tickets I can't define it in words I am speechless At this point🥹❤️‍🩹♥️💋.Every evening began with us deciding the color of the day, sending each other mirror selfies, and smiling at how perfectly we matched like we were married couples 🤭🤭. Walking into the garba grounds side by side, surrounded by lights, music, and chaos, yet feeling completely locked into our own little world👉🏻👈🏻. After the garba and the noise, sitting in the car felt like our pause button , soft conversations, tired laughs, quiet closeness, and moments where words weren’t even needed because being together said enough on the backseat of the car🙃🤤❤️‍🩹👩🏼‍❤️‍💋‍👨🏼. I miss those makeout we used to do in the cab without letting the cab driver know what are we doing 🤤🙃😈.

And then came our late-night drives to Urban Chowk(the best part where u feed me with your soft soft hands🤭🤭)the hunger, the excitement, the comfort of knowing the night wasn’t over yet. Sharing food, teasing each other, talking about random things, sometimes deep, sometimes silly, with the city lights glowing around us✨🥹🙃. Those slow drives back, windows down, music low, hearts full—those were the moments that stayed with me the most and I still miss those moments 😭❤️‍🩹. Navratri with you wasn’t just about colors or festivals; it was about feeling chosen every night, about us creating memories in between dances, dinners, and quiet car rides that I’ll always carry with me👾🎀 miss those days babe please thai to i jaje navratri ma🥺💋👉🏻👈🏻..</p></div>
      <div class="envelope-right"><img src="C:\Users\Darshil\OneDrive\Pictures/fifth.jpeg"></div>
    </div>
  </div>

  <div class="envelope" onclick="openEnvelope(this)">
    <div class="envelope-content">
      <div class="envelope-left"><h3>Forever Together</h3><p>Forever together those words carry the weight of everything we’ve survived🧿. Loving you was never the easy road I don't mean that loving u is hard or smtg like that it's just bcz of me u were living in fear of your small sister and been tortured 😭 sorry my love 😭, but it was always the right one❤️‍🩹. We faced strict parents, constant fear, hidden tears, and moments where the pressure felt unbearable😭😭😭. Even when we lived just 30 minutes apart, it felt like the world was trying to pull us away from each other and that April month was the test for both of us that we passed and see now we are here together 🧿🫂. Yet every struggle only proved one thing more clearly: what we have is TURU LOB. We didn’t give up when it hurt, and that strength lives in our love💋💋💋.

Now life has taken you all the way to Georgia to chase your dream of becoming a doctor👩🏻‍⚕️, and our story has turned into a true long-distance love which we will make through 💋❤️‍🩹. The distance is bigger, the time zones are different, and some days are painfully quiet—but my heart still knows exactly where it belongs(to you ofc my lovee🙃✨). Every call, every text, every “take care” feels like a reminder that love doesn’t need proximity to stay alive👉🏻👈🏻👾. I’m so proud of you, of your courage, your sacrifices, and the way you keep moving forward even when it’s hard and it's like a dead end😭🧿👾💋💋❤️‍🩹. And also i remember those small small makeouts that we used to do in those 5 min meetup and those movie makeouts and also navratri cab also🤭🤭💞🥰 ImissyoumyloveTanubabu💋❤️‍🩹.

This Valentine’s Day, I want you to know that I’m not going anywhere and I mean it I am not going anywhere soo don't worry about me u focus on your studies my love💋❤️‍🩹❤️‍🩹🧿. No distance, no rules, no obstacles can undo what we’ve built with patience, trust, and endless love and after all this year I don't think soo I need to tell you this that I have allready married u in my mind and u are stuck with me🥰🤭🧿. You are my safe place, my strength, my forever. One day this distance will close and we will marry like we always wanted to 😭😭🧿, and all the waiting will make sense. Until then, it’s you and me across borders(in bed😈 too), across time, against everything. Forever together and always ❤️🧿❤️‍🩹.</p></div>
      <div class="envelope-right"><img src="C:\Users\Darshil\OneDrive\Pictures/sisth.jpeg"></div>
    </div>
  </div>
</div>
</div>

<script>
const envelopes = document.querySelectorAll('.envelope');

// Intro / overlay functions
function showLoveOverlay() {
  document.getElementById('introCat').style.display = 'none';
  document.getElementById('loveOverlay').style.display = 'flex';
  document.body.style.cursor = 'default';
}

function showYesCat() {
  document.getElementById('loveOverlay').style.display = 'none';
  document.getElementById('yesCat').style.display = 'flex';
  document.body.style.cursor = 'default';
}

function showMain() {
  document.getElementById('yesCat').style.display = 'none';
  document.querySelector('.container').style.display = 'block';
  document.body.style.cursor = 'default';
}

// No button action
function noLove() {
  alert("Oops! You can't click No 😅,You stuck here HEHEHHEHHEH");
}

// Envelope functions
function openEnvelope(envelope) {
  envelopes.forEach(e => { if (e !== envelope) e.style.opacity = '0'; });
  envelope.classList.add('open', 'fullscreen');

  // Add back button
  let backBtn = document.createElement('button');
  backBtn.innerText = '⬅ Back';
  backBtn.style.position = 'fixed';
  backBtn.style.top = '20px';
  backBtn.style.right = '20px';
  backBtn.style.padding = '10px 20px';
  backBtn.style.fontSize = '16px';
  backBtn.style.border = 'none';
  backBtn.style.borderRadius = '12px';
  backBtn.style.background = '#ff5c8a';
  backBtn.style.color = '#fff';
  backBtn.style.cursor = 'pointer';
  backBtn.id = 'backBtn';
  backBtn.onclick = () => closeEnvelope(envelope);
  document.body.appendChild(backBtn);
}

function closeEnvelope(envelope) {
  envelope.classList.remove('open', 'fullscreen');
  envelopes.forEach(e => e.style.opacity = '1');
  const btn = document.getElementById('backBtn');
  if (btn) btn.remove();
}

// Escape key closes envelope
document.addEventListener('keydown', e => {
  if (e.key === 'Escape') {
    envelopes.forEach(e => e.classList.remove('open', 'fullscreen'));
    envelopes.forEach(e => e.style.opacity = '1');
    const btn = document.getElementById('backBtn');
    if (btn) btn.remove();
  }
});

// Floating hearts
setInterval(() => {
  const h = document.createElement('div');
  h.className = 'heart';
  h.innerHTML = '❤️';
  h.style.left = Math.random() * 100 + 'vw';
  h.style.fontSize = (16 + Math.random() * 20) + 'px';
  h.style.animationDuration = (6 + Math.random() * 4) + 's';
  document.body.appendChild(h);
  setTimeout(() => h.remove(), 9000);
}, 600);
</script>

</body>
</html>
