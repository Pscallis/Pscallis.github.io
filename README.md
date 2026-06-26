<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Hey, Nun.</title>
  <link rel="preconnect" href="https://fonts.googleapis.com"/>
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin/>
  <link href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,300;0,400;0,500;1,300;1,400;1,500&family=Lora:ital,wght@0,400;0,500;1,400&display=swap" rel="stylesheet"/>

  <style>
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

    :root {
      --bg:       #0C0E1A;
      --surface:  #10121F;
      --text:     #EDE8DF;
      --muted:    #8A8FA8;
      --gold:     #D4A96A;
      --rose:     #C4A0A8;
      --fade:     rgba(212, 169, 106, 0.12);
    }

    html {
      scroll-behavior: smooth;
    }

    body {
      background: var(--bg);
      color: var(--text);
      font-family: 'Lora', Georgia, serif;
      min-height: 100vh;
      overflow-x: hidden;
    }

    /* ── Canvas stars ── */
    #stars {
      position: fixed;
      inset: 0;
      z-index: 0;
      pointer-events: none;
    }

    /* ── Hero ── */
    .hero {
      position: relative;
      z-index: 1;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      height: 100svh;
      text-align: center;
      padding: 2rem;
    }

    .hero-eyebrow {
      font-family: 'Lora', serif;
      font-style: italic;
      font-size: clamp(0.75rem, 2vw, 0.85rem);
      letter-spacing: 0.25em;
      color: var(--gold);
      opacity: 0.7;
      margin-bottom: 1.4rem;
      text-transform: uppercase;
    }

    .hero-name {
      font-family: 'Cormorant Garamond', serif;
      font-weight: 300;
      font-style: italic;
      font-size: clamp(4.5rem, 13vw, 9rem);
      line-height: 1;
      color: var(--text);
      letter-spacing: -0.02em;
    }

    .hero-name span {
      color: var(--gold);
    }

    .hero-sub {
      margin-top: 2rem;
      font-family: 'Lora', serif;
      font-style: italic;
      font-size: clamp(0.85rem, 2.2vw, 1rem);
      color: var(--muted);
      letter-spacing: 0.04em;
      max-width: 28ch;
      line-height: 1.7;
    }

    .scroll-cue {
      position: absolute;
      bottom: 2.5rem;
      left: 50%;
      transform: translateX(-50%);
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 0.5rem;
      color: var(--muted);
      font-size: 0.7rem;
      letter-spacing: 0.18em;
      text-transform: uppercase;
      animation: cue-bob 2.4s ease-in-out infinite;
    }

    .scroll-cue::after {
      content: '';
      display: block;
      width: 1px;
      height: 36px;
      background: linear-gradient(to bottom, var(--gold), transparent);
    }

    @keyframes cue-bob {
      0%, 100% { opacity: 0.35; transform: translateX(-50%) translateY(0); }
      50%       { opacity: 0.65; transform: translateX(-50%) translateY(6px); }
    }

    /* ── Letter wrapper ── */
    .letter-wrap {
      position: relative;
      z-index: 1;
      max-width: 680px;
      margin: 0 auto;
      padding: 4rem 2rem 8rem;
    }

    /* Top ornamental rule */
    .letter-rule {
      display: flex;
      align-items: center;
      gap: 1rem;
      margin-bottom: 4rem;
      opacity: 0.3;
    }

    .letter-rule::before,
    .letter-rule::after {
      content: '';
      flex: 1;
      height: 1px;
      background: linear-gradient(to right, transparent, var(--gold));
    }
    .letter-rule::after {
      background: linear-gradient(to left, transparent, var(--gold));
    }
    .letter-rule-dot {
      width: 5px; height: 5px;
      border-radius: 50%;
      background: var(--gold);
    }

    /* ── Paragraphs ── */
    .para {
      font-size: clamp(1.02rem, 2.5vw, 1.13rem);
      line-height: 1.95;
      color: var(--text);
      font-weight: 400;
      margin-bottom: 2.4rem;

      opacity: 0;
      transform: translateY(22px);
      transition: opacity 0.8s ease, transform 0.8s ease;
    }

    .para.visible {
      opacity: 1;
      transform: translateY(0);
    }

    .para em {
      color: var(--gold);
      font-style: italic;
    }

    /* Opening salutation */
    .salutation {
      font-family: 'Cormorant Garamond', serif;
      font-weight: 300;
      font-style: italic;
      font-size: clamp(2.4rem, 6vw, 3.4rem);
      color: var(--text);
      line-height: 1.1;
      margin-bottom: 3rem;
      letter-spacing: -0.01em;

      opacity: 0;
      transform: translateY(22px);
      transition: opacity 0.9s ease, transform 0.9s ease;
    }

    .salutation.visible {
      opacity: 1;
      transform: translateY(0);
    }

    /* Pull-quote / heavy moment */
    .pull {
      border-left: 2px solid var(--gold);
      padding: 0.6rem 0 0.6rem 1.6rem;
      margin: 3.2rem 0;
      font-family: 'Cormorant Garamond', serif;
      font-style: italic;
      font-weight: 400;
      font-size: clamp(1.2rem, 3vw, 1.45rem);
      line-height: 1.6;
      color: var(--rose);

      opacity: 0;
      transform: translateY(22px);
      transition: opacity 0.9s ease, transform 0.9s ease;
    }

    .pull.visible {
      opacity: 1;
      transform: translateY(0);
    }

    /* Memory list — small moments */
    .memories {
      list-style: none;
      margin: 0.4rem 0 2.4rem 0;
      display: flex;
      flex-direction: column;
      gap: 0.65rem;

      opacity: 0;
      transform: translateY(22px);
      transition: opacity 0.9s ease, transform 0.9s ease;
    }

    .memories.visible {
      opacity: 1;
      transform: translateY(0);
    }

    .memories li {
      display: flex;
      align-items: baseline;
      gap: 0.9rem;
      font-size: clamp(0.95rem, 2.2vw, 1.05rem);
      line-height: 1.7;
      color: #C8C3BC;
    }

    .memories li::before {
      content: '·';
      color: var(--gold);
      font-size: 1.3em;
      flex-shrink: 0;
      line-height: 1;
    }

    /* Closing */
    .closing {
      margin-top: 4rem;
      text-align: center;
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 0.8rem;

      opacity: 0;
      transform: translateY(22px);
      transition: opacity 1s ease, transform 1s ease;
    }

    .closing.visible {
      opacity: 1;
      transform: translateY(0);
    }

    .closing-line {
      font-family: 'Cormorant Garamond', serif;
      font-style: italic;
      font-size: clamp(1.5rem, 4vw, 2rem);
      color: var(--muted);
      line-height: 1.4;
      font-weight: 300;
    }

    .heart {
      font-size: 1.6rem;
      color: var(--rose);
      margin-top: 0.4rem;
      animation: heart-pulse 3.5s ease-in-out infinite;
    }

    @keyframes heart-pulse {
      0%, 100% { opacity: 0.6; transform: scale(1); }
      50%       { opacity: 1; transform: scale(1.08); }
    }

    .postscript {
      margin-top: 3rem;
      font-size: 0.88rem;
      font-style: italic;
      color: var(--muted);
      text-align: center;
      line-height: 1.8;
      opacity: 0;
      transition: opacity 1.2s ease;
    }

    .postscript.visible {
      opacity: 0.55;
    }

    /* Bottom ornament */
    .letter-end {
      margin-top: 5rem;
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 1.2rem;
      opacity: 0.18;
    }

    .letter-end span {
      width: 48px; height: 1px;
      background: var(--gold);
    }

    .letter-end-star {
      color: var(--gold);
      font-size: 0.6rem;
    }

    /* ── Reduced motion ── */
    @media (prefers-reduced-motion: reduce) {
      .para, .pull, .memories, .closing, .postscript, .salutation {
        transition: none;
        opacity: 1;
        transform: none;
      }
      .scroll-cue { animation: none; }
      .heart { animation: none; }
    }

    /* ── Mobile ── */
    @media (max-width: 520px) {
      .letter-wrap { padding: 3rem 1.4rem 6rem; }
      .para { line-height: 1.88; }
    }
  </style>
