<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
    <title>Will You Be My Valentine?</title>
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: #fffafb;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            text-align: center;
        }

        .container {
            max-width: 420px;
            padding: 20px;
        }

        .sticker {
            width: 150px;
            margin-bottom: 20px;
            display: block;
            margin-left: auto;
            margin-right: auto;
        }

        h1 {
            color: #d63384;
            font-size: 24px;
            margin: 0.3em 0;
        }

        .btn-group {
            display: flex;
            justify-content: center;
            gap: 20px;
            margin-top: 20px;
        }

        button {
            padding: 10px 25px;
            border: none;
            border-radius: 20px;
            cursor: pointer;
            font-weight: bold;
            transition: transform 0.2s;
        }

        .yes-btn { background-color: #4CAF50; color: white; }
        .no-btn { background-color: #ff4d4d; color: white; }
        .back-btn { background-color: #888; color: white; margin-top: 20px; }

        .hidden { display: none; }

        .gift-container {
            display: flex;
            justify-content: center;
            gap: 15px;
            margin-top: 20px;
        }

        .gift-box {
            font-size: 40px;
            cursor: pointer;
            transition: transform 0.2s;
            user-select: none;
        }

        .gift-box:hover { transform: scale(1.2); }

        #playback-error { color: #cc0000; margin-top: 12px; display:none; font-size:0.95em; }
        #manualPlayBtn { margin-top: 8px; display:none; background:#4CAF50; color:white; padding:8px 16px; border-radius:16px; border:none; cursor:pointer; }
    </style>
</head>
<body>

    <!-- Page 1 -->
    <div id="page1" class="container">
        <svg class="sticker" viewBox="0 0 64 64" xmlns="http://www.w3.org/2000/svg" aria-hidden="true">
            <path fill="#ff6b9a" d="M47.6 6c-5.1 0-9.7 2.8-12.1 6.9C32.1 8.8 27.5 6 22.4 6 12.3 6 4 14.3 4 24.4 4 39 27 53 32 58c5-5 28-19 28-33.6C60 14.3 51.7 6 41.6 6z"/>
        </svg>

        <h1>Will You Be My Valentine?</h1>
        <div class="btn-group">
            <button class="yes-btn" onclick="acceptYes()">Yes</button>
            <button class="no-btn" id="noBtn" onmouseover="moveNoButton()">No</button>
        </div>
    </div>

    <!-- Page No -->
    <div id="page-no" class="container hidden">
        <svg class="sticker" viewBox="0 0 64 64" xmlns="http://www.w3.org/2000/svg" aria-hidden="true">
            <circle cx="32" cy="32" r="30" fill="#ffeef0"/>
            <path d="M20 24c2 4 6 6 12 6s10-2 12-6" fill="none" stroke="#d63384" stroke-width="2" stroke-linecap="round"/>
            <circle cx="24" cy="24" r="3" fill="#d63384"/>
            <circle cx="40" cy="24" r="3" fill="#d63384"/>
        </svg>

        <h1>I don't accept No as an answer!</h1>
        <button class="back-btn" onclick="nextPage('page1')">Go Back</button>
    </div>

    <!-- Gifts page -->
    <div id="page-gifts" class="container hidden">
        <svg class="sticker" viewBox="0 0 64 64" xmlns="http://www.w3.org/2000/svg" aria-hidden="true">
            <rect x="8" y="20" width="48" height="28" rx="3" fill="#ffdede"/>
            <path d="M32 20v-8" stroke="#ff6b9a" stroke-width="4" stroke-linecap="round"/>
            <path d="M20 20c0-6 6-8 12-8s12 2 12 8" fill="none" stroke="#ff6b9a" stroke-width="3" stroke-linecap="round"/>
        </svg>

        <h1>Choose a Gift!</h1>
        <div class="gift-container">
            <div class="gift-box" onclick="showReward('flowers')" title="Flowers">🎁</div>
            <div class="gift-box" onclick="showReward('music')" title="Music">🎁</div>
            <div class="gift-box" onclick="showReward('letter')" title="Letter">🎁</div>
        </div>
    </div>

    <!-- Reward page -->
    <div id="reward-page" class="container hidden">
        <div id="reward-content"></div>
        <button class="back-btn" onclick="nextPage('page-gifts')">Go Back</button>
    </div>

    <!-- Audio element for the song.
         IMPORTANT: You must add the MP3 file yourself (see instructions below).
         Default path: assets/about_you_the_1975.mp3
    -->
    <audio id="song" preload="none" src="assets/about_you_the_1975.mp3"></audio>

    <div id="playback-error" role="status" aria-live="polite"></div>
    <button id="manualPlayBtn" onclick="manualPlay()">Play song</button>

    <script>
        function nextPage(pageId) {
            document.querySelectorAll('.container').forEach(page => {
                page.classList.add('hidden');
            });
            const el = document.getElementById(pageId);
            if (el) el.classList.remove('hidden');
        }

        function moveNoButton() {
            // playful redirect on hover
            nextPage('page-no');
        }

        function showReward(type) {
            const content = document.getElementById('reward-content');
            nextPage('reward-page');

            if (type === 'flowers') {
                content.innerHTML = `
                    <div style="font-size:1.1em;">
                        <div style="font-size:72px;">💐</div>
                        <h3>Got This For You &lt;3</h3>
                        <p>Sweet treats and flowers!</p>
                    </div>
                `;
            } else if (type === 'music') {
                content.innerHTML = `
                    <div style="font-size:1.1em;">
                        <div style="font-size:72px;">🎵</div>
                        <h3>A song for you...</h3>
                        <p>Playing: "About You" — The 1975</p>
                    </div>
                `;
                // Try to play when user explicitly chose music
                playSong().catch(err => {
                    console.warn('Playback failed:', err);
                });
            } else {
                content.innerHTML = `
                    <div style="font-size:1.1em;">
                        <div style="font-size:72px;">💌</div>
                        <h3>A Note for You</h3>
                        <p style="font-style: italic; font-size: 0.95em;">"In your eyes, I have found a home..."</p>
                    </div>
                `;
            }
        }

        function acceptYes() {
            // Navigate first to provide instant feedback (counts as user gesture)
            nextPage('page-gifts');

            playSong().then(() => {
                console.log('Playback started');
            }).catch(err => {
                const errEl = document.getElementById('playback-error');
                errEl.textContent = 'Could not start audio playback automatically. Tap "Play song" to start the music.';
                errEl.style.display = 'block';
                document.getElementById('manualPlayBtn').style.display = 'inline-block';
                console.warn('Playback error:', err);
            });
        }

        async function playSong() {
            const audio = document.getElementById('song');
            if (!audio) return Promise.reject(new Error('Audio element not found'));

            try {
                // Load the audio if it's not ready
                if (audio.readyState === 0) audio.load();
            } catch (e) {
                console.warn('Audio load failed:', e);
            }

            const playPromise = audio.play();
            if (playPromise !== undefined) {
                await playPromise; // will reject if playback is blocked
            }
        }

        function manualPlay() {
            playSong().then(() => {
                document.getElementById('playback-error').style.display = 'none';
                document.getElementById('manualPlayBtn').style.display = 'none';
            }).catch(err => {
                const errEl = document.getElementById('playback-error');
                errEl.textContent = 'Playback still blocked. Try tapping the page or check the audio file/path.';
                errEl.style.display = 'block';
                console.warn('Manual play failed:', err);
            });
        }

        // Accessibility / keyboard: allow pressing 'y' to accept
        document.addEventListener('keydown', (e) => {
            if (e.key === 'y' || e.key === 'Y') acceptYes();
        });
    </script>
</body>
</html>
