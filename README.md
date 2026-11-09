<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>アイヌ文化 ミニテスト</title>

    <style>
        * {
            box-sizing: border-box;
            user-select: none;
            -webkit-user-select: none;
        }

        body {
            font-family: "Hiragino Kaku Gothic Pro", "Meiryo", sans-serif;
            background-color: #e0f7fa;
            color: #333;
            margin: 0;
            padding: 10px;
            display: flex;
            flex-direction: column;
            align-items: center;
            min-height: 100vh;
        }

        #app-container {
            background-color: #ffffff;
            border-radius: 20px;
            box-shadow: 0 10px 25px rgba(0,0,0,0.1);
            width: 100%;
            max-width: 900px;
            padding: 20px;
            text-align: center;
            position: relative;
            min-height: 90vh;
            display: flex;
            flex-direction: column;
            justify-content: space-between;
        }

        ruby { ruby-align: center; }
        rt { font-size: 0.55em; color: #00695c; font-weight: bold; }

        .title {
            font-size: 28px;
            font-weight: bold;
            color: #00838f;
            margin-bottom: 10px;
        }

        #touch-screen-area {
            flex: 1;
            display: flex;
            flex-direction: column;
            justify-content: space-between;
            cursor: pointer;
            border: 4px dashed transparent;
            border-radius: 15px;
            padding: 10px;
            transition: background-color 0.2s, border-color 0.2s;
        }
        
        #touch-screen-area.active-touch {
            background-color: #fff9c4 !important;
            border-color: #fbc02d !important;
        }

        .question-box {
            background-color: #f4fbfb;
            border: 3px solid #b2f2e8;
            border-radius: 15px;
            padding: 15px;
            margin-bottom: 15px;
            text-align: left;
        }

        .question-text {
            font-size: 24px;
            font-weight: bold;
            line-height: 1.5;
            color: #222;
        }

        .choices-container {
            display: flex;
            flex-direction: column;
            gap: 15px;
            margin-bottom: 15px;
        }

        .choice-card {
            border-radius: 15px;
            padding: 15px;
            text-align: left;
            font-weight: bold;
            transition: all 0.2s ease;
            opacity: 0.6;
            border: 5px solid transparent;
            display: flex;
            align-items: center;
            justify-content: space-between;
            background-color: #f5f5f5;
        }

        .choice-touch { background-color: #e8f5e9; color: #1b5e20; border-color: #a5d6a7; }
        .choice-wait { background-color: #fff3e0; color: #e65100; border-color: #ffe0b2; }

        .choice-card.speaking {
            opacity: 1.0 !important;
            box-shadow: 0 0 20px rgba(255, 215, 0, 0.9) !important;
            border-color: #ffd700 !important;
            background-color: #fffde7 !important;
        }

        .choice-card.active {
            opacity: 1.0;
            transform: scale(1.02);
            box-shadow: 0 0 20px rgba(0, 150, 136, 0.4);
        }
        .choice-touch.active { border-color: #2e7d32; }
        .choice-wait.active { border-color: #ef6c00; }

        .badge {
            display: inline-block;
            padding: 6px 12px;
            border-radius: 8px;
            font-size: 18px;
            color: white;
            margin-bottom: 5px;
        }
        .choice-touch .badge { background-color: #4caf50; }
        .choice-wait .badge { background-color: #ff9800; }

        .choice-content {
            flex: 1;
            padding-right: 15px;
        }

        .choice-text {
            font-size: 24px;
            line-height: 1.4;
        }

        .choice-img {
            width: 160px;
            height: 130px;
            border-radius: 10px;
            object-fit: cover;
            background-color: #fff;
            border: 2px solid #ccc;
        }

        .timer-container {
            width: 100%;
            background-color: #e0e0e0;
            border-radius: 10px;
            height: 20px;
            overflow: hidden;
            margin-top: 10px;
        }

        .timer-bar {
            width: 0%;
            height: 100%;
            background-color: #00bcd4;
            transition: width 0.1s linear;
        }

        .btn {
            border: none;
            border-radius: 15px;
            padding: 15px 30px;
            font-size: 24px;
            font-weight: bold;
            cursor: pointer;
            box-shadow: 0 5px 0 rgba(0,0,0,0.2);
            color: white;
            margin: 5px;
        }
        .btn-start { background-color: #4caf50; font-size: 28px; padding: 20px 40px; }
        .btn-utility { font-size: 16px; padding: 8px 16px; border-radius: 10px; }
        .btn-read { background-color: #2196f3; }
        .btn-clear { background-color: #e91e63; }

        #feedback {
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            font-size: 200px;
            font-weight: bold;
            display: none;
            z-index: 100;
            pointer-events: none;
        }
        .correct { color: #f44336; }
        .incorrect { color: #2196f3; }

        .result-table {
            width: 100%;
            border-collapse: collapse;
            margin: 15px 0;
            font-size: 16px;
        }
        .result-table th, .result-table td {
            border: 2px solid #b2dfdb;
            padding: 10px;
            text-align: left;
        }
        .result-table th { background-color: #e0f2f1; color: #00695c; text-align: center; }
        .text-center { text-align: center !important; }
        .status-ok { color: #d32f2f; font-weight: bold; font-size: 20px; }
        .status-ng { color: #1976d2; font-weight: bold; font-size: 20px; }

        .history-section {
            margin-top: 20px;
            border-top: 2px dashed #b2dfdb;
            padding-top: 15px;
            text-align: left;
        }

        .hidden { display: none !important; }

        .instruction-box {
            background-color: #e1f5fe;
            border: 2px solid #81d4fa;
            border-radius: 12px;
            padding: 10px;
            font-size: 18px;
            font-weight: bold;
            color: #0277bd;
            margin-bottom: 10px;
        }
    </style>
</head>
<body>

    <div id="app-container">
        <div id="screen-start">
            <div class="title">アイヌ<ruby>文化<rt>ぶんか</rt></ruby> ミニテスト</div>
            <p style="font-size: 22px; line-height: 1.6; margin: 20px 0;">
                【えらびかた】<br><br>
                👆 <b>がめんをタッチする（どこでもOK）</b><br>
                ➔ <b>みどり色のこたえ</b><br><br>
                ⏳ <b>何も触らずに 5秒待つ</b><br>
                ➔ <b>オレンジ色のこたえ</b>
            </p>
            <button class="btn btn-start" onclick="startQuiz()">スタート！</button>
        </div>

        <div id="screen-quiz" class="hidden">
            <div id="touch-screen-area">
                <div>
                    <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 5px;">
                        <span style="font-size: 20px; font-weight: bold; color: #00838f;" id="q-number"></span>
                        <button class="btn btn-utility btn-read" onclick="readQuestionAndChoices(event)">🔊 もう一度読む</button>
                    </div>
                    
                    <div class="question-box">
                        <div class="question-text" id="q-text"></div>
                    </div>

                    <div class="instruction-box" id="guide-text">
                        よみあげ中...
                    </div>
                </div>

                <div class="choices-container">
                    <div class="choice-card choice-touch" id="card-touch">
                        <div class="choice-content">
                            <div class="badge">👆 がめんをタッチする</div>
                            <div class="choice-text" id="touch-text"></div>
                        </div>
                        <div id="touch-img-box"></div>
                    </div>

                    <div class="choice-card choice-wait" id="card-wait">
                        <div class="choice-content">
                            <div class="badge">⏳ タッチせずに待つ（5秒）</div>
                            <div class="choice-text" id="wait-text"></div>
                        </div>
                        <div id="wait-img-box"></div>
                    </div>
                </div>

                <div>
                    <div class="timer-container">
                        <div id="timer-bar" class="timer-bar"></div>
                    </div>
                </div>
            </div>
        </div>

        <div id="screen-result" class="hidden">
            <div class="title">アイヌ<ruby>文化<rt>ぶんか</rt></ruby> テスト<ruby>結果<rt>けっか</rt></ruby></div>
            <div style="font-size: 18px; font-weight: bold; margin-bottom: 10px;" id="result-date"></div>
            <div style="font-size: 32px; font-weight: bold; color: #e65100; margin-bottom: 15px;" id="score-display"></div>
            <table class="result-table">
                <thead><tr><th style="width: 15%;">問題</th><th>内容</th><th style="width: 20%;">判定</th></tr></thead>
                <tbody id="current-result-body"></tbody>
            </table>
            <div style="margin: 15px 0;">
                <button class="btn btn-utility btn-start" style="font-size: 20px; padding: 12px 25px;" onclick="location.reload()">🔄 もう一度テストをする</button>
            </div>
        </div>

        <div id="feedback"></div>
    </div>

    <script>
        const audioCtx = new (window.AudioContext || window.webkitAudioContext)();
        const WAIT_TIME = 5000;

        const rawQuizData = [
            {
                title: "アイヌのあいさつ",
                q_html: "第一問 アイヌの<ruby>言葉<rt>ことば</rt></ruby>で「こんにちは」はどっち？",
                q_speech: "第一問、アイヌの言葉で、こんにちは、はどっち？",
                correct: { label: "イランカラプテ", speech: "イランカラプテ" },
                wrong:   { label: "アニョハセヨ", speech: "アニョハセヨ" }
            },
            {
                title: "アイヌの神様",
                q_html: "第二問 アイヌの<ruby>言葉<rt>ことば</rt></ruby>で「<ruby>神様<rt>かみさま</rt></ruby>」のことは何という？",
                q_speech: "第二問、アイヌの言葉で、神様、のことは何という？",
                correct: { label: "カムイ", speech: "カムイ" },
                wrong:   { label: "ゴッド", speech: "ゴッド" }
            },
            {
                title: "アイヌの家",
                q_html: "第三問 アイヌの<ruby>昔<rt>むかし</rt></ruby>の<ruby>家<rt>いえ</rt></ruby>（たてもの）の<ruby>名前<rt>なまえ</rt></ruby>は？",
                q_speech: "第三問、アイヌの昔の家（たてもの）の名前は？",
                correct: { label: "チセ", speech: "チセ" },
                wrong:   { label: "マンション", speech: "マンション" }
            },
            {
                title: "アイヌの楽器",
                q_html: "第四問 <ruby>口<rt>くち</rt></ruby>に当てて「ビヨーン」と<ruby>鳴<rt>な</rt></ruby>らすアイヌの<ruby>楽器<rt>がっき</rt></ruby>は？",
                q_speech: "第四問、口に当てて、ビヨーンと鳴らす、アイヌの楽器は？",
                correct: { label: "ムックリ", speech: "ムックリ" },
                wrong:   { label: "ピアノ", speech: "ピアノ" }
            },
            {
                title: "アイヌの料理",
                q_html: "第五問 肉や魚を細かくトントンたたいて作るアイヌの<ruby>料理<rt>りょうり</rt></ruby>は？",
                q_speech: "第五問、肉や魚を細かくトントンたたいて作る、アイヌの料理は？",
                correct: { label: "チタタプ", speech: "チタタプ" },
                wrong:   { label: "ハンバーグ", speech: "ハンバーグ" }
            },
            {
                title: "アイヌの服",
                q_html: "第六問 アイヌの<ruby>服<rt>ふく</rt></ruby>（着物）はどっちの写真？",
                q_speech: "第六問、アイヌの服はどっちの写真？",
                correct: { label: "黒い生地に独特の模様がある着物", imgSrc: "9bdf40befda56aded47d3d00bffed831-2.jpg", speech: "くろいきじに、もようがある着物" },
                wrong:   { label: "ピンク色のチマチョゴリ", imgSrc: "images.jpg", speech: "ピンクいろの、チマチョゴリ" }
            },
            {
                title: "アイヌの歌・物語",
                q_html: "第七問 アイヌの歌（物語）を表している写真はどっち？",
                q_speech: "第七問、アイヌの歌をあらわしている写真はどっち？",
                correct: { label: "アイヌの歌のアニメ", imgSrc: "images-1.jpg", speech: "アイヌのうたのアニメ" },
                wrong:   { label: "アイドルの写真", imgSrc: "images-2.jpg", speech: "アイドルの写真" }
            }
        ];

        let quizData = [];
        let currentIndex = 0;
        let answeredQuestions = [];
        let timerId = null;
        let animationId = null;
        let waitStartTime = 0;
        let isWaitingForInput = false;
        let touchAssignedToCorrect = true;
        let isSpeaking = false;

        function playSound(type) {
            if (audioCtx.state === 'suspended') audioCtx.resume();
            const osc = audioCtx.createOscillator();
            const gain = audioCtx.createGain();
            osc.connect(gain);
            gain.connect(audioCtx.destination);

            if (type === 'correct') {
                osc.type = 'sine';
                osc.frequency.setValueAtTime(880, audioCtx.currentTime);
                osc.frequency.setValueAtTime(1100, audioCtx.currentTime + 0.1);
                gain.gain.setValueAtTime(1, audioCtx.currentTime);
                gain.gain.exponentialRampToValueAtTime(0.01, audioCtx.currentTime + 0.5);
                osc.start(audioCtx.currentTime);
                osc.stop(audioCtx.currentTime + 0.5);
            } else if (type === 'incorrect') {
                osc.type = 'triangle';
                osc.frequency.setValueAtTime(300, audioCtx.currentTime);
                osc.frequency.setValueAtTime(250, audioCtx.currentTime + 0.2);
                gain.gain.setValueAtTime(1, audioCtx.currentTime);
                gain.gain.exponentialRampToValueAtTime(0.01, audioCtx.currentTime + 0.4);
                osc.start(audioCtx.currentTime);
                osc.stop(audioCtx.currentTime + 0.4);
            }
        }

        function speak(text, callback) {
            window.speechSynthesis.cancel();
            const uttr = new SpeechSynthesisUtterance(text);
            uttr.lang = 'ja-JP';
            uttr.rate = 1.0;
            uttr.pitch = 1.2;
            
            isSpeaking = true;
            uttr.onend = () => {
                isSpeaking = false;
                if (callback) callback();
            };
            window.speechSynthesis.speak(uttr);
        }

        function shuffle(array) {
            for (let i = array.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [array[i], array[j]] = [array[j], array[i]];
            }
        }

        function startQuiz() {
            if (audioCtx.state === 'suspended') audioCtx.resume();
            
            quizData = JSON.parse(JSON.stringify(rawQuizData));
            shuffle(quizData);
            
            currentIndex = 0;
            answeredQuestions = [];
            
            document.getElementById('screen-start').classList.add('hidden');
            document.getElementById('screen-quiz').classList.remove('hidden');
            
            showQuestion();
        }

        function createImgElement(src) {
            if (!src) return '';
            return `<img src="${src}" class="choice-img" onerror="this.src=''; this.alt='画像が見つかりません: ${src}'; this.style.fontSize='12px'; this.style.display='flex'; this.style.alignItems='center'; this.style.justifyContent='center';">`;
        }

        function showQuestion() {
            if (currentIndex >= quizData.length) {
                showResult();
                return;
            }

            const q = quizData[currentIndex];
            document.getElementById('q-number').textContent = `問題 ${currentIndex + 1} / ${quizData.length}`;
            document.getElementById('q-text').innerHTML = q.q_html;

            touchAssignedToCorrect = Math.random() < 0.5;

            const touchData = touchAssignedToCorrect ? q.correct : q.wrong;
            const waitData = touchAssignedToCorrect ? q.wrong : q.correct;

            document.getElementById('touch-text').innerHTML = touchData.label;
            document.getElementById('wait-text').innerHTML = waitData.label;
            document.getElementById('touch-img-box').innerHTML = createImgElement(touchData.imgSrc);
            document.getElementById('wait-img-box').innerHTML = createImgElement(waitData.imgSrc);

            const cardTouch = document.getElementById('card-touch');
            const cardWait = document.getElementById('card-wait');
            
            cardTouch.classList.remove('active', 'speaking');
            cardWait.classList.remove('active', 'speaking');
            document.getElementById('touch-screen-area').classList.remove('active-touch');

            const guide = document.getElementById('guide-text');
            guide.textContent = "🔊 よみあげ中...";
            guide.style.backgroundColor = "#e1f5fe";
            guide.style.borderColor = "#81d4fa";
            guide.style.color = "#0277bd";

            document.getElementById('timer-bar').style.width = '0%';
            isWaitingForInput = false;

            readQuestionAndChoices();
        }

        function readQuestionAndChoices(e) {
            if (e) e.stopPropagation();
            if (isSpeaking) window.speechSynthesis.cancel();
            
            const q = quizData[currentIndex];
            const touchData = touchAssignedToCorrect ? q.correct : q.wrong;
            const waitData = touchAssignedToCorrect ? q.wrong : q.correct;

            const cardTouch = document.getElementById('card-touch');
            const cardWait = document.getElementById('card-wait');
            
            isWaitingForInput = false;
            if (animationId) cancelAnimationFrame(animationId);
            document.getElementById('timer-bar').style.width = '0%';

            cardTouch.classList.remove('speaking');
            cardWait.classList.remove('speaking');
            document.getElementById('guide-text').textContent = "🔊 よみあげ中...";

            speak(q.q_speech, () => {
                cardTouch.classList.add('speaking');
                speak("タッチするこたえは、" + touchData.speech, () => {
                    cardTouch.classList.remove('speaking');
                    cardWait.classList.add('speaking');
                    speak("待つこたえは、" + waitData.speech, () => {
                        cardWait.classList.remove('speaking');
                        startInputPhase();
                    });
                });
            });
        }

        function startInputPhase() {
            isWaitingForInput = true;
            waitStartTime = performance.now();
            
            const guide = document.getElementById('guide-text');
            guide.innerHTML = "🏁 よみあげがおわりました。<br>タッチするか、何もしないで5秒待つか、選んでね！";
            guide.style.backgroundColor = "#fff3e0";
            guide.style.borderColor = "#ffb74d";
            guide.style.color = "#e65100";

            function updateTimer(currentTime) {
                if (!isWaitingForInput) return;
                
                const elapsed = currentTime - waitStartTime;
                const percentage = Math.min((elapsed / WAIT_TIME) * 100, 100);
                
                document.getElementById('timer-bar').style.width = percentage + '%';
                
                if (elapsed >= WAIT_TIME) {
                    handleChoice('wait');
                } else {
                    animationId = requestAnimationFrame(updateTimer);
                }
            }
            animationId = requestAnimationFrame(updateTimer);
        }

        document.getElementById('touch-screen-area').addEventListener('mousedown', function(e) {
            if (!isWaitingForInput) return;
            if (e.target.closest('.btn-utility')) return;
            
            this.classList.add('active-touch');
            setTimeout(() => this.classList.remove('active-touch'), 200);
            
            handleChoice('touch');
        });
        
        document.getElementById('touch-screen-area').addEventListener('touchstart', function(e) {
            if (!isWaitingForInput) return;
            if (e.target.closest('.btn-utility')) return;
            
            e.preventDefault();
            this.classList.add('active-touch');
            setTimeout(() => this.classList.remove('active-touch'), 200);
            
            handleChoice('touch');
        }, {passive: false});

        function handleChoice(action) {
            if (!isWaitingForInput) return;
            isWaitingForInput = false;
            
            if (animationId) cancelAnimationFrame(animationId);
            window.speechSynthesis.cancel();
            
            let isCorrect = false;
            let userAnswerLabel = "";
            const q = quizData[currentIndex];

            if (action === 'touch') {
                document.getElementById('card-touch').classList.add('active');
                isCorrect = touchAssignedToCorrect;
                userAnswerLabel = touchAssignedToCorrect ? q.correct.label : q.wrong.label;
            } else {
                document.getElementById('card-wait').classList.add('active');
                isCorrect = !touchAssignedToCorrect;
                userAnswerLabel = (!touchAssignedToCorrect) ? q.correct.label : q.wrong.label;
            }

            answeredQuestions.push({
                question: q.title,
                correctAnswer: q.correct.label,
                userAnswer: userAnswerLabel,
                userIsCorrect: isCorrect
            });

            const fb = document.getElementById('feedback');
            fb.style.display = 'block';
            fb.textContent = isCorrect ? '〇' : '×';
            fb.className = isCorrect ? 'correct' : 'incorrect';
            
            playSound(isCorrect ? 'correct' : 'incorrect');

            setTimeout(() => {
                fb.style.display = 'none';
                currentIndex++;
                showQuestion();
            }, 1500);
        }

        function showResult() {
            document.getElementById('screen-quiz').classList.add('hidden');
            document.getElementById('screen-result').classList.remove('hidden');

            const now = new Date();
            document.getElementById('result-date').textContent = `${now.getFullYear()}年${now.getMonth()+1}月${now.getDate()}日 ${now.getHours()}:${now.getMinutes().toString().padStart(2, '0')}`;

            let correctCount = answeredQuestions.filter(q => q.userIsCorrect).length;
            document.getElementById('score-display').textContent = `${quizData.length}問中、${correctCount}問正解！`;

            const tbody = document.getElementById('current-result-body');
            tbody.innerHTML = '';
            
            answeredQuestions.forEach((q, idx) => {
                const tr = document.createElement('tr');
                tr.innerHTML = `
                    <td class="text-center">第${idx+1}問</td>
                    <td>${q.question}</td>
                    <td class="text-center ${q.userIsCorrect ? 'status-ok' : 'status-ng'}">${q.userIsCorrect ? '〇' : '×'}</td>
                `;
                tbody.appendChild(tr);
            });
        }
    </script>
</body>
</html>
