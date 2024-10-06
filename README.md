<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>무지개 빤짝</title>
    <style>
        body {
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            position: relative; /* 절대 위치를 위한 상대 위치 설정 */
            transition: background-color 0.2s; /* 색 변경 시 애니메이션 효과 */
        }
        button {
            padding: 10px 20px;
            font-size: 16px;
            cursor: pointer;
            margin-bottom: 20px; /* 버튼과 슬라이더 간격 */
        }
        #controls {
            position: absolute; /* 절대 위치 설정 */
            top: 20px; /* 위쪽에서 20px */
            left: 20px; /* 왼쪽에서 20px */
            display: flex; /* 가로로 배치 */
            flex-direction: column; /* 세로로 정렬 */
        }
        input[type="range"] {
            width: 300px; /* 슬라이더 폭 조정 */
            margin-bottom: 10px; /* 슬라이더 간격 */
        }
        #volumeLabel, #speedLabel {
            margin-top: 5px; /* 레이블과 슬라이더 간격 */
        }
    </style>
</head>
<body>
    <div id="controls">
        <input type="range" id="volumeControl" min="0" max="1" step="0.01" value="0.5">
        <div id="volumeLabel">볼륨: 50%</div>
        <input type="range" id="speedControl" min="0.5" max="2" step="0.1" value="1">
        <div id="speedLabel">속도: 100%</div>
    </div>
    <button id="rainbowButton">빤짝</button>
    <audio id="backgroundMusic" src="sound.mp3" preload="auto"></audio>

    <script>
        const colors = ['#FF0000', '#FF7F00', '#FFFF00', '#00FF00', '#0000FF', '#4B0082', '#9400D3'];
        let currentIndex = 0;
        let intervalId;

        const volumeControl = document.getElementById('volumeControl');
        const volumeLabel = document.getElementById('volumeLabel');
        const speedControl = document.getElementById('speedControl');
        const speedLabel = document.getElementById('speedLabel');
        const music = document.getElementById('backgroundMusic');

        // 초기 볼륨 및 속도 설정
        music.volume = volumeControl.value;
        music.playbackRate = speedControl.value;

        volumeControl.addEventListener('input', () => {
            music.volume = volumeControl.value; // 볼륨 설정
            volumeLabel.textContent = `볼륨: ${(volumeControl.value * 100).toFixed(0)}%`; // 볼륨 백분율 표시
        });

        speedControl.addEventListener('input', () => {
            music.playbackRate = speedControl.value; // 속도 설정
            speedLabel.textContent = `속도: ${(speedControl.value * 100).toFixed(0)}%`; // 속도 백분율 표시
        });

        document.getElementById('rainbowButton').addEventListener('click', () => {
            // 초기화
            clearInterval(intervalId);
            currentIndex = 0;

            // 음악 재생
            music.currentTime = 0; // 음악 시작 부분으로 이동
            music.play(); // 음악 재생

            // 15초 동안 색 변경
            intervalId = setInterval(() => {
                document.body.style.backgroundColor = colors[currentIndex];
                currentIndex = (currentIndex + 1) % colors.length;
            }, 200); // 0.2초마다 색 변경

            // 곡이 끝날 때 색 변경 중지 및 음악 정지
            music.onended = () => {
                clearInterval(intervalId);
                document.body.style.backgroundColor = ''; // 원래 색으로 복구
                music.pause(); // 음악 정지
                music.currentTime = 0; // 음악 시작 부분으로 이동
            };

            // 15초 후에 색 변경 중지
            setTimeout(() => {
                clearInterval(intervalId);
                document.body.style.backgroundColor = ''; // 원래 색으로 복구
                music.pause(); // 음악 정지
                music.currentTime = 0; // 음악 시작 부분으로 이동
            }, 15000); // 15초
        });
    </script>
</body>
</html>
