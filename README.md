
1 file changed
+160
-0
lines changed
Search within code
 
‎index.html‎
+160
Lines changed: 160 additions & 0 deletions
Original file line number	Diff line number	Diff line change
@@ -0,0 +1,160 @@
<!DOCTYPE html>
<html lang="si">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>For My Suralee ❤️</title>
    <link href="https://fonts.googleapis.com/css2?family=Parisienne&family=Lora:ital,wght@0,400;1,500&display=swap" rel="stylesheet">
    <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.5.1/dist/confetti.browser.min.js"></script>
    <style>
        body { margin: 0; min-height: 100vh; font-family: 'Lora', serif; background: linear-gradient(to bottom right, #ffdde1, #ee9ca7); text-align: center; color: #4a4a4a; overflow-x: hidden; }
        .screen { min-height: 100vh; display: flex; flex-direction: column; justify-content: center; align-items: center; padding: 20px; box-sizing: border-box; }
        h1.fancy { font-family: 'Parisienne', cursive; color: #d63384; font-size: 32px; margin-bottom: 20px; text-shadow: 1px 1px 2px rgba(0,0,0,0.1); }
        .big-heart { font-size: 70px; color: #ff4d6d; animation: beat 1.5s infinite; }
        @keyframes beat { 0%, 100% { transform: scale(1); } 50% { transform: scale(1.15); } }
        .btn-group { position: relative; width: 100%; height: 100px; max-width: 300px; margin-top: 30px; }
        button { padding: 15px 35px; font-size: 18px; border: none; border-radius: 50px; cursor: pointer; font-weight: bold; box-shadow: 0 5px 15px rgba(0,0,0,0.1); transition: 0.3s; }
        #yes { background: linear-gradient(45deg, #ff4d6d, #ff8fa3); color: white; z-index: 10; position: relative; }
        #no { background: #b2bec3; color: white; position: absolute; left: 0; }
        #success, #letter { display: none; padding-top: 50px; }
        .gallery { display: grid; grid-template-columns: 1fr 1fr; gap: 15px; width: 100%; max-width: 500px; margin-top: 20px; }
        .card { height: 210px; perspective: 1000px; cursor: pointer; }
        .inner { position: relative; width: 100%; height: 100%; transition: 0.8s; transform-style: preserve-3d; border-radius: 12px; box-shadow: 0 6px 12px rgba(0,0,0,0.15); }
        .card.flipped .inner { transform: rotateY(180deg); }
        .front, .back { position: absolute; width: 100%; height: 100%; backface-visibility: hidden; border-radius: 12px; overflow: hidden; }
        .front img { width: 100%; height: 100%; object-fit: cover; }
        .back { background: linear-gradient(135deg, #ff4d6d 0%, #d63384 100%); color: white; transform: rotateY(180deg); display: flex; align-items: center; justify-content: center; padding: 15px; font-size: 13px; font-weight: 500; box-sizing: border-box; line-height: 1.5; text-align: center; }
        .paper { background: #fffdf0; padding: 40px 25px; border-radius: 5px; box-shadow: 0 10px 30px rgba(0,0,0,0.15); max-width: 550px; text-align: left; line-height: 1.9; margin-bottom: 30px; border: 1px solid #e8e0c5; color: #333; font-size: 16.5px; }
        .sig { font-family: 'Parisienne', cursive; font-size: 32px; color: #d63384; text-align: right; margin-top: 25px; }
        .msg-btn { background: linear-gradient(45deg, #d63384, #ff4d6d); color: white; margin-top: 30px; border: 2px solid white; animation: pulse 2s infinite; }
        @keyframes pulse { 0% { box-shadow: 0 0 0 0 rgba(214, 51, 132, 0.4); } 70% { box-shadow: 0 0 0 10px rgba(214, 51, 132, 0); } 100% { box-shadow: 0 0 0 0 rgba(214, 51, 132, 0); } }
        
        /* අලුතින් එකතු කළ Music Button එක */
        #music-toggle {
            position: fixed; bottom: 20px; right: 20px; width: 50px; height: 50px;
            border-radius: 50%; background: #ff4d6d; color: white; border: 2px solid white;
            display: none; align-items: center; justify-content: center; font-size: 20px;
            box-shadow: 0 4px 10px rgba(0,0,0,0.3); z-index: 2000; cursor: pointer;
        }
    </style>
</head>
<body>
    <audio id="music" loop preload="auto">
        <source src="https://docs.google.com/uc?export=open&id=1-IxwpTLbEwoNu2Z4hpOKw2INNyPGg5Ud" type="audio/mpeg">
    </audio>
    <div id="music-toggle" onclick="toggleMusic()">🎵</div>
    <div id="q-screen" class="screen">
        <div class="big-heart">❤️</div>
        <h1 class="fancy">මගේ ආදරණීය සුරලි...<br>Will you be my Valentine?</h1>
        <div class="btn-group">
            <button id="yes" onclick="start()">ඔව්! මම කැමතියි 😍</button>
            <button id="no" onmouseover="move()" ontouchstart="move()">නෑ 💔</button>
        </div>
    </div>
    <div id="success" class="screen">
        <h1 class="fancy">Love You Suralee! ❤️</h1>
        <p style="font-style: italic; color: #666;">පින්තූරයක් touch කරලා මගේ හිතේ තියෙන දේ බලන්න... 👇</p>
        <div class="gallery" id="g"></div>
        <button class="msg-btn" onclick="showLetter()">💌 මගේ හිතේ තියෙන දේ කියවන්න</button>
        <div style="height: 50px;"></div>
    </div>
    <div id="letter" class="screen">
        <h1 class="fancy">මගේ සුරලිට... ❤️</h1>
        <div class="paper">
            <p>මගේ පණ වගේ ආදරණීය සුරලි වෙත,</p>
            <p>ඔයාට මේ ලියුම ලියන හැම මොහොතකම මගේ හිතේ තියෙන්නේ වචන වලින් විස්තර කරන්න බැරි තරම් ලොකු ආදරයක්. ඔයා මගේ ජීවිතේට ආපු දවස තමයි මගේ ජීවිතේ වාසනාවන්තම දවස. එදා ඉඳන් අද වෙනකම් ඔයා මට දුන්න ආදරේ, මාව තේරුම් ගත්ත විදිය මට මගේ පණටත් වඩා වටිනවා.</p>
            <p>සමහර වෙලාවට මම ඔයාව රිදවන්න ඇති, පොඩි පොඩි දේවල් වලට අපි රණ්ඩු වෙන්න ඇති. හැබැයි ඒ හැම දේකටම පස්සේ ඔයා මගේ ළඟටම වෙලා මාව තේරුම් ගන්නකොට මට දැනෙන්නේ මම මේ ලෝකේ ඉන්න වාසනාවන්තම කෙනා කියලා. ඔයාගේ හිනාව, ඔයාගේ ඒ ආදරණීය බැල්ම මගේ දවසම ලස්සන කරන්න ඕනෑවටත් වඩා වැඩියි.</p>
            <p>මම දන්නවා මම හැමදාම පරිපූර්ණ කෙනෙක් නෙවෙයි කියලා, හැබැයි මම එක දෙයක් දන්නවා... මගේ මේ හුස්ම තියෙනකම් මම මේ ලෝකේ වැඩියෙන්ම ආදරේ කරන්නේ මගේ සුරලිට විතරයි. මම පොරොන්දු වෙනවා ඔයාගේ අතින් අල්ලගෙන මේ ජීවිතේ තියෙන හැම දුකකදීම වගේම සැපකදීම ඔයාගේ හෙවනැල්ල වගේ ඉන්නවා කියලා. ඔයාගේ ලස්සන ඇස් දෙකෙන් සතුටට මිසක් කිසිම දවසක දුකට කඳුළක් වැටෙන්න මම ඉඩ දෙන්නේ නෑ.</p>
            <p>ඔයා තමයි මගේ ලෝකය, මගේ හුස්ම, මගේ හැමදේම. මේ වැලන්ටයින් දවසේ මට ඔයාට කියන්න තියෙන්නේ මගේ අවසාන හුස්ම දක්වාම මම ඔයාගේ විතරයි කියලා. මම ඔයාට පිස්සුවෙන් වගේ ආදරෙයි මගේ රත්තරන් මැණික!</p>
            <div class="sig">මන් ඔයාගෙ vishuu 🌹</div>
        </div>
        <button onclick="back()" style="background:#b2bec3; color:white; font-size:14px; padding:10px 20px; border:none; border-radius:50px;">Back</button>
    </div>
    <script>
        const music = document.getElementById('music');
        const musicBtn = document.getElementById('music-toggle');
        function toggleMusic() {
            if (music.paused) {
                music.play();
                musicBtn.innerHTML = "⏸️";
            } else {
                music.pause();
                musicBtn.innerHTML = "▶️";
            }
        }
        const data = [
            {u: "https://i.postimg.cc/rKDGcsdb/628C5370-E60F-4EDB-AA3E-BBDF85C6DD07.png", t: "ඔයා මගේ ජීවිතේට ආපු එක මට ලැබුණු ලොකුම වාසනාව සහ සතුටයි මැණික."},
            {u: "https://i.postimg.cc/56X51yHr/82C9D672-DFFB-4D40-BA85-E8D7BE3F0093.png", t: "මම මේ ලෝකේ වැඩියෙන්ම ආදරේ කරන ලස්සනම සහ හරිම කරුණාවන්තම කෙනා ඔයායි."},
            {u: "https://i.postimg.cc/zVyw5vb1/99732FBA-FDA0-4C51-93DE-832746EECD1F.jpg", t: "ඔයාගේ ඔය ලස්සන හිනාව මගේ ජීවිතේ තියෙන හැම දුකක්ම අමතක කරවනවා සත්තයි."},
            {u: "https://i.postimg.cc/Jt639ZnW/A6566DBA-09DC-4876-BCA6-9319A82BC3D1.jpg", t: "සුරලි, ඔයා මගේ ජීවිතේට ලැබුණු වටිනාම මැණික කියලා මම හැමදාම හිතනවා රත්තරං."},
            {u: "https://i.postimg.cc/F7YbmzdX/A9F9B2C8-21FB-4EAC-8791-046939C06FB0.jpg", t: "මගේ ජීවිතේ අවසානය වෙනකම්ම ඔයාගේ අතින් අල්ලගෙන මට ඉන්න ඕනේ මගේ ආදරී."},
            {u: "https://i.postimg.cc/9rDPV0Rm/Air-Brush-20251026071510.jpg", t: "ඔයා මගේ ළඟ ඉන්නකොට මට දැනෙන සතුට වචන වලින් විස්තර කරන්න කොහොමටවත්ම බෑ."},
            {u: "https://i.postimg.cc/Bj1C5b4k/IMG-0782.jpg", t: "ඔයා නැති ලෝකෙක මට තත්පරයක්වත් ඉන්න බෑ කියලා මට අද හදවතින්ම හිතෙනවා."},
            {u: "https://i.postimg.cc/rKDGcs0T/IMG-20251220-121050.jpg", t: "මගේ හදවතේ ගැහෙන හැම හුස්මක් පාසාම මම ඔයාට ගොඩක් ආදරේ කරනවා මැණික."},
            {u: "https://i.postimg.cc/jwRQrhP0/IMG-20251227-124409.jpg", t: "අපි දෙන්නගේ මේ ආදරේ මතු මතු ආත්මෙකත් මේ වගේම පවතින්න කියලා මම පතනවා."},
            {u: "https://i.postimg.cc/5XFm5yb5/IMG-20251227-124511.jpg", t: "හැමදාම මගේ ළඟින්ම ඉඳගෙන මගේ හෙවනැල්ල වෙලා මාව සතුටින් තියන්න මගේ රත්තරං."},
            {u: "https://i.postimg.cc/7GqMyN7F/IMG-20251227-124730.jpg", t: "ඔයා මගේ ජීවිතේ හැමදේම වගේම මගේ මුළු ලෝකයම කියලා මම අද පොරොන්දු වෙනවා."},
            {u: "https://i.postimg.cc/B89Bm3vn/IMG-20251227-124948.jpg", t: "මගේ ජීවිතේට ඔයා වගේ කෙනෙක් ආපු එක ගැන මම දෙවියන්ට හැමදාම ස්තූති කරනවා."},
            {u: "https://i.postimg.cc/SYs6nNgc/IMG-3337.jpg", t: "ඔයා මට දෙන ඒ පුංචි ආදරේ මට ජීවත් වෙන්න ලොකු ශක්තියක් සහ ජීවයක් දෙනවා."},
            {u: "https://i.postimg.cc/nMRK6Bzf/IMG-4077.jpg", t: "මගේ හිතේ තියෙන ආදරේ ඔයාට කියන්න මේ වචන කොහොමටවත්ම ප්‍රමාණවත් වෙන්නේ නෑ."},
            {u: "https://i.postimg.cc/8sKdqMC2/IMG-4404.jpg", t: "ඔයා මගේ ජීවිතේට ආපු දේවදූතියක් වගේ මගේ මුළු ලෝකයම හරිම ලස්සන කළා මැණික."},
            {u: "https://i.postimg.cc/grSyf801/IMG-4410.jpg", t: "අපි ගත කරපු හැම මතකයක්ම මගේ හදවතේ රත්තරං වගේ මම සදහටම තියාගන්නවා සත්තයි."},
            {u: "https://i.postimg.cc/vxBr4H0f/IMG-4463.jpg", t: "ඔයාගේ ඔය ලස්සන දෑස් දකිනකොට මට මගේ මුළු ලෝකයම පේනවා වගේ දැනෙනවා රත්තරං."},
            {u: "https://i.postimg.cc/LYvk4Z3K/IMG-6062.jpg", t: "මගේ ජීවිතේ ලැබුණු හොඳම තෑග්ග ඔයා කියලා මම අද මුළු හදවතින්ම හඬගා කියනවා."},
            {u: "https://i.postimg.cc/DJ7dKcLT/IMG-6733.jpg", t: "සත්තයි මම මගේ පණටත් වඩා වැඩියෙන් ඔයාට මේ ලෝකේ ආදරේ කරනවා මගේ රත්තරං."},
            {u: "https://i.postimg.cc/HcT4CtXm/IMG-6765.jpg", t: "ඔයා ගැන හිතන හැම මොහොතකම මගේ දවස හරිම සතුටින් සහ ලස්සනට ගෙවෙනවා මැණික."},
            {u: "https://i.postimg.cc/V5G93nNC/IMG-7048.jpg", t: "මගේ හදවතේ එකම අයිතිකාරිය ඔයා විතරයි කියලා මම අද පොරොන්දු වෙනවා මගේ ආදරී."},
            {u: "https://i.postimg.cc/4nLvrV35/IMG-8720.png", t: "සුරලි, මගේ අවසාන හුස්ම යනකම්ම මම ඔයාගේ විතරයි කියලා මම අද පොරොන්දු වෙනවා."}
        ];
        let s = 1;
        function move() {
            const b = document.getElementById('no');
            b.style.position = 'fixed';
            b.style.left = Math.random() * (window.innerWidth - 80) + 'px';
            b.style.top = Math.random() * (window.innerHeight - 50) + 'px';
            s += 0.5; document.getElementById('yes').style.transform = `scale(${s})`;
        }
        function start() {
            music.play().then(() => {
                musicBtn.style.display = "flex";
                musicBtn.innerHTML = "⏸️";
            }).catch(() => {
                musicBtn.style.display = "flex";
                musicBtn.innerHTML = "▶️";
            });
            confetti({ particleCount: 200, spread: 90, origin: { y: 0.6 } });
            document.getElementById('q-screen').style.display = 'none';
            document.getElementById('success').style.display = 'flex';
            document.body.style.overflow = 'auto';
            const g = document.getElementById('g');
            data.forEach(item => {
                const c = document.createElement('div'); c.className = 'card';
                c.onclick = () => c.classList.toggle('flipped');
                c.innerHTML = `<div class="inner"><div class="front"><img src="${item.u}"></div><div class="back">${item.t}</div></div>`;
                g.appendChild(c);
            });
        }
        function showLetter() {
            confetti({ particleCount: 150, spread: 70, origin: { y: 0.8 } });
            document.getElementById('success').style.display = 'none';
            document.getElementById('letter').style.display = 'flex';
            window.scrollTo(0, 0);
        }
        function back() {
            document.getElementById('letter').style.display = 'none';
            document.getElementById('success').style.display = 'flex';
        }
    </script>
</body>
  </html>
