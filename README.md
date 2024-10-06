<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>무지개 빤짝</title>
    <style>
        body {
            display: flex;
            justify-content: center; /* 가로 중앙 정렬 */
            align-items: center; /* 세로 중앙 정렬 */
            height: 100vh; /* 화면 전체 높이 */
            margin: 0; /* 기본 여백 제거 */
            transition: background-color 0.5s;
            position: relative; /* 슬라이더 위치 설정을 위한 상대적 위치 지정 */
        }
        button {
            padding: 10px 20px; /* 버튼 크기 조정 */
            font-size: 16px; /* 글자 크기 조정 */
            cursor: pointer; /* 마우스 커서 변경 */
        }
        #volumeControl {
            position: absolute; /* 절대 위치 */
            top: 20px; /* 위쪽에서 20px */
            left: 20px; /* 왼쪽에서 20px */
        }
        #volumeLabel {
            position: absolute; /* 절대 위치 */
            top: 50px; /* 슬라이더 아래에 위치 */
            left: 20px; /* 왼쪽에서 20px */
            font-size: 16px; /* 글자 크기 조정 */
        }
    </style>
</head>
<body>
    <button id="rainbowButton">빤짝</button>
    <input type="range" id="volumeControl" min="0" max="1" step="0.01" value="0.5">
    <span id="volumeLabel">볼륨: 50%</span>
    <audio id="backgroundMusic" src="sound.mp3"></audio>

    <script>
        const colors = ['#FF0000', '#FF7F00', '#FFFF00', '#00FF00', '#0000FF', '#4B0082', '#9400D3'];
        const button = document.getElementById('rainbowButton');
        const music = document.getElementById('backgroundMusic');
        const volumeControl = document.getElementById('volumeControl');
        const volumeLabel = document.getElementById('volumeLabel');

        // 초기 볼륨 설정
        music.volume = volumeControl.value;

        // 볼륨 조절 이벤트
        volumeControl.addEventListener('input', () => {
            music.volume = volumeControl.value;
            volumeLabel.textContent = `볼륨: ${(volumeControl.value * 100).toFixed(0)}%`; // 볼륨 백분율 표시
        });

        button.addEventListener('click', () => {
            let duration = 3000; // 3초
            let interval = 200; // 0.2초마다 색 변경
            let currentIndex = 0;

            // 음악 재생
            music.currentTime = 0; // 노래 시작 부분으로 이동
            music.play(); // 음악 재생

            const changeColor = () => {
                document.body.style.backgroundColor = colors[currentIndex];
                currentIndex = (currentIndex + 1) % colors.length;
            };

            const colorInterval = setInterval(changeColor, interval);

            setTimeout(() => {
                clearInterval(colorInterval);
                document.body.style.backgroundColor = ''; // 원래 색으로 복구
                music.pause(); // 음악 정지
            }, duration);
        });
    </script>
</body>
</html>
