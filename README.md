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
        #musicControls {
            position: absolute; /* 절대 위치 설정 */
            top: 20px; /* 위쪽에서 20px */
            right: 20px; /* 오른쪽에서 20px */
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

    <div id="musicControls">
        <button id="music1Button">음악 1</button>
        <button id="music2Button">음악 2</button>
        <button id="stopButton">음악 중지</button>
    </div>

    <audio id="backgroundMusic1" src="sound.mp3" preload="auto"></audio>
    <audio id="backgroundMusic2" src="sound2.mp3" preload="auto"></audio>

    <script>
        const colors = ['#FF0000', '#FF7F00', '#FFFF00', '#00FF00', '#0000FF', '#4B0082', '#9400D3'];
        let currentIndex = 0;
        let intervalId;
        let activeMusic = null; // 현재 활성화된 음악

        const volumeControl = document.getElementById('volumeControl');
        const volumeLabel = document.getElementById('volumeLabel');
        const speedControl = document.getElementById('speedControl');
        const speedLabel = document.getElementById('speedLabel');
        const music1 = document.getElementById('backgroundMusic1');
        const music2 = document.getElementById('backgroundMusic2');

        // 초기 볼륨 및 속도 설정
        music1.volume = volumeControl.value;
        music2.volume = volumeControl.value;
        music1.playbackRate = speedControl.value;
        music2.playbackRate = speedControl.value;

        volumeControl.addEventListener('input', () => {
            music1.volume = volumeControl.value; // 볼륨 설정
            music2.volume = volumeControl.value; // 볼륨 설정
            volumeLabel.textContent = `볼륨: ${(volumeControl.value * 100).toFixed(0)}%`; // 볼륨 백분율 표시
        });

        speedControl.addEventListener('input', () => {
            music1.playbackRate = speedControl.value; // 속도 설정
            music2.playbackRate = speedControl.value; // 속도 설정
            speedLabel.textContent = `속도: ${(speedControl.value * 100).toFixed(0)}%`; // 속도 백분율 표시
        });

        document.getElementById('rainbowButton').addEventListener('click', () => {
            // 초기화
            clearInterval(intervalId);
            currentIndex = 0;

            // 음악 재생
            if (activeMusic) {
                activeMusic.currentTime = 0; // 음악 시작 부분으로 이동
                activeMusic.play(); // 음악 재생
            }

            // 색 변경 간격 설정
            intervalId = setInterval(() => {
                document.body.style.backgroundColor = colors[currentIndex];
                currentIndex = (currentIndex + 1) % colors.length;
            }, 200); // 0.2초마다 색 변경

            // 곡이 끝날 때 색 변경 중지 및 음악 정지
            if (activeMusic) {
                activeMusic.onended = () => {
                    clearInterval(intervalId);
                    document.body.style.backgroundColor = ''; // 원래 색으로 복구
                    activeMusic.pause(); // 음악 정지
                    activeMusic.currentTime = 0; // 음악 시작 부분으로 이동
                };
            }
        });

        document.getElementById('music1Button').addEventListener('click', () => {
            activeMusic = music1; // 음악 1 활성화
            music2.pause(); // 음악 2 정지
            document.getElementById('rainbowButton').click(); // 빤짝 버튼 클릭
        });

        document.getElementById('music2Button').addEventListener('click', () => {
            activeMusic = music2; // 음악 2 활성화
            music1.pause(); // 음악 1 정지
            document.getElementById('rainbowButton').click(); // 빤짝 버튼 클릭
        });

        document.getElementById('stopButton').addEventListener('click', () => {
            clearInterval(intervalId);
            document.body.style.backgroundColor = ''; // 원래 색으로 복구
            if (activeMusic) {
                activeMusic.pause(); // 음악 정지
                activeMusic.currentTime = 0; // 음악 시작 부분으로 이동
            }
        });
    </script>
</body>
</html>
