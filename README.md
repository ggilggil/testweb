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
            width: 150px; /* 슬라이더 폭 조정 */
        }
        #pitchControl {
            position: absolute; /* 절대 위치 */
            top: 20px; /* 위쪽에서 20px */
            left: 180px; /* 볼륨 슬라이더 옆에 위치 */
            width: 150px; /* 슬라이더 폭 조정 */
        }
        #volumeLabel, #pitchLabel {
            position: absolute; /* 절대 위치 */
            font-size: 16px; /* 글자 크기 조정 */
        }
        #volumeLabel {
            top: 50px; /* 볼륨 레이블 위치 */
            left: 20px; /* 왼쪽에서 20px */
        }
        #pitchLabel {
            top: 50px; /* 음높이 레이블 위치 */
            left: 180px; /* 볼륨 슬라이더 옆에 위치 */
        }
    </style>
</head>
<body>
    <button id="rainbowButton">빤짝</button>
    <input type="range" id="volumeControl" min="0" max="1" step="0.01" value="0.5">
    <span id="volumeLabel">볼륨: 50%</span>
    
    <input type="range" id="pitchControl" min="0.5" max="2" step="0.1" value="1">
    <span id="pitchLabel">음높이: 1x</span>

    <audio id="backgroundMusic" src="sound.mp3"></audio>

    <script>
        const colors = ['#FF0000', '#FF7F00', '#FFFF00', '#00FF00', '#0000FF', '#4B0082', '#9400D3'];
        const button = document.getElementById('rainbowButton');
        const music = document.getElementById('backgroundMusic');
        const volumeControl = document.getElementById('volumeControl');
        const pitchControl = document.getElementById('pitchControl');
        const volumeLabel = document.getElementById('volumeLabel');
        const pitchLabel = document.getElementById('pitchLabel');

        // 초기 볼륨 및 음높이 설정
        music.volume = volumeControl.value;
        music.playbackRate = pitchControl.value; // 초기 음높이 설정

        // 볼륨 조절 이벤트
        volumeControl.addEventListener('input', () => {
            music.volume = volumeControl.value;
            volumeLabel.textContent = `볼륨: ${(volumeControl.value * 100).toFixed(0)}%`; // 볼륨 백분율 표시
        });

        // 음높이 조절 이벤트
        pitchControl.addEventListener('input', () => {
            music.playbackRate = pitchControl.value; // 음높이 조절
            pitchLabel.textContent = `음높이: ${pitchControl.value}x`; // 음높이 표시
        });

        button.addEventListener('click', () => {
            let duration = 15000; // 15초
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
