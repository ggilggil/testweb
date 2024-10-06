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
        #volumeControl {
            position: absolute; /* 절대 위치 설정 */
            top: 20px; /* 위쪽에서 20px */
            left: 20px; /* 왼쪽에서 20px */
            width: 300px; /* 슬라이더 폭 조정 */
        }
        #volumeLabel {
            position: absolute; /* 절대 위치 설정 */
            top: 50px; /* 위쪽에서 50px */
            left: 20px; /* 왼쪽에서 20px */
        }
    </style>
</head>
<body>
    <input type="range" id="volumeControl" min="0" max="1" step="0.01" value="0.5">
    <div id="volumeLabel">볼륨: 50%</div>
    <button id="rainbowButton">빤짝</button>
    <audio id="backgroundMusic" src="sound.mp3" preload="auto"></audio>

    <script>
        const colors = ['#FF0000', '#FF7F00', '#FFFF00', '#00FF00', '#0000FF', '#4B0082', '#9400D3'];
        let currentIndex = 0;
        let intervalId;

        const volumeControl = document.getElementById('volumeControl');
        const volumeLabel = document.getElementById('volumeLabel');
        const music = document.getElementById('backgroundMusic');

        // 초기 볼륨 설정
        music.volume = volumeControl.value;

        volumeControl.addEventListener('input', () => {
            music.volume = volumeControl.value; // 볼륨 설정
            volumeLabel.textContent = `볼륨: ${(volumeControl.value * 100).toFixed(0)}%`; // 볼륨 백분율 표시
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

            // 15초 후에 색 변경 중지 및 음악 정지
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
