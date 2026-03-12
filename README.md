# -
立牌製作
<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <title>A4 三角立牌產生器 (含Logo & 稱謂)</title>
    <style>
        :root {
            --a4-w: 210mm;
            --a4-h: 297mm;
            --step: calc(297mm / 4);
        }

        body {
            background-color: #525659;
            display: flex;
            flex-direction: column;
            align-items: center;
            margin: 0;
            padding: 20px;
            font-family: "Microsoft JhengHei", "PingFang TC", sans-serif;
        }

        .toolbar {
            background: #fff;
            padding: 15px 25px;
            border-radius: 8px;
            margin-bottom: 20px;
            display: flex;
            gap: 20px;
            align-items: center;
            box-shadow: 0 4px 10px rgba(0,0,0,0.3);
            position: sticky;
            top: 10px;
            z-index: 1000;
        }

        .tool-item {
            display: flex;
            flex-direction: column;
            align-items: flex-start;
            gap: 5px;
        }

        .a4-page {
            width: var(--a4-w);
            height: var(--a4-h);
            background: white;
            display: flex;
            flex-direction: column;
            box-shadow: 0 0 20px rgba(0,0,0,0.5);
            box-sizing: border-box;
        }

        .row {
            height: var(--step);
            width: 100%;
            border-bottom: 1px dashed #ccc;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            position: relative;
            box-sizing: border-box;
        }

        .row:last-child { border-bottom: none; background: #fdfdfd; }

        /* 文字樣式控制 */
        .content-box { text-align: center; width: 90%; outline: none; }
        
        .school-row {
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 15px; /* Logo 與學校文字的距離 */
            margin-bottom: 8px;
        }

        .school-logo {
            max-width: 40mm; /* 限制 Logo 最大寬度 */
            max-height: 18pt; /* 限制 Logo 最大高度，與學校字體對齊 */
            display: none; /* 預設不顯示，直到上傳圖片 */
        }

        .school { font-size: 18pt; color: #666; font-weight: normal; margin: 0; }
        
        .name-row { 
            display: flex; 
            align-items: baseline; 
            justify-content: center; 
            gap: 15px; 
        }

        .name { font-size: 64pt; font-weight: bold; color: #000; }
        .title { font-size: 24pt; color: #555; font-weight: normal; }

        .upside-down { transform: rotate(180deg); }

        .label {
            position: absolute;
            left: 5mm;
            top: 5mm;
            font-size: 9pt;
            color: #ccc;
        }

        button {
            padding: 10px 15px;
            background: #007bff;
            color: white;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            font-size: 14px;
        }
        button:hover { background: #0056b3; }

        input[type="file"] {
            display: none; /* 隱藏原生檔案上傳按鈕 */
        }

        @media print {
            body { background: none; padding: 0; }
            .toolbar { display: none; }
            .a4-page { margin: 0; box-shadow: none; }
            .row { border-bottom: 1px dashed #eee; }
        }
    </style>
</head>
<body>

<div class="toolbar">
    <div class="tool-item">
        <label style="font-size:12px">姓名大小</label>
        <input type="range" min="40" max="120" value="64" oninput="resizeName(this.value)">
    </div>
    <div class="tool-item">
        <label style="font-size:12px">學校/Logo行</label>
        <button onclick="document.getElementById('logoInput').click()">上傳 Logo</button>
        <input type="file" id="logoInput" accept="image/*" onchange="loadLogo(this)">
    </div>
    <div class="tool-item">
        <label style="font-size:12px">Logo 寬度 (mm)</label>
        <input type="range" min="10" max="60" value="25" oninput="resizeLogo(this.value)">
    </div>
    <button onclick="window.print()" style="background: #28a745;">匯出 PDF / 列印</button>
    <div style="font-size: 13px; color: #888;">
        💡 點擊「上傳 Logo」並輸入文字，所有面會自動同步！
    </div>
</div>

<div class="a4-page">
    <div class="row">
        <span class="label">第一面 (摺疊後在背面)</span>
        <div class="content-box">
            <div class="school-row">
                <img src="" alt="School Logo" class="school-logo sync-logo">
                <div class="school sync-school" contenteditable="true">國立臺北護理健康大學</div>
            </div>
            <div class="name-row">
                <div class="name sync-name" contenteditable="true">鄭晴</div>
                <div class="title sync-title" contenteditable="true">研究生</div>
            </div>
        </div>
    </div>

    <div class="row upside-down">
        <span class="label" style="transform: rotate(180deg); left:auto; right:5mm;">第二面 (正面 - 倒放)</span>
        <div class="content-box">
            <div class="school-row">
                <img src="" alt="School Logo" class="school-logo sync-logo">
                <div class="school sync-school" contenteditable="true">國立臺北護理健康大學</div>
            </div>
            <div class="name-row">
                <div class="name sync-name" contenteditable="true">鄭晴</div>
                <div class="title sync-title" contenteditable="true">研究生</div>
            </div>
        </div>
    </div>

    <div class="row">
        <span class="label">第三面 (正面 - 正放)</span>
        <div class="content-box">
            <div class="school-row">
                <img src="" alt="School Logo" class="school-logo sync-logo">
                <div class="school sync-school" contenteditable="true">國立臺北護理健康大學</div>
            </div>
            <div class="name-row">
                <div class="name sync-name" contenteditable="true">鄭晴</div>
                <div class="title sync-title" contenteditable="true">研究生</div>
            </div>
        </div>
    </div>

    <div class="row">
        <span class="label">第四面 (底座)</span>
        <div style="border: 1px dashed #ddd; padding: 15px; color: #bbb;">
            此處為黏合底座
        </div>
    </div>
</div>

<script>
    // 自動同步編輯內容
    const textClasses = ['.sync-school', '.sync-name', '.sync-title'];
    
    textClasses.forEach(className => {
        const elements = document.querySelectorAll(className);
        elements.forEach(el => {
            el.addEventListener('input', () => {
                const text = el.innerText;
                document.querySelectorAll(className).forEach(other => {
                    if (other !== el) other.innerText = text;
                });
            });
        });
    });

    // Logo 上傳與同步
    function loadLogo(input) {
        if (input.files && input.files[0]) {
            const reader = new FileReader();
            reader.onload = function(e) {
                document.querySelectorAll('.sync-logo').forEach(img => {
                    img.src = e.target.result;
                    img.style.display = 'block'; // 顯示 Logo
                });
            };
            reader.readAsDataURL(input.files[0]);
        }
    }

    function resizeName(val) {
        document.querySelectorAll('.name').forEach(el => el.style.fontSize = val + 'pt');
    }

    function resizeLogo(val) {
        document.querySelectorAll('.school-logo').forEach(img => img.style.maxWidth = val + 'mm');
    }
</script>

</body>
</html>
