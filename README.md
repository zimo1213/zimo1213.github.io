<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>青雀的个人网站</title>
    <style>
        /* 新增样式 */

        .light-beam {
            position: absolute;
            width: 100%;
            height: 100%;
            pointer-events: none;
            mix-blend-mode: screen;
        }

        /* 泡泡容器 */
        .bubbles {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            overflow: hidden;
            z-index: 0;
        }

        /* 单个泡泡 */
        .bubble {
            position: absolute;
            background: rgba(255, 255, 255, 0.1);
            border: 1px solid rgba(255, 255, 255, 0.2);
            border-radius: 50%;
            animation: float-to-top 8s ease-out forwards;
            backdrop-filter: blur(2px);
            pointer-events: auto;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        /* 泡泡悬停效果 */
        .bubble:hover {
            animation-play-state: paused;
            background: rgba(255, 255, 255, 0.25);
            border: 2px solid rgba(255, 255, 255, 0.6);
            box-shadow: 0 0 20px rgba(255, 255, 255, 0.4);
            transform: scale(1.1);
        }

        /* 泡泡破裂效果 */
        .bubble.burst {
            animation: burst 0.3s ease-out forwards;
        }

        /* 泡泡点击破裂效果 */
        .bubble.clicked {
            animation: click-burst 0.3s ease-out forwards;
        }

        /* 破裂粒子 */
        .burst-particle {
            position: absolute;
            width: 4px;
            height: 4px;
            background: rgba(255, 255, 255, 0.9);
            border-radius: 50%;
            pointer-events: none;
            box-shadow: 0 0 6px rgba(255, 255, 255, 0.6);
        }

        /* 烟花主要粒子 */
        .firework-particle {
            position: absolute;
            width: 3px;
            height: 3px;
            border-radius: 50%;
            pointer-events: none;
        }

        /* 烟花次级粒子 */
        .firework-spark {
            position: absolute;
            width: 2px;
            height: 2px;
            background: rgba(255, 255, 255, 0.8);
            border-radius: 50%;
            pointer-events: none;
        }

        /* 烟花爆炸光环 */
        .firework-ring {
            position: absolute;
            border: 2px solid rgba(255, 255, 255, 0.6);
            border-radius: 50%;
            pointer-events: none;
        }

        /* 粒子容器 */
        .particles {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: 0;
        }

        /* 单个粒子 */
        .particle {
            position: absolute;
            background: rgba(255, 255, 255, 0.8);
            border-radius: 50%;
            animation: sparkle 3s infinite ease-in-out;
        }

        /* 流星容器 */
        .shooting-stars {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: 0;
        }

        /* 单个流星 */
        .shooting-star {
            position: absolute;
            width: 2px;
            height: 2px;
            background: white;
            border-radius: 50%;
            box-shadow: 0 0 10px white;
            animation: shoot 3s linear infinite;
        }

        .shooting-star::after {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 50px;
            height: 1px;
            background: linear-gradient(90deg, white, transparent);
            transform-origin: left center;
        }

        .time-display {
            position: fixed;
            top: 20px;
            left: 0%;
           // transform: translateX(-50%);
            color: white;
            font-size: 24px;
            font-weight: bold;
            text-shadow: 0 0 10px rgba(0, 0, 0, 0.3);
            background: rgba(0, 0, 0, 0.1);
            padding: 8px 16px;
            border-radius:0px 24px 24px 0px;
            -select: none;
        }

        .profile {
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background: rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.2);
            border-radius: 20px;
            padding: 30px;
            text-align: center;
            color: white;
            font-size: 18px;
            font-weight: bold;
            text-shadow: 0 0 10px rgba(0, 0, 0, 0.3);
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
            z-index: 15;
            min-width: 280px;
        }

        .profile img {
            width: 120px;
            height: 120px;
            border-radius: 50%;
            border: 3px solid rgba(255, 255, 255, 0.3);
            margin-bottom: 20px;
            box-shadow: 0 4px 20px rgba(0, 0, 0, 0.2);
            object-fit: cover;
        }

        .profile .name {
            font-size: 24px;
            margin-bottom: 10px;
            background: linear-gradient(45deg, #fff, #fff);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        .profile .phone {
            font-size: 16px;
            opacity: 0.9;
            letter-spacing: 1px;
        }

        .icp-info {
            position: fixed;
            bottom: 20px;
            left: 50%;
            transform: translateX(-50%);
            color: white;
            font-size: 14px;
            text-shadow: 0 0 10px rgba(0, 0, 0, 0.3);
            background: rgba(0, 0, 0, 0.1);
            padding: 8px 16px;
            border-radius: 20px;
            user-select: none;
            z-index: 10;
        }

        .icp-info a {
            color: white;
            text-decoration: none;
            transition: all 0.3s ease;
        }

        .icp-info a:hover {
            text-shadow: 0 0 15px rgba(255, 255, 255, 0.8);
            text-decoration: underline;
        }

        /* 动画定义 */
        @keyframes float-to-top {
            0% {
                transform: translateY(100vh) rotate(0deg);
                opacity: 0;
            }
            10% {
                opacity: 1;
            }
            90% {
                transform: translateY(10px) rotate(320deg);
                opacity: 1;
            }
            100% {
                transform: translateY(10px) rotate(360deg);
                opacity: 1;
            }
        }

        @keyframes burst {
            0% {
                transform: translateY(10px) scale(1);
                opacity: 1;
            }
            50% {
                transform: translateY(10px) scale(1.3);
                opacity: 0.7;
                background: rgba(255, 255, 255, 0.3);
            }
            100% {
                transform: translateY(10px) scale(0);
                opacity: 0;
            }
        }

        @keyframes click-burst {
            0% {
                transform: scale(1);
                opacity: 1;
                background: rgba(255, 255, 255, 0.25);
            }
            30% {
                transform: scale(1.4);
                opacity: 0.8;
                background: rgba(255, 255, 255, 0.4);
                box-shadow: 0 0 30px rgba(255, 255, 255, 0.6);
            }
            100% {
                transform: scale(0);
                opacity: 0;
                background: rgba(255, 255, 255, 0);
            }
        }

        @keyframes particle-burst {
            0% {
                transform: translate(0, 0) scale(1);
                opacity: 1;
            }
            100% {
                opacity: 0;
                transform: scale(0.2);
            }
        }

        @keyframes sparkle {
            0%, 100% {
                opacity: 0;
                transform: scale(0);
            }
            50% {
                opacity: 1;
                transform: scale(1);
            }
        }

        @keyframes shoot {
            0% {
                transform: translateX(-100px) translateY(0px);
                opacity: 1;
            }
            100% {
                transform: translateX(calc(100vw + 100px)) translateY(100px);
                opacity: 0;
            }
        }

        @keyframes gradient {
            0% { background-position: 0% 50%; }
            50% { background-position: 100% 50%; }
            100% { background-position: 0% 50%; }
        }

        @keyframes pulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.1); }
            100% { transform: scale(1); }
        }

        @keyframes pulse-fast {
            0% { transform: scale(1.1); }
            50% { transform: scale(1.2); }
            100% { transform: scale(1.1); }
        }

        @keyframes firework-burst {
            0% {
                transform: translate(0, 0) scale(1);
                opacity: 1;
            }
            50% {
                opacity: 0.8;
                transform: scale(1.2);
            }
            100% {
                opacity: 0;
                transform: scale(0.1);
            }
        }

        @keyframes firework-spark {
            0% {
                transform: translate(0, 0) scale(1);
                opacity: 1;
            }
            30% {
                opacity: 1;
                transform: scale(1.5);
            }
            60% {
                opacity: 0.5;
                transform: scale(0.8);
            }
            100% {
                opacity: 0;
                transform: scale(0.2);
            }
        }

        @keyframes firework-ring {
            0% {
                transform: scale(0);
                opacity: 1;
                border-width: 3px;
            }
            50% {
                opacity: 0.6;
                border-width: 1px;
            }
            100% {
                transform: scale(3);
                opacity: 0;
                border-width: 0px;
            }
        }

        @keyframes twinkle {
            0%, 100% {
                opacity: 0.3;
                transform: scale(0.8);
            }
            50% {
                opacity: 1;
                transform: scale(1.2);
            }
        }

        @keyframes score-popup {
            0% {
                transform: translateY(0) scale(1);
                opacity: 1;
            }
            50% {
                transform: translateY(-30px) scale(1.2);
                opacity: 1;
            }
            100% {
                transform: translateY(-60px) scale(0.8);
                opacity: 0;
            }
        }

        body {
            margin: 0;
            padding: 0;
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            font-family: "Arial", sans-serif;
            background: linear-gradient(45deg, #00BFFF, #008080, #4682B4, #20B2AA);
            background-size: 400% 400%;
            animation: gradient 15s ease infinite;
            overflow: hidden;
            cursor: default;
            user-select: none;
            -webkit-user-select: none;
            -moz-user-select: none;
            -ms-user-select: none;
        }

        * {
            cursor: default !important;
            caret-color: transparent !important;
            outline: none !important;
        }

        *:focus {
            outline: none !important;
        }

        .text {
            color: white;
            font-size: calc(2vw + 20px); 
            text-transform: uppercase;
           // text-shadow:
<!--                0 0 10px rgba(255,255,255,0.5),-->
<!--                0 0 20px rgba(255,255,255,0.3),-->
<!--                0 0 40px rgba(255,255,255,0.1);-->
           // animation: pulse 2s ease-in-out infinite;
            position: relative;
            z-index: 10;
        }
    </style>
</head>
<body>
    <!-- 泡泡容器 -->
    <div class="bubbles"></div>

    <!-- 粒子容器 -->
    <div class="particles"></div>

    <!-- 流星容器 -->
    <div class="shooting-stars"></div>

    <div class="profile">
        <img src="https://static.dingtalk.com/media/lQDPD3ct_c_TbxXNAsPNAsOw0XKyPRlaiMwIQg7hBQv-AA_707_707.jpg" alt="青雀头像"/>
        <div class="name">青雀</div>
    </div>
    <div class="icp-info">
        <a href="https://beian.miit.gov.cn/" target="_blank" rel="noopener noreferrer">浙ICP备2025154122号-1</a>
    </div>

    <script>
        // 积分计数器
        let bubbleScore = 0;

        // 创建积分提示效果
        function createScorePopup(x, y, score) {
            const popup = document.createElement('div');
            popup.className = 'score-popup';
            popup.textContent = `+${score}`;
            popup.style.position = 'absolute';
            popup.style.left = x + 'px';
            popup.style.top = y + 'px';
            popup.style.color = '#FFD700';
            popup.style.fontSize = '20px';
            popup.style.fontWeight = 'bold';
            popup.style.textShadow = '0 0 10px rgba(255, 215, 0, 0.8)';
            popup.style.pointerEvents = 'none';
            popup.style.zIndex = '100';
            popup.style.animation = 'score-popup 1s ease-out forwards';
            
            document.body.appendChild(popup);
            
            // 1秒后移除积分提示
            setTimeout(() => {
                if (popup.parentNode) {
                    popup.parentNode.removeChild(popup);
                }
            }, 1000);
        }

        // 创建破裂粒子效果
        function createBurstEffect(bubble, isClick = false) {
            const rect = bubble.getBoundingClientRect();
            const centerX = rect.left + rect.width / 2;
            const centerY = rect.top + rect.height / 2;

            // 烟花颜色数组
            const colors = [
                'rgba(255, 100, 100, 0.9)', // 红色
                'rgba(100, 255, 100, 0.9)', // 绿色
                'rgba(100, 100, 255, 0.9)', // 蓝色
                'rgba(255, 255, 100, 0.9)', // 黄色
                'rgba(255, 100, 255, 0.9)', // 紫色
                'rgba(100, 255, 255, 0.9)', // 青色
                'rgba(255, 150, 100, 0.9)', // 橙色
            ];

            // 创建爆炸光环
            if (isClick) {
                const ring = document.createElement('div');
                ring.className = 'firework-ring';
                ring.style.left = (centerX - 20) + 'px';
                ring.style.top = (centerY - 20) + 'px';
                ring.style.width = '40px';
                ring.style.height = '40px';
                ring.style.animation = 'firework-ring 0.8s ease-out forwards';
                ring.style.borderColor = colors[Math.floor(Math.random() * colors.length)];

                document.body.appendChild(ring);

                setTimeout(() => {
                    if (ring.parentNode) {
                        ring.parentNode.removeChild(ring);
                    }
                }, 800);
            }

            // 主要粒子层（更多、更远）
            const mainParticleCount = isClick ? Math.floor(Math.random() * 12) + 20 : Math.floor(Math.random() * 5) + 6;

            for (let i = 0; i < mainParticleCount; i++) {
                const particle = document.createElement('div');
                particle.className = 'firework-particle';

                // 随机颜色
                const color = colors[Math.floor(Math.random() * colors.length)];
                particle.style.background = color;
                particle.style.boxShadow = `0 0 ${isClick ? 8 : 6}px ${color}`;

                // 随机方向和距离
                const angle = (Math.PI * 2 * i) / mainParticleCount + Math.random() * 0.3;
                const distance = isClick ? Math.random() * 80 + 40 : Math.random() * 30 + 20;
                const finalX = Math.cos(angle) * distance;
                const finalY = Math.sin(angle) * distance;

                particle.style.left = centerX + 'px';
                particle.style.top = centerY + 'px';
                particle.style.animation = `firework-burst ${isClick ? '1.2s' : '0.6s'} ease-out forwards`;
                particle.style.transform = `translate(${finalX}px, ${finalY}px)`;

                document.body.appendChild(particle);

                // 为每个主粒子创建次级火花
                if (isClick) {
                    setTimeout(() => {
                        const sparkCount = Math.floor(Math.random() * 3) + 2;
                        for (let j = 0; j < sparkCount; j++) {
                            const spark = document.createElement('div');
                            spark.className = 'firework-spark';

                            const sparkAngle = Math.random() * Math.PI * 2;
                            const sparkDistance = Math.random() * 20 + 10;
                            const sparkX = Math.cos(sparkAngle) * sparkDistance;
                            const sparkY = Math.sin(sparkAngle) * sparkDistance;

                            spark.style.left = (centerX + finalX) + 'px';
                            spark.style.top = (centerY + finalY) + 'px';
                            spark.style.animation = 'firework-spark 0.8s ease-out forwards';
                            spark.style.transform = `translate(${sparkX}px, ${sparkY}px)`;

                            document.body.appendChild(spark);

                            setTimeout(() => {
                                if (spark.parentNode) {
                                    spark.parentNode.removeChild(spark);
                                }
                            }, 800);
                        }
                    }, 300);
                }

                // 清理主粒子
                setTimeout(() => {
                    if (particle.parentNode) {
                        particle.parentNode.removeChild(particle);
                    }
                }, isClick ? 1200 : 600);
            }

            // 闪烁粒子层（点击时添加）
            if (isClick) {
                const twinkleCount = Math.floor(Math.random() * 8) + 10;
                for (let i = 0; i < twinkleCount; i++) {
                    const twinkle = document.createElement('div');
                    twinkle.className = 'firework-spark';

                    const color = colors[Math.floor(Math.random() * colors.length)];
                    twinkle.style.background = color;
                    twinkle.style.boxShadow = `0 0 4px ${color}`;

                    const angle = Math.random() * Math.PI * 2;
                    const distance = Math.random() * 60 + 20;
                    const finalX = Math.cos(angle) * distance;
                    const finalY = Math.sin(angle) * distance;

                    twinkle.style.left = (centerX + finalX) + 'px';
                    twinkle.style.top = (centerY + finalY) + 'px';
                    twinkle.style.animation = 'twinkle 0.5s ease-in-out infinite';

                    document.body.appendChild(twinkle);

                    setTimeout(() => {
                        if (twinkle.parentNode) {
                            twinkle.parentNode.removeChild(twinkle);
                        }
                    }, 1500);
                }
            }
        }

        // 创建泡泡效果
        function createBubbles() {
            const bubblesContainer = document.querySelector('.bubbles');

            function createBubble() {
                const bubble = document.createElement('div');
                bubble.className = 'bubble';

                // 随机大小
                const size = Math.random() * 50 + 25;
                bubble.style.width = size + 'px';
                bubble.style.height = size + 'px';

                // 随机位置
                bubble.style.left = Math.random() * (window.innerWidth - size) + 'px';

                // 随机动画时长
                const floatDuration = Math.random() * 3 + 6; // 6-9秒上浮
                bubble.style.animationDuration = floatDuration + 's';

                bubblesContainer.appendChild(bubble);

                // 添加点击事件监听器
                bubble.addEventListener('click', function(e) {
                    e.stopPropagation();
                    // 如果泡泡还没破裂，则立即破裂
                    if (!bubble.classList.contains('burst') && !bubble.classList.contains('clicked')) {
                        // 增加积分
                        bubbleScore++;
                        
                        // 在鼠标位置显示积分提示
                        createScorePopup(e.clientX, e.clientY, bubbleScore);

                        // 获取当前位置并固定
                        const currentStyle = window.getComputedStyle(bubble);
                        const matrix = new DOMMatrix(currentStyle.transform);
                        const currentX = parseFloat(bubble.style.left);
                        const currentY = matrix.m42; // translateY 值

                        // 停止当前动画并设置固定位置
                        bubble.style.animation = 'none';
                        bubble.style.left = currentX + 'px';
                        bubble.style.top = currentY + 'px';
                        bubble.style.transform = 'none';

                        // 创建破裂效果（点击版本）
                        createBurstEffect(bubble, true);

                        // 添加点击破裂动画类
                        bubble.classList.add('clicked');

                        // 破裂动画结束后移除泡泡
                        setTimeout(() => {
                            if (bubble.parentNode) {
                                bubble.parentNode.removeChild(bubble);
                            }
                        }, 300);
                    }
                });

                // 监听动画结束事件
                bubble.addEventListener('animationend', () => {
                    // 如果泡泡没有被点击破裂，则随机延时后破裂（1-4秒）
                    if (!bubble.classList.contains('clicked')) {
                        const burstDelay = Math.random() * 3000 + 1000;

                        setTimeout(() => {
                            // 创建破裂效果（自动版本）
                            createBurstEffect(bubble, false);

                            // 添加破裂动画类
                            bubble.classList.add('burst');

                            // 破裂动画结束后移除泡泡
                            setTimeout(() => {
                                if (bubble.parentNode) {
                                    bubble.parentNode.removeChild(bubble);
                                }
                            }, 300);
                        }, burstDelay);
                    }
                });

                // 备用清理机制（防止内存泄漏）
                setTimeout(() => {
                    if (bubble.parentNode) {
                        bubble.parentNode.removeChild(bubble);
                    }
                }, (floatDuration + 8) * 1000);
            }

            // 提高气泡出现频率：从1200ms改为600ms
            setInterval(createBubble, 600);

            // 初始创建更多泡泡：从5个增加到8个
            for (let i = 0; i < 8; i++) {
                setTimeout(createBubble, i * 400);
            }
        }

        // 创建粒子效果
        function createParticles() {
            const particlesContainer = document.querySelector('.particles');

            function createParticle() {
                const particle = document.createElement('div');
                particle.className = 'particle';

                // 随机大小
                const size = Math.random() * 4 + 1;
                particle.style.width = size + 'px';
                particle.style.height = size + 'px';

                // 随机位置
                particle.style.left = Math.random() * 100 + '%';
                particle.style.top = Math.random() * 100 + '%';

                // 随机动画时长
                particle.style.animationDuration = (Math.random() * 2 + 2) + 's';
                particle.style.animationDelay = Math.random() * 3 + 's';

                particlesContainer.appendChild(particle);

                // 动画结束后移除
                setTimeout(() => {
                    if (particle.parentNode) {
                        particle.parentNode.removeChild(particle);
                    }
                }, 6000);
            }

            // 每隔一段时间创建新粒子
            setInterval(createParticle, 300);

            // 初始创建一些粒子
            for (let i = 0; i < 15; i++) {
                setTimeout(createParticle, i * 200);
            }
        }

        // 创建流星效果
        function createShootingStars() {
            const starsContainer = document.querySelector('.shooting-stars');

            function createShootingStar() {
                const star = document.createElement('div');
                star.className = 'shooting-star';

                // 随机起始位置
                star.style.left = '-100px';
                star.style.top = Math.random() * 50 + '%';

                // 随机动画时长
                star.style.animationDuration = (Math.random() * 2 + 1) + 's';
                star.style.animationDelay = Math.random() * 5 + 's';

                starsContainer.appendChild(star);

                // 动画结束后移除
                setTimeout(() => {
                    if (star.parentNode) {
                        star.parentNode.removeChild(star);
                    }
                }, 8000);
            }

            // 每隔一段时间创建新流星
            setInterval(createShootingStar, 3000);

            // 初始创建一颗流星
            setTimeout(createShootingStar, 2000);
        }

        // 启动所有动态效果
        createBubbles();
        createParticles();
        createShootingStars();
    </script>
</body>
</html>
