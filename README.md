# kabuto_shop
```html
<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <title>カブトムシPOP（縦型）</title>
    <style>
        /* A4縦長ぴったりに収めるための印刷設定 */
        @page {
            size: A4 portrait;
            margin: 0;
        }
        * {
            box-sizing: border-box;
        }
        body {
            margin: 0;
            padding: 0;
            font-family: "Hiragino Maru Gothic ProN", "Yu Gothic UI", "Meiryo", sans-serif;
            background-color: #FFFDF0; /* 温かみのあるうすいクリームイエロー */
            -webkit-print-color-adjust: exact; /* 印刷時に背景色を強制適用 */
            print-color-adjust: exact;
        }
        .page {
            width: 210mm;
            height: 297mm;
            position: relative;
            page-break-after: always;
            overflow: hidden;
            background-color: #FFFDF0;
        }
        /* 印刷時に最後の余白ページが出ないようにする */
        .page:last-of-type {
            page-break-after: avoid;
        }

        /* 共通の元気なオレンジ点線の外枠 */
        .border-box {
            position: absolute;
            top: 12mm;
            left: 12mm;
            right: 12mm;
            bottom: 12mm;
            border: 8px dashed #E67E22; 
            border-radius: 24px;
            background-color: #FFFFFF;
        }

        /* 文字をど真ん中に配置 */
        .char-container {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            width: 100%;
            text-align: center;
            z-index: 5;
        }

        /* 遠くからでも見える特大ポップ文字 */
        .kabuto-char {
            font-size: 160pt;
            font-weight: 900;
            color: #4A2E1B; /* カブトムシの深いブラウン */
            display: inline-block;
            line-height: 1;
            /* ポップな立体感を出すための元気なオレンジの太い影 */
            text-shadow: 8px 8px 0px #F39C12; 
        }

        /* イラストの配置定義（A4の余白部分を飾る） */
        .illust {
            position: absolute;
            z-index: 2;
        }
        .illust-top-left {
            top: 15mm;
            left: 15mm;
            width: 35mm;
            height: 35mm;
        }
        .illust-top-right {
            top: 15mm;
            right: 15mm;
            width: 35mm;
            height: 35mm;
        }
        .illust-bottom-left {
            bottom: 20mm;
            left: 15mm;
            width: 35mm;
            height: 35mm;
        }
        .illust-bottom-right {
            bottom: 20mm;
            right: 15mm;
            width: 35mm;
            height: 35mm;
        }

        /* イラストに動きを出すための傾き */
        .rotate-l15 { transform: rotate(-15deg); }
        .rotate-r15 { transform: rotate(15deg); }
        .rotate-l30 { transform: rotate(-30deg); }
        .rotate-r30 { transform: rotate(30deg); }

        /* 足元のカラフルドット（どんぐりの実をイメージ） */
        .deco-dots {
            position: absolute;
            bottom: 10mm;
            left: 0;
            right: 0;
            text-align: center;
            z-index: 3;
        }
        .dot {
            display: inline-block;
            width: 14px;
            height: 14px;
            background-color: #2ECC71; /* みずみずしい緑 */
            border-radius: 50%;
            margin: 0 10px;
        }
        .dot.orange { background-color: #E67E22; }
        .dot.yellow { background-color: #F1C40F; }
    </style>
</head>
<body>

    <!-- ==========================================
         SVGイラスト定義（共通パーツとして再利用）
         ========================================== -->
    <svg style="display: none;">
        <!-- カブトムシ -->
        <g id="beetle">
            <svg viewBox="0 0 100 100">
                <!-- ツノ -->
                <path d="M 50 40 L 50 12 M 50 12 L 38 7 M 50 12 L 62 7" stroke="#4A2E1B" stroke-width="7" stroke-linecap="round" fill="none" />
                <path d="M 50 28 L 50 22 M 50 22 L 42 18 M 50 22 L 58 18" stroke="#4A2E1B" stroke-width="6" stroke-linecap="round" fill="none" />
                <!-- 足 -->
                <path d="M 30 50 Q 12 45 8 50" stroke="#4A2E1B" stroke-width="5" stroke-linecap="round" fill="none" />
                <path d="M 30 65 Q 10 65 8 70" stroke="#4A2E1B" stroke-width="5" stroke-linecap="round" fill="none" />
                <path d="M 32 80 Q 12 85 10 94" stroke="#4A2E1B" stroke-width="5" stroke-linecap="round" fill="none" />
                <path d="M 70 50 Q 88 45 92 50" stroke="#4A2E1B" stroke-width="5" stroke-linecap="round" fill="none" />
                <path d="M 70 65 Q 90 65 92 70" stroke="#4A2E1B" stroke-width="5" stroke-linecap="round" fill="none" />
                <path d="M 68 80 Q 88 85 90 94" stroke="#4A2E1B" stroke-width="5" stroke-linecap="round" fill="none" />
                <!-- 体（楕円） -->
                <ellipse cx="50" cy="65" rx="23" ry="29" fill="#5C3A21" stroke="#4A2E1B" stroke-width="5" />
                <!-- 頭 -->
                <ellipse cx="50" cy="42" rx="15" ry="11" fill="#4A2E1B" />
                <!-- 背中の縦線 -->
                <line x1="50" y1="36" x2="50" y2="94" stroke="#352013" stroke-width="4" />
                <!-- 目 -->
                <circle cx="41" cy="42" r="3.5" fill="#FFFFFF" />
                <circle cx="59" cy="42" r="3.5" fill="#FFFFFF" />
                <circle cx="41" cy="42" r="1.5" fill="#000000" />
                <circle cx="59" cy="42" r="1.5" fill="#000000" />
            </svg>
        </g>

        <!-- 葉っぱ -->
        <g id="leaf">
            <svg viewBox="0 0 100 100">
                <path d="M 50 12 C 15 32 15 75 50 88 C 85 75 85 32 50 12 Z" fill="#2ECC71" stroke="#27AE60" stroke-width="5" stroke-linejoin="round" />
                <path d="M 50 12 L 50 88" stroke="#27AE60" stroke-width="5" stroke-linecap="round" />
                <path d="M 50 38 L 28 28" stroke="#27AE60" stroke-width="4" stroke-linecap="round" />
                <path d="M 50 38 L 72 28" stroke="#27AE60" stroke-width="4" stroke-linecap="round" />
                <path d="M 50 55 L 30 46" stroke="#27AE60" stroke-width="4" stroke-linecap="round" />
                <path d="M 50 55 L 70 46" stroke="#27AE60" stroke-width="4" stroke-linecap="round" />
                <path d="M 50 72 L 32 66" stroke="#27AE60" stroke-width="4" stroke-linecap="round" />
                <path d="M 50 72 L 68 66" stroke="#27AE60" stroke-width="4" stroke-linecap="round" />
            </svg>
        </g>

        <!-- どんぐり -->
        <g id="acorn">
            <svg viewBox="0 0 100 100">
                <path d="M 25 42 C 25 18 75 18 75 42 Z" fill="#8E5A36" stroke="#5C3A21" stroke-width="5" stroke-linejoin="round" />
                <path d="M 25 42 L 75 42" stroke="#5C3A21" stroke-width="5" stroke-linecap="round" />
                <path d="M 25 42 C 25 72 50 92 50 92 C 50 92 75 72 75 42 Z" fill="#E2A76F" stroke="#5C3A21" stroke-width="5" stroke-linejoin="round" />
                <path d="M 50 22 L 50 8" stroke="#5C3A21" stroke-width="6" stroke-linecap="round" />
            </svg>
        </g>
    </svg>

    <!-- ==========================================
         各ページ（カ・ブ・ト・ム・シ）
         ========================================== -->

    <!-- 1ページ目: カ -->
    <div class="page">
        <div class="border-box">
            <!-- 左上に葉っぱ、右下にカブトムシ -->
            <div class="illust illust-top-left rotate-l15">
                <svg viewBox="0 0 100 100"><use href="#leaf" /></svg>
            </div>
            <div class="illust illust-bottom-right rotate-r15">
                <svg viewBox="0 0 100 100"><use href="#beetle" /></svg>
            </div>
            
            <div class="char-container">
                <span class="kabuto-char">カ</span>
            </div>
            
            <div class="deco-dots">
                <span class="dot"></span>
                <span class="dot orange"></span>
                <span class="dot yellow"></span>
            </div>
        </div>
    </div>

    <!-- 2ページ目: ブ -->
    <div class="page">
        <div class="border-box">
            <!-- 右上にどんぐり、左下にカブトムシ -->
            <div class="illust illust-top-right rotate-r30">
                <svg viewBox="0 0 100 100"><use href="#acorn" /></svg>
            </div>
            <div class="illust illust-bottom-left rotate-l15">
                <svg viewBox="0 0 100 100"><use href="#beetle" /></svg>
            </div>
            
            <div class="char-container">
                <span class="kabuto-char">ブ</span>
            </div>
            
            <div class="deco-dots">
                <span class="dot yellow"></span>
                <span class="dot"></span>
                <span class="dot orange"></span>
            </div>
        </div>
    </div>

    <!-- 3ページ目: ト -->
    <div class="page">
        <div class="border-box">
            <!-- 左上にカブトムシ、右下に葉っぱ -->
            <div class="illust illust-top-left rotate-l30">
                <svg viewBox="0 0 100 100"><use href="#beetle" /></svg>
            </div>
            <div class="illust illust-bottom-right rotate-r15">
                <svg viewBox="0 0 100 100"><use href="#leaf" /></svg>
            </div>
            
            <div class="char-container">
                <span class="kabuto-char">ト</span>
            </div>
            
            <div class="deco-dots">
                <span class="dot orange"></span>
                <span class="dot yellow"></span>
                <span class="dot"></span>
            </div>
        </div>
    </div>

    <!-- 4ページ目: ム -->
    <div class="page">
        <div class="border-box">
            <!-- 右上に葉っぱ、左下にどんぐり -->
            <div class="illust illust-top-right rotate-r15">
                <svg viewBox="0 0 100 100"><use href="#leaf" /></svg>
            </div>
            <div class="illust illust-bottom-left rotate-l15">
                <svg viewBox="0 0 100 100"><use href="#acorn" /></svg>
            </div>
            
            <div class="char-container">
                <span class="kabuto-char">ム</span>
            </div>
            
            <div class="deco-dots">
                <span class="dot"></span>
                <span class="dot orange"></span>
                <span class="dot yellow"></span>
            </div>
        </div>
    </div>

    <!-- 5ページ目: シ -->
    <div class="page">
        <div class="border-box">
            <!-- 左上に葉っぱ、右上にどんぐり、右下にカブトムシ -->
            <div class="illust illust-top-left rotate-l15">
                <svg viewBox="0 0 100 100"><use href="#leaf" /></svg>
            </div>
            <div class="illust illust-top-right rotate-r15">
                <svg viewBox="0 0 100 100"><use href="#acorn" /></svg>
            </div>
            <div class="illust illust-bottom-right rotate-r30">
                <svg viewBox="0 0 100 100"><use href="#beetle" /></svg>
            </div>
            
            <div class="char-container">
                <span class="kabuto-char">シ</span>
            </div>
            
            <div class="deco-dots">
                <span class="dot yellow"></span>
                <span class="dot"></span>
                <span class="dot orange"></span>
            </div>
        </div>
    </div>

</body>
</html>

```