</head>
<body>

<canvas id="stars"></canvas>

<!-- Hero -->
<section class="hero">
  <p class="hero-eyebrow">a letter</p>
  <h1 class="hero-name">Hey, <span>Nun.</span></h1>
  <p class="hero-sub">Some things don't arrive loudly. They settle in slowly, quietly.</p>
  <div class="scroll-cue">read</div>
</section>

<!-- Letter -->
<main class="letter-wrap">

  <div class="letter-rule"><div class="letter-rule-dot"></div></div>

  <p class="salutation">Hey, Nun.</p>

  <p class="para">
    Some things don't arrive loudly. They settle in slowly, quietly, like something that was always meant to be felt. Not rushed.
  </p>

  <p class="para">
    It started with a conversation. One that stretched longer than either of us planned, like neither of us wanted to be the first to leave. And somewhere in the middle of all those words, I found something I didn't expect. Someone walking the same quiet road, carrying the same kind of dreams, moving in the same direction. So we walked together. No label, no promise, no expiration date. Just two people who, without saying it out loud, chose each other. That meant everything to me. More than I ever found the words for.
  </p>

  <p class="para">
    And I remember the small things more than anything. Coffee together, just the two of us and whatever we had to say that day. The drive to Semarang to meet your best friend, like I was slowly becoming part of your world. Grocery stores that felt oddly like home. Pottery, hands in clay, laughing at something I can't even remember now but still feel. And then your birthday, a gift, a smile, and me carrying everything else home in silence.
  </p>

  <p class="pull">"Those weren't just moments. They were mine. And I'd keep every single one."</p>

  <p class="para">
    They say confessing feelings is brave. Maybe it is. It costs something to open your chest and let someone see what's inside. But I've come to learn that letting go of someone you still deeply feel for, that is a different kind of courage altogether. Quieter. Heavier. The kind no one sees.
  </p>

  <p class="para">
    You said it once, clearly. If this doesn't go well, there are no second chances. And we stay as friends, no matter what. I heard you. I accepted it. I held on to that too, maybe even tighter than I held on to the hope. Because I'd rather have you in my life as a friend than lose you entirely. I told myself that was enough. For a while, I believed it.
  </p>

  <p class="para">
    But somewhere along the way, the space we built together began to fade. What was meant to be a place to breathe, to grow, to one day meet in the middle, slowly became a place only I kept coming back to. The feelings were mine. The hope was mine. And I stayed longer than I should have, not out of stubbornness, but because what we had felt too real to just walk away from. Even without a name, even without a promise, even without certainty.
  </p>

  <p class="para">
    On your birthday, I had all of this sitting heavy in my chest, ready to finally be said. But I looked at you, and I couldn't. Not then. Because you deserved to just exist in that moment, unburdened. So I gave you a gift. I gave you the day. And I carried the rest home with me, alone.
  </p>

  <p class="para">
    I don't regret any of it. Not the feelings, not the hope, not the long wait, not even the silence.
  </p>

  <p class="pull">"You were real to me. What we had was real to me. And I would never take that back."</p>

  <p class="para">
    But I need to be honest, even if it's only to myself. I can't even bring myself to call this love. That word feels both too big and too true at the same time. But whatever it was, it lived in me deeply. And I think that's exactly why leaving is the only honest thing left to do.
  </p>

  <p class="para">
    Because this isn't me saying the feelings are gone. It's the opposite. I'm stepping away because I feel too much. Too deeply. Because caring for someone the way I cared for you, quietly and without asking for anything in return, starts to hollow you out when it only ever flows one way.
  </p>

  <p class="para">
    I'm not disappearing. I'm not bitter. I'm not broken. I'm just dissolving into it, into all of it, and learning, slowly, to abide with what this is. And what it isn't. And what it never got the chance to become.
  </p>

  <p class="para">
    I hope the road ahead is kind to you. I hope you find whatever it is your heart is quietly looking for. I hope you rest well, grow well, live well.
  </p>

  <div class="closing">
    <p class="closing-line">Thank you for being a part of my little world.</p>
    <p class="closing-line">Thank you for being Mia for me <em style="font-size:0.85em; color: var(--muted);">(La La Land?)</em></p>
    <p class="closing-line" style="font-size: 1em; color: var(--muted);">
      And if the crochet is too hard, at least build the brick puppy for me.
    </p>
    <div class="heart">🤍</div>
  </div>

  <p class="postscript">
    Take care of yourself.
  </p>

  <div class="letter-end">
    <span></span>
    <span class="letter-end-star">✦</span>
    <span></span>
  </div>

