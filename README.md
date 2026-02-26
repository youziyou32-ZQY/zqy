<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>给你的告白</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            width: 100%;
            height: 100vh;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            font-family: 'Arial', sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            overflow: hidden;
            position: relative;
        }

        /* 背景心形动画 */
        .heart {
            position: absolute;
            opacity: 0.3;
            animation: float 6s ease-in-out infinite;
            font-size: 2rem;
        }

        @keyframes float {
            0%, 100% {
                transform: translateY(0px) translateX(0px);
            }
            25% {
                transform: translateY(-20px) translateX(-15px);
            }
            50% {
                transform: translateY(-40px) translateX(0px);
            }
            75% {
                transform: translateY(-20px) translateX(15px);
            }
        }

        .container {
            background: white;
            border-radius: 20px;
            padding: 60px 50px;
            box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
            text-align: center;
            max-width: 600px;
            width: 90%;
            margin: auto;
            position: relative;
            z-index: 10;
            animation: slideIn 0.8s ease;
        }

        @keyframes slideIn {
            from {
                opacity: 0;
                transform: translateY(30px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        .header {
            margin-bottom: 40px;
        }

        .title {
            font-size: 2.5rem;
            color: #764ba2;
            margin-bottom: 20px;
            font-weight: bold;
            animation: pulse 2s ease infinite;
        }

        @keyframes pulse {
            0%, 100% {
                transform: scale(1);
            }
            50% {
                transform: scale(1.05);
            }
        }

        .heart-icon {
            font-size: 3rem;
            margin: 20px 0;
            animation: heartBeat 1.2s ease infinite;
        }

        @keyframes heartBeat {
            0%, 100% {
                transform: scale(1);
            }
            15% {
                transform: scale(1.2);
            }
            30% {
                transform: scale(1);
            }
        }

        .content {
            font-size: 1.1rem;
            color: #333;
            line-height: 1.8;
            margin-bottom: 40px;
            letter-spacing: 0.5px;
        }

        .message {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            padding: 30px;
            border-radius: 15px;
            margin-bottom: 40px;
            font-size: 1.2rem;
            font-weight: bold;
            line-height: 1.8;
        }

        .buttons {
            display: flex;
            gap: 20px;
            justify-content: center;
            flex-wrap: wrap;
        }

        button {
            padding: 15px 40px;
            font-size: 1.1rem;
            border: none;
            border-radius: 50px;
            cursor: pointer;
            font-weight: bold;
            transition: all 0.3s ease;
            text-transform: uppercase;
            letter-spacing: 1px;
        }

        .yes-btn {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            min-width: 150px;
        }

        .yes-btn:hover {
            transform: scale(1.1);
            box-shadow: 0 10px 30px rgba(118, 75, 162, 0.4);
        }

        .no-btn {
            background: #f0f0f0;
            color: #333;
            min-width: 150px;
        }

        .no-btn:hover {
            background: #e0e0e0;
            transform: translateX(50px);
        }

        .success-message {
            display: none;
            animation: confetti-animation 0.6s ease;
        }

        .confetti {
            position: fixed;
            width: 10px;
            height: 10px;
            background: #FF1493;
            animation: confetti-fall 3s ease-out forwards;
        }

        @keyframes confetti-fall {
            to {
                transform: translateY(100vh) rotate(360deg);
                opacity: 0;
            }
        }

        .final-message {
            font-size: 1.5rem;
            color: #764ba2;
            font-weight: bold;
            margin-top: 30px;
            animation: slideIn 0.8s ease;
        }

        .stars {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
        }

        .star {
            position: absolute;
            width: 2px;
            height: 2px;
            background: white;
            border-radius: 50%;
            animation: twinkle 3s infinite;
        }

        @keyframes twinkle {
            0%, 100% {
                opacity: 0.3;
            }
            50% {
                opacity: 1;
            }
        }

        /* 音乐控制按钮 */
        .music-btn {
            position: fixed;
            bottom: 30px;
            right: 30px;
            width: 60px;
            height: 60px;
            border-radius: 50%;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            border: none;
            color: white;
            font-size: 28px;
            cursor: pointer;
            box-shadow: 0 10px 30px rgba(118, 75, 162, 0.3);
            transition: all 0.3s ease;
            z-index: 100;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .music-btn:hover {
            transform: scale(1.1);
            box-shadow: 0 15px 40px rgba(118, 75, 162, 0.5);
        }

        .music-btn:active {
            transform: scale(0.95);
        }

        /* 移动设备适配 */
        @media (max-width: 768px) {
            .music-btn {
                bottom: 20px;
                right: 20px;
                width: 50px;
                height: 50px;
                font-size: 24px;
            }
        }
    </style>
</head>
<body>
    <!-- 背景音乐 - 钢琴纯音乐 -->
    <audio id="bgMusic" autoplay loop style="display: none;">
        <source src="https://pixabay.com/download/audio/2023/09/03/romantic-piano-131590.mp3" type="audio/mpeg">
    </audio>

    <!-- 音乐控制按钮 -->
    <button class="music-btn" id="musicBtn" title="暂停/播放音乐">🎵</button>

    <div class="stars" id="stars"></div>

    <div class="container">
        <div class="header">
            <div class="heart-icon">❤️</div>
            <h1 class="title">我有话要对你说</h1>
        </div>

        <div class="content" id="mainContent">
            <div class="message">
                从认识你的那一刻起，
                <br>
                我的世界就被你的光芒照亮。
                <br>
                <br>
                你的笑容，你的温柔，
                <br>
                你的善良，都深深吸引着我。
            </div>

            <p>
                我想和你一起看日出日落，<br>
                一起走过春夏秋冬，<br>
                在你身边度过每一个珍贵的时刻。
            </p>
        </div>

        <div class="buttons" id="buttons">
            <button class="yes-btn" onclick="handleYes()">💕 我也喜欢你</button>
            <button class="no-btn" onclick="handleNo()">再考虑考虑</button>
        </div>

        <div id="successSection" style="display: none;">
            <div class="message" style="margin-top: 40px;">
                耶！🎉<br>
                你让我成为世界上最幸福的人！
            </div>
            <div class="final-message">
                让我们携手共进美好未来吧！💑
            </div>
        </div>
    </div>

    <script>
        // 生成背景星星
        function generateStars() {
            const starsContainer = document.getElementById('stars');
            for (let i = 0; i < 100; i++) {
                const star = document.createElement('div');
                star.className = 'star';
                star.style.left = Math.random() * 100 + '%';
                star.style.top = Math.random() * 100 + '%';
                star.style.animationDelay = Math.random() * 3 + 's';
                starsContainer.appendChild(star);
            }
        }

        // 生成背景飘动的心
        function generateHearts() {
            const body = document.body;
            const hearts = ['❤️', '💕', '💖', '💗', '💝'];
            for (let i = 0; i < 10; i++) {
                const heart = document.createElement('div');
                heart.className = 'heart';
                heart.textContent = hearts[Math.floor(Math.random() * hearts.length)];
                heart.style.left = Math.random() * 100 + '%';
                heart.style.top = Math.random() * 100 + '%';
                heart.style.animationDelay = Math.random() * 2 + 's';
                body.appendChild(heart);
            }
        }

        // 点击"我也喜欢你"
        function handleYes() {
            const buttons = document.getElementById('buttons');
            const mainContent = document.getElementById('mainContent');
            const successSection = document.getElementById('successSection');

            buttons.style.display = 'none';
            mainContent.style.display = 'none';
            successSection.style.display = 'block';

            createConfetti();
            playHeartRain();
        }

        // "再考虑"按钮逃离效果
        function handleNo() {
            const noBtn = document.querySelector('.no-btn');
            const randomX = (Math.random() - 0.5) * 400;
            const randomY = (Math.random() - 0.5) * 400;
            
            // 添加平滑过渡和缩放效果
            noBtn.style.transition = 'all 0.3s cubic-bezier(0.34, 1.56, 0.64, 1)';
            noBtn.style.transform = `translate(${randomX}px, ${randomY}px) scale(1.05)`;
        }
        
        // 鼠标悬停时也让按钮逃离
        document.addEventListener('DOMContentLoaded', function() {
            const noBtn = document.querySelector('.no-btn');
            noBtn.addEventListener('mouseenter', function() {
                if (window.innerWidth > 768) { // 仅在桌面设备上启用
                    const randomX = (Math.random() - 0.5) * 250;
                    const randomY = (Math.random() - 0.5) * 250;
                    noBtn.style.transition = 'all 0.2s ease-out';
                    noBtn.style.transform = `translate(${randomX}px, ${randomY}px) scale(1.05)`;
                }
            });
        });

        // 彩纸掉落效果
        function createConfetti() {
            for (let i = 0; i < 50; i++) {
                const confetti = document.createElement('div');
                confetti.className = 'confetti';
                confetti.style.left = Math.random() * 100 + '%';
                confetti.style.top = '-10px';
                confetti.style.background = ['#FF1493', '#FF69B4', '#FFB6C1', '#FFC0CB'][Math.floor(Math.random() * 4)];
                confetti.style.animation = `confetti-fall ${2 + Math.random() * 1}s ease-out forwards`;
                document.body.appendChild(confetti);

                setTimeout(() => {
                    confetti.remove();
                }, 3000);
            }
        }

        // 心形雨效果
        function playHeartRain() {
            for (let i = 0; i < 30; i++) {
                setTimeout(() => {
                    const heart = document.createElement('div');
                    heart.style.position = 'fixed';
                    heart.style.left = Math.random() * 100 + '%';
                    heart.style.top = '-30px';
                    heart.style.fontSize = (20 + Math.random() * 30) + 'px';
                    heart.textContent = '❤️';
                    heart.style.animation = `confetti-fall ${2 + Math.random() * 1}s ease-out forwards`;
                    document.body.appendChild(heart);

                    setTimeout(() => {
                        heart.remove();
                    }, 3000);
                }, i * 50);
            }
        }

        // 音乐控制功能
        function initMusicControl() {
            const bgMusic = document.getElementById('bgMusic');
            const musicBtn = document.getElementById('musicBtn');

            musicBtn.addEventListener('click', function() {
                if (bgMusic.paused) {
                    bgMusic.play();
                    musicBtn.textContent = '🎵';
                    musicBtn.title = '点击暂停';
                } else {
                    bgMusic.pause();
                    musicBtn.textContent = '🔇';
                    musicBtn.title = '点击播放';
                }
            });
        }

        // 页面加载时初始化
        window.addEventListener('load', () => {
            generateStars();
            generateHearts();
            initMusicControl();
        });
    </script>
</body>
</html>
