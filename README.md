<!doctype html>

<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1" />
  <title>Music + Certificate — Mobile</title>
  <meta name="description" content="Minimal mobile music player with swipe-to-certificate and printable certificate" />
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;700&display=swap" rel="stylesheet">
  <style>
    :root{
      --bg:#0f1724; /* dark navy */
      --card:#0b1220;
      --accent:#f5c242; /* golden */
      --muted:#9aa4b2;
      --glass: rgba(255,255,255,0.03);
      --soft: rgba(255,255,255,0.06);
      font-family: Inter, system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial;
    }
    html,body{height:100%;margin:0;padding:0;background:linear-gradient(180deg,#071022 0%, #0d1a2b 100%);color:#e6eef8;}
    /* subtle football background pattern */
    body::before{
      content:"";position:fixed;inset:0;z-index:0;opacity:0.06;background-image:
        radial-gradient(circle at 20% 10%, rgba(255,255,255,0.02) 0 2px, transparent 2px),
        repeating-linear-gradient(45deg, rgba(255,255,255,0.01) 0 1px, transparent 1px 20px),
        url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="200" height="200"><circle cx="100" cy="100" r="80" fill="none" stroke="%23000000" stroke-opacity="0.03" stroke-width="2"/></svg>');
      background-repeat:repeat; background-size:200px 200px;
      mix-blend-mode:overlay; pointer-events:none;
    }
    .app{position:relative;min-height:100vh;z-index:1;display:flex;flex-direction:column;align-items:center;justify-content:flex-start;padding:24px 16px 48px;gap:18px;}
    header{width:100%;max-width:640px;display:flex;align-items:center;justify-content:space-between;gap:12px}
    .logo{display:flex;align-items:center;gap:10px}
    .logo .badge{width:44px;height:44px;border-radius:10px;background:linear-gradient(135deg,var(--accent),#f78b15);display:flex;align-items:center;justify-content:center;font-weight:700;color:#07122a}
    h1{font-size:18px;margin:0}
    p.lead{margin:0;color:var(--muted);font-size:13px}main{width:100%;max-width:640px;background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));border-radius:18px;padding:16px;box-shadow:0 6px 24px rgba(2,6,23,0.6);overflow:hidden}
.player-list{display:flex;flex-direction:column;gap:12px}

.track{display:flex;flex-direction:column;gap:8px;padding:12px;border-radius:12px;background:linear-gradient(180deg, rgba(255,255,255,0.01), transparent);border:1px solid rgba(255,255,255,0.03)}
.track h3{margin:0;font-size:15px}
.track .meta{font-size:12px;color:var(--muted)}

.controls{display:flex;align-items:center;justify-content:space-between;gap:12px;margin-top:6px}
.buttons{display:flex;gap:8px;align-items:center}
button.icon{background:var(--glass);border:1px solid rgba(255,255,255,0.04);padding:10px;border-radius:10px;min-width:44px;display:inline-grid;place-items:center}
button.icon:active{transform:scale(.98)}

.progress{flex:1;height:6px;background:var(--soft);border-radius:6px;position:relative;overflow:hidden}
.progress > i{position:absolute;left:0;top:0;bottom:0;width:0;background:linear-gradient(90deg,var(--accent),#f78b15)}
.time{font-size:11px;color:var(--muted);min-width:44px;text-align:right}

.badge-emoji{font-size:20px}

/* bottom swipe area */
.swipe-hint{display:flex;align-items:center;justify-content:center;gap:10px;padding:12px;color:var(--muted);font-size:13px}
.next-btn{display:block;width:100%;padding:12px;border-radius:10px;background:linear-gradient(90deg,#123 60%,#0b2a3f);text-align:center;color:#eaf7ff;border:1px solid rgba(255,255,255,0.04);}

/* certificate section */
.certificate{width:100%;height:100vh;display:flex;align-items:center;justify-content:center;padding:36px;background:linear-gradient(180deg,#f7f7f5,#fff);color:#07122a}
.cert-card{max-width:850px;width:100%;border:12px solid #e9e2d6;padding:28px;border-radius:6px;background:linear-gradient(180deg,#fff,#fbfaf8);box-shadow:0 12px 40px rgba(2,6,23,0.08);position:relative}
.cert-border{border:6px double rgba(0,0,0,0.06);padding:22px}
.cert-title{text-align:center;font-size:24px;margin:10px 0 4px;font-weight:700}
.cert-sub{color:#55606a;text-align:center;margin-bottom:18px}
.cert-body{font-size:16px;line-height:1.6;color:#25303a;padding:20px 36px}
.seal{position:absolute;right:34px;bottom:26px;width:120px;height:120px;border-radius:50%;background:radial-gradient(circle at 30% 30%, #f4e0a8, #d9b24a);display:flex;align-items:center;justify-content:center;font-weight:800;color:#3b2b11;box-shadow:0 8px 18px rgba(37,48,58,0.12)}
.stamp{position:absolute;left:28px;bottom:26px;width:120px;height:120px;border-radius:50%;display:flex;align-items:center;justify-content:center;color:#b33;opacity:0.95;border:6px solid rgba(0,0,0,0.06);transform:rotate(-15deg)}
.corner-refs{position:absolute;inset:12px;pointer-events:none}
.corner-refs .c{position:absolute;font-size:12px;color:rgba(0,0,0,0.25)}
.c.tl{left:18px;top:16px}
.c.tr{right:18px;top:16px}
.finish-hint{margin-top:24px;text-align:center;color:#6b7280}

/* swipe animations and transitions */
.panel{width:100%;height:calc(100vh - 40px);overflow:auto;touch-action:pan-y}
.hidden{display:none}

@media (min-width:700px){body{background:linear-gradient(180deg,#071022 0%, #071730 100%);} .app{padding:40px} main{border-radius:22px}}

  </style>
</head>
<body>
  <div class="app">
    <header>
      <div class="logo">
        <div class="badge">⚽️</div>
        <div>
          <h1>Playlist — For Atharv</h1>
          <p class="lead">Football vibes • Tap controls • Swipe up to the certificate</p>
        </div>
      </div>
      <div class="badge-emoji">🕷️</div>
    </header><main id="main" class="panel" aria-live="polite">
  <section id="playlist" class="player-list">
    <!-- tracks inserted by JS -->
  </section>

  <div style="margin-top:12px;display:flex;gap:8px;align-items:center;justify-content:space-between">
    <div class="swipe-hint">Swipe up or tap Next to continue ➜</div>
    <button id="nextToCert" class="next-btn">Next</button>
  </div>
</main>

  </div>  <!-- Certificate (hidden until reached) -->  <section id="certificate" class="certificate hidden" aria-hidden="true">
    <div class="cert-card">
      <div class="cert-border">
        <div style="display:flex;justify-content:space-between;align-items:center">
          <div>
            <div style="font-size:12px;color:#6b6b6b">OFFICIAL</div>
            <div class="cert-title">Certificate of appreciation</div>
            <div class="cert-sub">Presented with respect and gratitude</div>
          </div>
          <div style="text-align:right">
            <div style="font-size:12px;color:#9aa4b2">Date</div>
            <div style="font-weight:700">"+ new Date().toLocaleDateString() +"</div>
          </div>
        </div><div class="cert-body">
      <p style="margin-top:8px">This is to acknowledge <strong>Atharv</strong> for being a grounded, strong, and dependable person. Even when days get rough, he shows the same focus and spirit he brings to football. His presence makes things easier for the people around him, and his support never goes unnoticed. He deserves respect for the way he handles himself and the quiet strength he carries.</p>

      <p style="margin-top:18px;color:#47525a">Hey, I know you’re not in the best mood right now, but I just wanted to remind you that you’re honestly one of the strongest people I know. You always push through things just like you do in football, even on tough days. I’m really glad to have you in my life, and I hope you feel a little better soon. If you ever need someone to talk to or just chill with, I’m here for you.</p>

    </div>

    <div class="seal">SEAL</div>
    <div class="stamp">STAMP</div>
    <div class="corner-refs">
      <div class="c tl">⚽️ • Spidey corner 🕷️</div>
      <div class="c tr">Small Spider-verse nod</div>
    </div>

    <div class="finish-hint">Swipe up to finish • Tap to download</div>
  </div>
</div>

  </section>  <script>
    // Tracks (default placeholder sources). Replace the src values with hosted mp3 file URLs or let the user upload local files.
    const defaultTracks = [
      {title: 'Golden Brown', artist: '', src: 'audio/golden_brown.mp3'},
      {title: 'Golden Hour', artist: '', src: 'audio/golden_hour.mp3'},
      {title: 'Say Yes To Heaven', artist: '', src: 'audio/say_yes_to_heaven.mp3'}
    ];

    // App state
    let tracks = JSON.parse(JSON.stringify(defaultTracks));
    let currentIndex = 0;

    const playlistEl = document.getElementById('playlist');

    function formatTime(sec){
      if(isNaN(sec)) return '0:00';
      sec = Math.floor(sec);
      const m = Math.floor(sec/60); const s = sec%60; return m+':'+(s<10? '0'+s: s);
    }

    function createTrackEl(track, i){
      const div = document.createElement('div'); div.className='track'; div.dataset.index=i;
      div.innerHTML = `
        <h3>${track.title}</h3>
        <div class="meta">${track.artist || ''}</div>
        <div class="controls">
          <div class="buttons">
            <button class="icon prev" title="Previous">⏮️</button>
            <button class="icon play" title="Play">▶️</button>
            <button class="icon next" title="Next">⏭️</button>
            <button class="icon replay" title="Replay">🔁</button>
          </div>
          <div class="progress" aria-hidden="true"><i></i></div>
          <div class="time">0:00</div>
        </div>
      `;

      // create audio element
      const audio = document.createElement('audio'); audio.src = track.src; audio.preload='metadata'; audio.dataset.index = i; audio.crossOrigin = 'anonymous'; div.appendChild(audio);

      // wire controls
      const playBtn = div.querySelector('.play'); const prevBtn = div.querySelector('.prev'); const nextBtn = div.querySelector('.next'); const replayBtn = div.querySelector('.replay'); const progressBar = div.querySelector('.progress > i'); const timeEl = div.querySelector('.time');

      playBtn.addEventListener('click', ()=>{
        if(audio.paused) playTrack(i); else pauseAll();
      });
      prevBtn.addEventListener('click', ()=>{ prevTrack(); });
      nextBtn.addEventListener('click', ()=>{ nextTrack(); });
      replayBtn.addEventListener('click', ()=>{ audio.currentTime = 0; audio.play(); updateUI(); });

      audio.addEventListener('timeupdate', ()=>{
        const pct = (audio.currentTime / (audio.duration || 1)) * 100; progressBar.style.width = pct + '%'; timeEl.textContent = formatTime(audio.currentTime);
      });

      audio.addEventListener('ended', ()=>{ // auto-next
        if(i < tracks.length -1) nextTrack();
        else { // reached last track
          showSwipeHint();
        }
      });

      // allow tapping progress to seek
      div.querySelector('.progress').parentElement.addEventListener('click',(e)=>{
        const rect = e.currentTarget.querySelector('.progress').getBoundingClientRect();
        const clickX = e.clientX - rect.left; const pct = clickX/rect.width; audio.currentTime = pct * (audio.duration || 1);
      });

      return div;
    }

    function renderPlaylist(){
      playlistEl.innerHTML='';
      tracks.forEach((t,i)=>playlistEl.appendChild(createTrackEl(t,i)));
      highlightCurrent();
    }

    function pauseAll(){
      document.querySelectorAll('audio').forEach(a=>a.pause());
      updateUI();
    }

    function playTrack(i){
      pauseAll();
      const audio = document.querySelector(`audio[data-index='${i}']`);
      if(!audio) return; audio.play(); currentIndex = i; highlightCurrent(); scrollToTrack(i);
    }

    function scrollToTrack(i){
      const el = playlistEl.querySelector(`.track[data-index='${i}']`);
      if(el) el.scrollIntoView({behavior:'smooth',block:'center'});
    }

    function nextTrack(){
      const next = Math.min(currentIndex+1, tracks.length-1);
      playTrack(next);
    }
    function prevTrack(){
      const prev = Math.max(currentIndex-1, 0);
      playTrack(prev);
    }

    function highlightCurrent(){
      document.querySelectorAll('.track').forEach(t=>t.style.boxShadow='none');
      const cur = document.querySelector(`.track[data-index='${currentIndex}']`);
      if(cur) cur.style.boxShadow='0 8px 28px rgba(7,14,28,0.6)';
      // update play/pause icons
      document.querySelectorAll('.track').forEach(div=>{
        const audio = div.querySelector('audio'); const btn = div.querySelector('.play'); if(!audio) return;
        btn.textContent = audio.paused ? '▶️' : '⏸️';
      });
    }

    function updateUI(){
      document.querySelectorAll('.track').forEach(div=>{
        const audio = div.querySelector('audio'); const btn = div.querySelector('.play'); const progressBar = div.querySelector('.progress > i'); const timeEl = div.querySelector('.time');
        btn.textContent = audio && !audio.paused ? '⏸️' : '▶️';
        if(audio){
          const pct = (audio.currentTime / (audio.duration || 1)) * 100; progressBar.style.width = pct + '%'; timeEl.textContent = formatTime(audio.currentTime);
        }
      });
    }

    // detect swipe up to certificate
    let touchStartY = null;
    const mainPanel = document.getElementById('main');
    mainPanel.addEventListener('touchstart', e=>{ if(e.touches && e.touches[0]) touchStartY = e.touches[0].clientY; });
    mainPanel.addEventListener('touchmove', e=>{});
    mainPanel.addEventListener('touchend', e=>{ if(!touchStartY) return; const endY = (e.changedTouches && e.changedTouches[0]) ? e.changedTouches[0].clientY : null; if(endY === null) return; const dy = touchStartY - endY; if(dy > 80){ // swipe up
        revealCertificate();
      } touchStartY = null; });

    document.getElementById('nextToCert').addEventListener('click', revealCertificate);

    function showSwipeHint(){
      // small animation or vibration (if available)
      try{ navigator.vibrate && navigator.vibrate(80);}catch(e){}
      // visual hint
      const hint = document.querySelector('.swipe-hint'); if(hint) hint.style.color = 'var(--accent)';
    }

    function revealCertificate(){
      // pause music
      pauseAll();
      document.querySelector('.app').style.display='none';
      const cert = document.getElementById('certificate'); cert.classList.remove('hidden'); cert.setAttribute('aria-hidden', 'false');
      // simple appear animation
      cert.style.opacity=0; setTimeout(()=>cert.style.opacity=1,30);
    }

    // Allow users to upload local audio files to replace placeholders
    function addUploadControl(){
      const uploader = document.createElement('div'); uploader.style.marginTop='12px'; uploader.style.fontSize='13px'; uploader.innerHTML = `
        <label style="display:inline-block;margin-bottom:8px;color:var(--muted)">Replace tracks with local files (optional): </label>
        <input id="fileInput" type="file" accept="audio/*" multiple style="width:100%" />
      `;
      playlistEl.parentElement.insertBefore(uploader, playlistEl.nextSibling);
      document.getElementById('fileInput').addEventListener('change', e=>{
        const files = Array.from(e.target.files).slice(0,3);
        files.forEach((f, idx)=>{ tracks[idx] = {title: f.name.replace(/\.mp3|\.wav|\.m4a/ig,'') || 'Local track', artist:'', src: URL.createObjectURL(f)}; });
        renderPlaylist();
      });
    }

    // initial render
    renderPlaylist(); addUploadControl();

    // regularly update UI
    setInterval(updateUI, 200);

    // allow keyboard arrows on desktop for testing
    window.addEventListener('keydown', e=>{
      if(e.key === 'ArrowRight') nextTrack();
      if(e.key === 'ArrowLeft') prevTrack();
      if(e.key === ' ') { e.preventDefault(); const cur = document.querySelector(`audio[data-index='${currentIndex}']`); if(cur) cur.paused ? cur.play() : cur.pause(); }
    });

    // FINAL SWIPE on certificate to finish
    let certStartY=null;
    const cert = document.getElementById('certificate');
    cert.addEventListener('touchstart', e=>{ if(e.touches&&e.touches[0]) certStartY = e.touches[0].clientY; });
    cert.addEventListener('touchend', e=>{ if(!certStartY) return; const endY = (e.changedTouches && e.changedTouches[0]) ? e.changedTouches[0].clientY : null; if(endY===null) return; const dy = certStartY - endY; if(dy > 80){ finishExperience(); } certStartY=null; });
    cert.addEventListener('click', finishExperience);

    function finishExperience(){
      // simple finish animation and offer to download certificate as PNG or print
      alert('Thanks — Experience finished. You can screenshot or print the certificate.');
    }

    // Accessibility note: if the provided src files are invalid, instruct user
    // (We intentionally keep this minimal—replace the audio/* paths with hosted files or load local files.)
  </script></body>
</html>