</main>

<script>
  /* ── Ambient particle field ── */
  const canvas = document.getElementById('stars');
  const ctx = canvas.getContext('2d');
  let W, H, particles;

  function resize() {
    W = canvas.width  = window.innerWidth;
    H = canvas.height = window.innerHeight;
  }

  function mkParticle() {
    const isGold = Math.random() < 0.18;
    return {
      x: Math.random() * W,
      y: Math.random() * H,
      r: Math.random() * 1.1 + 0.2,
      alpha: Math.random() * 0.45 + 0.08,
      alphaDir: (Math.random() < 0.5 ? 1 : -1) * (Math.random() * 0.003 + 0.001),
      dx: (Math.random() - 0.5) * 0.12,
      dy: (Math.random() - 0.5) * 0.08,
      color: isGold ? '212,169,106' : '180,185,210',
    };
  }

  function init() {
    resize();
    const count = Math.min(Math.floor((W * H) / 6200), 200);
    particles = Array.from({ length: count }, mkParticle);
  }

  function draw() {
    ctx.clearRect(0, 0, W, H);
    for (const p of particles) {
      p.x += p.dx;
      p.y += p.dy;
      p.alpha += p.alphaDir;

      if (p.alpha <= 0.04)  { p.alphaDir =  Math.abs(p.alphaDir); }
      if (p.alpha >= 0.55)  { p.alphaDir = -Math.abs(p.alphaDir); }
      if (p.x < -4) p.x = W + 4;
      if (p.x > W + 4) p.x = -4;
      if (p.y < -4) p.y = H + 4;
      if (p.y > H + 4) p.y = -4;

      ctx.beginPath();
      ctx.arc(p.x, p.y, p.r, 0, Math.PI * 2);
      ctx.fillStyle = `rgba(${p.color},${p.alpha.toFixed(3)})`;
      ctx.fill();
    }
    requestAnimationFrame(draw);
  }

  init();
  draw();
  window.addEventListener('resize', () => { init(); });

  /* ── Scroll-reveal ── */
  const revealEls = document.querySelectorAll(
    '.para, .pull, .memories, .closing, .postscript, .salutation'
  );

  const observer = new IntersectionObserver((entries) => {
    entries.forEach(e => {
      if (e.isIntersecting) {
        e.target.classList.add('visible');
        observer.unobserve(e.target);
      }
    });
  }, { threshold: 0.12 });

  revealEls.forEach(el => observer.observe(el));
</script>

</body>
</html>
