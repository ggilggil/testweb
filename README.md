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
        #skipButton {
            display: none; /* 처음에는 숨김 */
            position: absolute; /* 절대 위치 설정 */
            top: 20px; /* 위쪽에서 20px */
            left: 20px; /* 왼쪽에서 20px */
        }
        #rebroadcastButton {
            display: none; /* 처음에는 숨김 */
            position: absolute; /* 절대 위치 설정 */
            bottom: 20px; /* 아래쪽에서 20px */
            right: 20px; /* 오른쪽에서 20px */
        }
        input[type="range"] {
            width: 300px; /* 슬라이더 폭 조정 */
            margin-bottom: 10px; /* 슬라이더 간격 */
        }
        #volumeLabel, #speedLabel {
            margin-top: 5px; /* 레이블과 슬라이더 간격 */
        }
        .hidden {
            display: none; /* 숨김 클래스 */
        }
        video {
            display: none; /* 처음에는 비디오 숨김 */
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

    <button id="skipButton">스킵</button>
    <button id="rebroadcastButton">다시보기</button>

    <audio id="backgroundMusic1" src="sound.mp3" preload="auto"></audio>
    <audio id="backgroundMusic2" src="sound2.mp3" preload="auto"></audio>
    <audio id="tragicMusic" src="tragic.mp3" preload="auto"></audio>
    <video id="tragicVideo" src="tragic.mp4" preload="auto"></video>

    <script>
        const colors = ['#FF0000', '#FF7F00', '#FFFF00', '#00FF00', '#0000FF', '#4B0082', '#9400D3'];
        let currentIndex = 0;
        let intervalId;
        let activeMusic = null; // 현재 활성화된 음악
        let music1Played = false; // 음악 1이 재생되었는지
        let music2Played = false; // 음악 2가 재생되었는지

        const volumeControl = document.getElementById('volumeControl');
        const volumeLabel = document.getElementById('volumeLabel');
        const speedControl = document.getElementById('speedControl');
        const speedLabel = document.getElementById('speedLabel');
        const music1 = document.getElementById('backgroundMusic1');
        const music2 = document.getElementById('backgroundMusic2');
        const skipButton = document.getElementById('skipButton');
        const rebroadcastButton = document.getElementById('rebroadcastButton');
        const tragicMusic = document.getElementById('tragicMusic');
        const tragicVideo = document.getElementById('tragicVideo');

        // 초기 볼륨 및 속도 설정
        music1.volume = volumeControl.value;
        music2.volume = volumeControl.value;
        tragicMusic.volume = volumeControl.value; // 비극 음악 볼륨 설정
        music1.playbackRate = speedControl.value;
        music2.playbackRate = speedControl.value;

        volumeControl.addEventListener('input', () => {
            music1.volume = volumeControl.value; // 볼륨 설정
            music2.volume = volumeControl.value; // 볼륨 설정
            tragicMusic.volume = volumeControl.value; // 비극 음악 볼륨 설정
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

            // 색 변경 간격 설정
            intervalId = setInterval(() => {
                document.body.style.backgroundColor = colors[currentIndex];
                currentIndex = (currentIndex + 1) % colors.length;
            }, 200); // 0.2초마다 색 변경
        });

        document.getElementById('music1Button').addEventListener('click', () => {
            if (activeMusic !== music1) { // 현재 음악이 음악 1이 아닐 때만 실행
                music1Played = true; // 음악 1 재생됨
                checkTragic(); // 비극 상태 확인
            }
        });

        document.getElementById('music2Button').addEventListener('click', () => {
            if (activeMusic !== music2) { // 현재 음악이 음악 2가 아닐 때만 실행
                music2Played = true; // 음악 2 재생됨
                checkTragic(); // 비극 상태 확인
            }
        });

        function checkTragic() {
            if (music1Played && music2Played) {
                // 두 음악을 모두 듣고 나면
                document.body.style.transition = "background-color 0.7s";
                document.body.style.backgroundColor = "black";
                setTimeout(() => {
                    tragicVideo.style.display = 'block'; // 비극 비디오 보이기
                    tragicMusic.play(); // 비극 음악 재생
                    tragicVideo.play(); // 비극 비디오 재생
                    hideButtons(); // 모든 버튼 숨김
                }, 700);
            }
        }

        function hideButtons() {
            const allButtons = document.querySelectorAll('button');
            allButtons.forEach(button => {
                if (button.id !== 'skipButton') {
                    button.classList.add('hidden'); // 숨김
                }
            });
            skipButton.style.display = 'block'; // 스킵 버튼 보이기
        }

        skipButton.addEventListener('click', () => {
            document.body.style.transition = "background-color 0.2s";
            document.body.style.backgroundColor = "white"; // 잠시 밝아짐
            setTimeout(() => {
                document.body.style.backgroundColor = ''; // 원래 색으로 복구
                showButtons(); // 모든 버튼 다시 보이기
                tragicVideo.style.display = 'none'; // 비극 비디오 숨기기
                tragicMusic.pause(); // 비극 음악 정지
                tragicMusic.currentTime = 0; // 비극 음악 시작 부분으로 이동
            }, 200);
        });

        function showButtons() {
            const allButtons = document.querySelectorAll('button');
            allButtons.forEach(button => {
                button.classList.remove('hidden'); // 보이기
            });
            rebroadcastButton.style.display = music1Played && music2Played ? 'block' : 'none'; // 다시보기 버튼 보이기
        }

        rebroadcastButton.addEventListener('click', () => {
            tragicVideo.style.display = 'block'; // 비극 비디오 보이기
            tragicVideo.play(); // 비극 비디오 재생
            tragicMusic.play(); // 비극 음악 재생
            hideButtons(); // 모든 버튼 숨김
        });

        tragicVideo.onended = () => {
            document.body.style.transition = "background-color 0.3s";
            document.body.style.backgroundColor = "white"; // 잠시 밝아짐
            setTimeout(() => {
                document.body.style.backgroundColor = ''; // 원래 색으로 복구
                showButtons(); // 모든 버튼 다시 보이기
            }, 300);
        };
    </script>
</body>
</html>
