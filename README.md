<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    <title>Pour Therancia</title>
    <!-- Import d'une police élégante -->
    <link href="https://fonts.googleapis.com/css2?family=Dancing+Script:wght@600&family=Poppins:wght@300;400&display=swap" rel="stylesheet">
    <style>
        :root {
            --primary: #ff4d6d; /* Ta couleur dans 1001112094.jpg */
            --bg: #0a0a0a;
        }

        body {
            margin: 0;
            padding: 0;
            background-color: var(--bg);
            color: white;
            font-family: 'Poppins', sans-serif;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            height: 100vh;
            overflow: hidden;
        }

        /* Fond étoilé */
        #stars {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: 0;
        }

        .header-text {
            font-family: 'Dancing Script', cursive;
            font-size: 3rem;
            color: var(--primary);
            margin-bottom: 20px;
            z-index: 1;
            text-shadow: 0 0 15px rgba(255, 77, 109, 0.5);
            animation: fadeIn 2s ease-in;
        }

        .card-container {
            position: relative;
            width: 320px;
            height: 450px;
            z-index: 1;
            border-radius: 30px;
            box-shadow: 0 0 50px rgba(0,0,0,0.8), 0 0 20px rgba(255, 77, 109, 0.2);
            overflow: hidden;
        }

        /* Message caché */
        .reveal-layer {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: linear-gradient(135deg, #1a1a1a, #2d1b1e);
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            padding: 30px;
            text-align: center;
            box-sizing: border-box;
        }

        .reveal-layer p {
            font-size: 1.1rem;
            line-height: 1.8;
            color: #eee;
            margin: 0;
        }

        .reveal-layer .heart-icon {
            color: var(--primary);
            font-size: 2rem;
            margin-top: 20px;
            animation: pulse 1.5s infinite;
        }

        /* Couche à gratter */
        #scratchCanvas {
            position: absolute;
            top: 0;
            left: 0;
            z-index: 2;
            touch-action: none;
        }

        .hint {
            margin-top: 25px;
            font-size: 0.8rem;
            letter-spacing: 2px;
            opacity: 0.5;
            text-transform: uppercase;
            z-index: 1;
        }

        /* Animations */
        @keyframes fadeIn { from { opacity: 0; transform: translateY(-10px); } to { opacity: 1; transform: translateY(0); } }
        @keyframes pulse { 0% { transform: scale(1); } 50% { transform: scale(1.2); } 100% { transform: scale(1); } }

        .floating-heart {
            position: fixed;
            pointer-events: none;
            color: var(--primary);
            z-index: 10;
            animation: flyUp 1s ease-out forwards;
        }

        @keyframes flyUp {
            0% { transform: translateY(0) scale(1); opacity: 1; }
            100% { transform: translateY(-100px) scale(0); opacity: 0; }
        }
    </style>
</head>
<body>

    <canvas id="stars"></canvas>

    <div class="header-text">pour therancia</div>

    <div class="card-container">
        <div class="reveal-layer">
            <p>
                Therancia,<br><br>
                On dit que le mystère attire, mais c'est ta lumière qui retient. 
                J'aime nos silences autant que nos mots. 
                <br><br>
                Une petite surprise digitale, parce que tu mérites qu'on crée des mondes pour toi.
            </p>
            <div class="heart-icon">♥</div>
        </div>
        <canvas id="scratchCanvas"></canvas>
    </div>

    <div class="hint">Efface pour révéler le secret</div>

    <script>
        // --- Fond étoilé ---
        const sCanvas = document.getElementById('stars');
        const sCtx = sCanvas.getContext('2d');
        let stars = [];

        function initStars() {
            sCanvas.width = window.innerWidth;
            sCanvas.height = window.innerHeight;
            for(let i=0; i<150; i++) {
                stars.push({x: Math.random()*sCanvas.width, y: Math.random()*sCanvas.height, size: Math.random()*1.5, speed: Math.random()*0.5});
            }
        }

        function drawStars() {
            sCtx.clearRect(0,0,sCanvas.width, sCanvas.height);
            sCtx.fillStyle = "white";
            stars.forEach(s => {
                sCtx.beginPath();
                sCtx.arc(s.x, s.y, s.size, 0, Math.PI*2);
                sCtx.fill();
                s.y += s.speed;
                if(s.y > sCanvas.height) s.y = 0;
            });
            requestAnimationFrame(drawStars);
        }

        // --- Grattage ---
        const canvas = document.getElementById('scratchCanvas');
        const ctx = canvas.getContext('2d');
        canvas.width = 320;
        canvas.height = 450;

        function initScratch() {
            // Fond du grattage (Sombre et texturé)
            ctx.fillStyle = '#1a1a1a';
            ctx.fillRect(0,0, canvas.width, canvas.height);
            
            // Effet de texture "poussière d'étoile"
            for(let i=0; i<500; i++){
                ctx.fillStyle = `rgba(255, 77, 109, ${Math.random()*0.15})`;
                ctx.fillRect(Math.random()*canvas.width, Math.random()*canvas.height, 1, 1);
            }

            ctx.globalCompositeOperation = 'destination-out';
        }

        function scratch(x, y) {
            ctx.beginPath();
            ctx.arc(x, y, 25, 0, Math.PI * 2);
            ctx.fill();
            createHeart(x, y);
        }

        // --- Animation de cœurs ---
        function createHeart(x, y) {
            const rect = canvas.getBoundingClientRect();
            const heart = document.createElement('div');
            heart.className = 'floating-heart';
            heart.innerHTML = '♥';
            heart.style.left = (rect.left + x) + 'px';
            heart.style.top = (rect.top + y) + 'px';
            heart.style.fontSize = (Math.random() * 20 + 10) + 'px';
            document.body.appendChild(heart);
            setTimeout(() => heart.remove(), 1000);
        }

        // Events
        canvas.addEventListener('touchmove', (e) => {
            e.preventDefault();
            const rect = canvas.getBoundingClientRect();
            const x = e.touches[0].clientX - rect.left;
            const y = e.touches[0].clientY - rect.top;
            scratch(x, y);
        });

        initStars();
        drawStars();
        initScratch();
    </script>
</body>
</html>
