<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>무지개 빤짝과 폭죽</title>
    <style>
        body {
            display: flex;
            justify-content: center; /* 가로 중앙 정렬 */
            align-items: center; /* 세로 중앙 정렬 */
            height: 100vh; /* 화면 전체 높이 */
            margin: 0; /* 기본 여백 제거 */
            position: relative; /* 상대적 위치 설정 */
            overflow: hidden; /* 폭죽이 화면 밖으로 나가는 것을 방지 */
        }
        button {
            padding: 10px 20px; /* 버튼 크기 조정 */
            font-size: 16px; /* 글자 크기 조정 */
            cursor: pointer; /* 마우스 커서 변경 */
        }
        #volumeControl, #pitchControl {
            position: absolute; /* 절대 위치 */
            top: 20px; /* 위쪽에서 20px */
            width: 150px; /* 슬라이더 폭 조정 */
        }
        #volumeControl {
            left: 20px; /* 왼쪽에서 20px */
        }
        #pitchControl {
            left: 180px; /* 볼륨 슬라이더 옆에 위치 */
        }
        #volumeLabel, #speedLabel {
            position: absolute; /* 절대 위치 */
            font-size: 16px; /* 글자 크기 조정 */
        }
        #volumeLabel {
            top: 50px; /* 볼륨 레이블 위치 */
            left: 20px; /* 왼쪽에서 20px */
        }
        #speedLabel {
            top: 50px; /* 속도 레이블 위치 */
            left: 180px; /* 볼륨 슬라이더 옆에 위치 */
        }
        canvas {
            position: absolute; /* 폭죽을 그리기 위한 캔버스 */
            top: 0;
            left: 0;
            pointer-events: none; /* 캔버스 클릭 방지 */
        }
    </style>
</head>
<body>
    <button id="rainbowButton">빤짝</button>
    <input type="range" id="volumeControl" min="0" max="1" step="0.01" value="0.5">
    <span id="volumeLabel">볼륨: 50%</span>
    
    <input type="range" id="pitchControl" min="0.5" max="2" step="0.1" value="1">
    <span id="speedLabel">속도: 1x</span>

    <audio id="backgroundMusic" src="sound.mp3"></audio>
    <canvas id="fireworkCanvas"></canvas>

    <script>
        const colors = [
            [255, 0, 0],   // 빨강
            [255, 127, 0], // 주황
            [255, 255, 0], // 노랑
            [0, 255, 0],   // 초록
            [0, 0, 255],   // 파랑
            [75, 0, 130],  // 남색
            [148, 0, 211]  // 보라
        ];
        
        const button = document.getElementById('rainbowButton');
        const music = document.getElementById('backgroundMusic');
        const volumeControl = document.getElementById('volumeControl');
        const pitchControl = document.getElementById('pitchControl');
        const volumeLabel = document.getElementById('volumeLabel');
        const speedLabel = document.getElementById('speedLabel');
        const canvas = document.getElementById('fireworkCanvas');
        const ctx = canvas.getContext('2d');

        // 캔버스 크기 설정
        canvas.width = window.innerWidth;
        canvas.height = window.innerHeight;

        // 초기 볼륨 및 속도 설정
        music.volume = volumeControl.value;
        music.playbackRate = pitchControl.value; // 초기 속도 설정

        // 볼륨 조절 이벤트
        volumeControl.addEventListener('input', () => {
            music.volume = volumeControl.value;
            volumeLabel.textContent = `볼륨: ${(volumeControl.value * 100).toFixed(0)}%`; // 볼륨 백분율 표시
        });

        // 속도 조절 이벤트
        pitchControl.addEventListener('input', () => {
            music.playbackRate = pitchControl.value; // 속도 조절
            speedLabel.textContent = `속도: ${pitchControl.value}x`; // 속도 표시
        });

        // 폭죽 애니메이션 함수
        function createFirework(x, y) {
            const particles = 100; // 파티클 수
            const colors = ['#FF0000', '#FF7F00', '#FFFF00', '#00FF00', '#0000FF', '#4B0082', '#9400D3'];
            const particleArray = [];

            for (let i = 0; i < particles; i++) {
                const angle = Math.random() * 2 * Math.PI; // 무작위 각도
                const speed = Math.random() * 4 + 1; // 무작위 속도
                const particle = {
                    x: x,
                    y: y,
                    vx: Math.cos(angle) * speed,
                    vy: Math.sin(angle) * speed,
                    life: 0,
                    maxLife: Math.random() * 20 + 20,
                    color: colors[Math.floor(Math.random() * colors.length)],
                };
                particleArray.push(particle);
            }

            // 파티클 애니메이션
            animateParticles(particleArray);
        }

        // 파티클 애니메이션
        function animateParticles(particles) {
            ctx.clearRect(0, 0, canvas.width, canvas.height); // 캔버스 초기화
            for (let i = 0; i < particles.length; i++) {
                const particle = particles[i];

                ctx.fillStyle = particle.color;
                ctx.beginPath();
                ctx.arc(particle.x, particle.y, 3, 0, Math.PI * 2);
                ctx.fill();

                // 파티클 업데이트
                particle.x += particle.vx;
                particle.y += particle.vy;
                particle.life++;

                // 파티클이 사라질 때
                if (particle.life >= particle.maxLife) {
                    particles.splice(i, 1);
                    i--;
                }
            }

            if (particles.length > 0) {
                requestAnimationFrame(() => animateParticles(particles));
            }
        }

        // 색깔 보간
        function interpolateColors(startColor, endColor, factor) {
            const result = startColor.map((start, index) => {
                return Math.round(start + factor * (endColor[index] - start));
            });
            return `rgb(${result[0]}, ${result[1]}, ${result[2]})`;
        }

        // 색이 자연스럽게 변하는 함수
        function changeRainbowColors() {
            let index = 0;
            let step = 0;

            const changeColor = () => {
                const nextIndex = (index + 1) % colors.length;
                const color = interpolateColors(colors[index], colors[nextIndex], step);
                document.body.style.backgroundColor = color;

                step += 0.01; // 보간 단계 증가
                if (step >= 1) {
                    step = 0;
                    index = nextIndex; // 다음 색으로 이동
                }
            };

            return setInterval(changeColor, 50); // 50ms마다 색 변경
        }

        button.addEventListener('click', () => {
            let duration = 15000; // 15초
            let colorInterval = changeRainbowColors();

            // 음악 재생
            music.currentTime = 0; // 노래 시작 부분으로 이동
            music.play(); // 음악 재생

            // 폭죽 생성
            createFirework(window.innerWidth / 2, window.innerHeight / 2);

            // 음악이 끝나면 무지개 효과 종료
            music.addEventListener('ended', () => {
                clearInterval(colorInterval);
                document.body.style.backgroundColor = ''; // 원래 색으로 복구
                ctx.clearRect(0, 0, canvas.width, canvas.height); // 폭죽 지우기
            });

            // 음악이 끝나기 전에 무지개 효과 종료
            setTimeout(() => {
                clearInterval(colorInterval);
                document.body.style.backgroundColor = ''; // 원래 색으로 복구
                ctx.clearRect(0, 0, canvas.width, canvas.height); // 폭죽 지우기
            }, duration);
        });
    </script>
</body>
</html>
