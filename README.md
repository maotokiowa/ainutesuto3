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
        <!-- スタート画面 -->
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

        <!-- クイズ画面 -->
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

                <!-- 2択 -->
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

        <!-- 結果画面 -->
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
                q_html: "第一問 アイヌの<ruby>言葉<rt>ことば</rt></ruby>で「あなたのこころにそっとふれさせてください。」という<ruby>優<rt>やさ</rt></ruby>しいあいさつは、どっち？",
                q_speech: "第一問、アイヌのことばで、あなたのこころにそっとふれさせてください、という優しいあいさつは、どっち？",
                correct: { label: "イランカラプテ", speech: "イランカラプテ" },
                wrong:   { label: "アロハ", speech: "アロハ" }
            },
            {
                title: "アイヌが暮らしていた場所",
                q_html: "第二問 アイヌの<ruby>人<rt>ひと</rt></ruby>たちが、むかしから<ruby>暮<rt>く</rt></ruby>らしていたのはどこ？",
                q_speech: "第二問、アイヌのひとたちが、むかしからくらしていたのはどこ？",
                correct: { label: "<ruby>北海道<rt>ほっかいどう</rt></ruby>", speech: "ほっかいどう" },
                wrong:   { label: "アメリカ", speech: "アメリカ" }
            },
            {
                title: "アイヌの温かい汁物",
                q_html: "第三問 さけや<ruby>野菜<rt>やさい</rt></ruby>など、ちかくでとれるものをつかって<ruby>作<rt>つく</rt></ruby>る、<ruby>温<rt>あたた</rt></ruby>かい<ruby>汁物<rt>しるもの</rt></ruby>はどれでしょう？",
                q_speech: "第三問、さけや野菜など、ちかくでとれるものをつかってつくる、あたたかいしるものはどれでしょう？",
                correct: { label: "オハウ", speech: "オハウ" },
                wrong:   { label: "ラーメン", speech: "ラーメン" }
            },
            {
                title: "自然への感謝の呼び方",
                q_html: "第四問 アイヌの<ruby>人<rt>ひと</rt></ruby>たちは<ruby>動物<rt>どうぶつ</rt></ruby>や<ruby>自然<rt>しぜん</rt></ruby>などを、<ruby>何<rt>なに</rt></ruby>と<ruby>呼<rt>よ</rt></ruby>んで<ruby>感謝<rt>かんしゃ</rt></ruby>しましたか？",
                q_speech: "第四問、アイヌのひとたちは動物や自然などを、何と呼んで感謝しましたか？",
