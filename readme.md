---
title: 選擇題測驗卷網站講義（學生版）.md

---

---
title: 選擇題測驗卷網站講義（學生版）

---

---
title: 選擇題測驗卷網站講義（學生版）
tags: [114程式設計與實習_上學期]

---

# 選擇題測驗卷網站講義（學生版）

學號：415730323　　姓名：謝宜軒

> **填寫方式**
> 1. 每個學習都要放：**執行截圖**、**三次問 AI 的提示詞**、**最後採用的程式碼**。
> 2. 問 AI 的提示詞請**逐字貼上**自己實際輸入的內容（不要寫摘要），第一次、第二次、第三次依序記錄。
> 3. 程式碼貼在「點開貼上」的收合區塊裡，貼上**你最後真正採用、而且能執行**的版本。

---

## 學習1：產生一個選擇題測驗卷網站

https://cfchen58.synology.me/115/week4/stage1/

**這個階段的目標：** 用 p5.js 做出一個一次顯示一題、四個選項、答完會顯示對錯與總分的測驗網站（題目先寫在程式裡）。
**這個階段會修改的檔案：** index.html、sketch.js

### 執行截圖
![261008.....1](https://hackmd.io/_uploads/S1gMpp2EiMl.gif)

![學習1截圖](請貼上截圖)

### 第一次問 AI

```tex!
使用p5.js撰寫一個選擇題測驗系統，我已經產生一個p5.js專案，請把程式碼寫到sketch.js檔案內，每條指令都需要加上中文註解，測驗系統題目設定為五題，測驗題目的內容為程式設計p5.js簡易指令練習測驗，系統採用全螢幕畫布，使用者答錯時，系統會在正確答案選項上，加上90e0ef背景顏色，該選項要上下跳動，答錯的選項採用f26a8d背景顏色，選項左右晃動，答題的選項為四個，當五題結束後需要顯示答對題數，每次顯示一個題目，需要有下一提的按鈕，題目須至中，並新增題目框框，框內背景顏色為fcf6bd
```

### 第二次問 AI

```tex!
（逐字貼上你第二次問 AI 的提示詞）
```

### 第三次問 AI

```tex!
（逐字貼上你第三次問 AI 的提示詞）
```

### 程式碼內容

:::info
:::spoiler 點開貼上學習1的程式碼
```javascript=
// 建立五題 p5.js 程式設計簡易指令測驗資料。
const questions = [
  // 建立第一題資料。
  {
    // 設定第一題題目。
    question: "在 p5.js 中，哪一個函式會在程式開始時執行一次？",

    // 設定第一題的四個選項。
    options: ["draw()", "setup()", "start()", "begin()"],

    // 設定第一題正確答案索引。
    answerIndex: 1
  },

  // 建立第二題資料。
  {
    // 設定第二題題目。
    question: "在 p5.js 中，哪一個函式會持續重複執行？",

    // 設定第二題的四個選項。
    options: ["loop()", "repeat()", "draw()", "run()"],

    // 設定第二題正確答案索引。
    answerIndex: 2
  },

  // 建立第三題資料。
  {
    // 設定第三題題目。
    question: "哪一個指令可以建立寬 400、高 300 的畫布？",

    // 設定第三題的四個選項。
    options: [
      "canvas(400, 300)",
      "createCanvas(400, 300)",
      "size(400, 300)",
      "makeCanvas(400, 300)"
    ],

    // 設定第三題正確答案索引。
    answerIndex: 1
  },

  // 建立第四題資料。
  {
    // 設定第四題題目。
    question: "哪一個指令可以設定繪圖填滿顏色？",

    // 設定第四題的四個選項。
    options: ["stroke()", "lineColor()", "fill()", "paint()"],

    // 設定第四題正確答案索引。
    answerIndex: 2
  },

  // 建立第五題資料。
  {
    // 設定第五題題目。
    question: "哪一個指令可以在畫布上畫出橢圓形？",

    // 設定第五題的四個選項。
    options: ["circle()", "ellipse()", "round()", "drawCircle()"],

    // 設定第五題正確答案索引。
    answerIndex: 1
  }
];

// 記錄目前顯示的題目編號。
let currentQuestion = 0;

// 記錄使用者選擇的選項編號。
let selectedIndex = -1;

// 記錄目前題目是否已經作答。
let answerLocked = false;

// 記錄使用者答對的題數。
let score = 0;

// 記錄測驗是否已經完成。
let quizFinished = false;

// 儲存下一題按鈕。
let nextButton;

// 儲存重新開始按鈕。
let restartButton;

// 儲存所有選項的位置。
let optionAreas = [];

// 設定題目框水平位置。
let questionBoxX;

// 設定題目框垂直位置。
let questionBoxY;

// 設定題目框寬度。
let questionBoxWidth;

// 設定題目框高度。
let questionBoxHeight;

// 設定選項起始位置。
let optionTop;

// 設定選項高度。
let optionHeight;

// 設定選項間距。
let optionGap;

// 設定提示文字位置。
let feedbackY;

// p5.js 初始化函式。
function setup() {
  // 建立全螢幕畫布。
  createCanvas(windowWidth, windowHeight);

  // 設定文字字型。
  textFont("Arial");

  // 設定文字水平與垂直置中。
  textAlign(CENTER, CENTER);

  // 設定矩形由左上角開始繪製。
  rectMode(CORNER);

  // 建立畫面按鈕。
  createButtons();

  // 設定畫面版面配置。
  updateLayout();
}

// p5.js 持續繪圖函式。
function draw() {
  // 設定畫布背景顏色。
  background("#f7f9fc");

  // 判斷測驗是否已完成。
  if (quizFinished) {
    // 顯示測驗結果。
    drawResultScreen();
  } else {
    // 顯示題目畫面。
    drawQuizScreen();
  }
}

// 更新畫面版面配置。
function updateLayout() {
  // 設定題目框寬度。
  questionBoxWidth = min(width * 0.86, 950);

  // 設定題目框高度。
  questionBoxHeight = min(height * 0.2, 150);

  // 計算題目框水平置中位置。
  questionBoxX = (width - questionBoxWidth) / 2;

  // 設定題目框垂直位置。
  questionBoxY = height * 0.19;

  // 設定選項高度。
  optionHeight = min(height * 0.068, 56);

  // 設定選項間距。
  optionGap = min(height * 0.018, 14);

  // 計算選項區域總高度。
  const optionsHeight =
    optionHeight * 4 + optionGap * 3;

  // 設定選項起始位置。
  optionTop = questionBoxY + questionBoxHeight + height * 0.045;

  // 設定提示文字位置。
  feedbackY = optionTop + optionsHeight + height * 0.035;
}

// 繪製題目作答畫面。
function drawQuizScreen() {
  // 取得目前題目資料。
  const questionData = questions[currentQuestion];

  // 設定標題文字大小。
  const titleSize = constrain(width * 0.035, 24, 42);

  // 設定題目文字大小。
  const questionSize = constrain(width * 0.027, 20, 34);

  // 設定選項文字大小。
  const optionSize = constrain(width * 0.022, 16, 25);

  // 設定選項區域寬度。
  const contentWidth = min(width * 0.86, 950);

  // 設定標題文字顏色。
  fill("#263238");

  // 關閉文字外框。
  noStroke();

  // 設定標題文字大小。
  textSize(titleSize);

  // 顯示測驗標題。
  text("p5.js 程式設計簡易指令測驗", width / 2, height * 0.065);

  // 設定題數文字顏色。
  fill("#607d8b");

  // 設定題數文字大小。
  textSize(constrain(width * 0.018, 16, 23));

  // 顯示目前題數。
  text(
    `第 ${currentQuestion + 1} 題／共 ${questions.length} 題`,
    width / 2,
    height * 0.12
  );

  // 設定題目框背景顏色。
  fill("#fcf6bd");

  // 設定題目框外框顏色。
  stroke("#e0d866");

  // 設定題目框外框粗細。
  strokeWeight(3);

  // 繪製題目框。
  rect(
    questionBoxX,
    questionBoxY,
    questionBoxWidth,
    questionBoxHeight,
    18
  );

  // 設定題目文字顏色。
  fill("#263238");

  // 關閉題目文字外框。
  noStroke();

  // 設定題目文字大小。
  textSize(questionSize);

  // 將題目文字置中並自動換行。
  drawCenteredWrappedText(
    questionData.question,
    questionBoxX + questionBoxWidth / 2,
    questionBoxY + questionBoxHeight / 2,
    questionBoxWidth * 0.88
  );

  // 清除選項位置資料。
  optionAreas = [];

  // 逐一繪製四個選項。
  for (let i = 0; i < questionData.options.length; i++) {
    // 計算選項水平位置。
    const x = (width - contentWidth) / 2;

    // 計算選項垂直位置。
    const y = optionTop + i * (optionHeight + optionGap);

    // 設定實際繪製水平位置。
    let drawX = x;

    // 設定實際繪製垂直位置。
    let drawY = y;

    // 設定選項預設背景顏色。
    let optionColor = "#ffffff";

    // 判斷答錯時的正確答案。
    if (
      answerLocked &&
      i === questionData.answerIndex &&
      selectedIndex !== questionData.answerIndex
    ) {
      // 設定正確答案背景顏色。
      optionColor = "#90e0ef";

      // 讓正確答案上下跳動。
      drawY += sin(frameCount * 0.18) * 8;
    }

    // 判斷使用者答錯的選項。
    if (
      answerLocked &&
      i === selectedIndex &&
      selectedIndex !== questionData.answerIndex
    ) {
      // 設定錯誤答案背景顏色。
      optionColor = "#f26a8d";

      // 讓錯誤答案左右晃動。
      drawX += sin(frameCount * 0.22) * 8;
    }

    // 判斷使用者答對的選項。
    if (
      answerLocked &&
      i === selectedIndex &&
      selectedIndex === questionData.answerIndex
    ) {
      // 設定答對答案背景顏色。
      optionColor = "#90e0ef";
    }

    // 儲存選項原始位置。
    optionAreas.push({
      x: x,
      y: y,
      w: contentWidth,
      h: optionHeight
    });

    // 設定選項背景顏色。
    fill(optionColor);

    // 設定選項外框顏色。
    stroke("#cfd8dc");

    // 設定選項外框粗細。
    strokeWeight(2);

    // 繪製選項框。
    rect(drawX, drawY, contentWidth, optionHeight, 14);

    // 設定選項文字顏色。
    fill("#263238");

    // 關閉選項文字外框。
    noStroke();

    // 設定選項文字大小。
    textSize(optionSize);

    // 顯示選項文字。
    text(
      `${String.fromCharCode(65 + i)}. ${questionData.options[i]}`,
      width / 2,
      drawY + optionHeight / 2
    );
  }

  // 設定答題提示文字大小。
  textSize(constrain(width * 0.021, 17, 25));

  // 判斷是否已經作答。
  if (answerLocked) {
    // 判斷答案是否正確。
    const isCorrect =
      selectedIndex === questionData.answerIndex;

    // 設定答題提示文字顏色。
    fill(isCorrect ? "#168aad" : "#d1495b");

    // 顯示答題結果，位置在四個選項下方。
    text(
      isCorrect ? "答對了！" : "答錯了，正確答案已標示。",
      width / 2,
      feedbackY
    );
  } else {
    // 設定操作提示文字顏色。
    fill("#78909c");

    // 顯示操作提示，位置在四個選項下方。
    text(
      "請點選一個答案開始作答",
      width / 2,
      feedbackY
    );
  }

  // 更新按鈕位置。
  updateButtonVisibility();
}

// 將題目文字置中並自動換行。
function drawCenteredWrappedText(
  content,
  centerX,
  centerY,
  maxWidth
) {
  // 取得換行後的文字陣列。
  const lines = getWrappedLines(content, maxWidth);

  // 計算每一行文字高度。
  const lineHeight = textSize() * 1.35;

  // 計算全部文字的總高度。
  const totalHeight = lines.length * lineHeight;

  // 計算第一行文字的垂直位置。
  const startY =
    centerY - totalHeight / 2 + lineHeight / 2;

  // 設定文字水平與垂直置中。
  textAlign(CENTER, CENTER);

  // 逐行顯示題目文字。
  for (let i = 0; i < lines.length; i++) {
    // 計算目前文字行的垂直位置。
    const lineY = startY + i * lineHeight;

    // 顯示目前文字行。
    text(lines[i], centerX, lineY);
  }
}

// 將文字依照最大寬度自動分行。
function getWrappedLines(content, maxWidth) {
  // 建立文字行陣列。
  const lines = [];

  // 建立目前文字行。
  let currentLine = "";

  // 逐字檢查題目內容。
  for (let i = 0; i < content.length; i++) {
    // 取得目前字元。
    const character = content[i];

    // 測試加入字元後的文字行。
    const testLine = currentLine + character;

    // 判斷文字是否超過最大寬度。
    if (
      textWidth(testLine) > maxWidth &&
      currentLine.length > 0
    ) {
      // 儲存目前文字行。
      lines.push(currentLine);

      // 將目前字元設定為新文字行。
      currentLine = character;
    } else {
      // 將目前字元加入文字行。
      currentLine = testLine;
    }
  }

  // 儲存最後一行文字。
  if (currentLine.length > 0) {
    lines.push(currentLine);
  }

  // 回傳換行後的文字陣列。
  return lines;
}

// 繪製測驗結果畫面。
function drawResultScreen() {
  // 設定結果標題文字顏色。
  fill("#263238");

  // 關閉文字外框。
  noStroke();

  // 設定結果標題文字大小。
  textSize(constrain(width * 0.05, 32, 60));

  // 顯示測驗完成文字。
  text("測驗完成！", width / 2, height * 0.27);

  // 設定分數文字顏色。
  fill("#168aad");

  // 設定分數文字大小。
  textSize(constrain(width * 0.06, 40, 76));

  // 顯示答對題數。
  text(
    `答對 ${score}／${questions.length} 題`,
    width / 2,
    height * 0.42
  );

  // 設定評語文字顏色。
  fill("#607d8b");

  // 設定評語文字大小。
  textSize(constrain(width * 0.022, 18, 28));

  // 顯示測驗評語。
  text(
    getResultMessage(),
    width / 2,
    height * 0.55
  );

  // 更新按鈕狀態。
  updateButtonVisibility();
}

// 根據分數取得測驗評語。
function getResultMessage() {
  // 判斷是否全部答對。
  if (score === questions.length) {
    // 回傳滿分評語。
    return "太棒了！你已經熟悉 p5.js 基礎指令！";
  }

  // 判斷是否答對三題以上。
  if (score >= 3) {
    // 回傳中高分評語。
    return "表現不錯！再練習一下就能全部答對。";
  }

  // 回傳鼓勵評語。
  return "繼續加油！建議重新練習 p5.js 基礎指令。";
}

// 建立操作按鈕。
function createButtons() {
  // 建立下一題按鈕。
  nextButton = createButton("下一題");

  // 設定下一題按鈕文字大小。
  nextButton.style("font-size", "20px");

  // 設定下一題按鈕內距。
  nextButton.style("padding", "10px 28px");

  // 設定下一題按鈕圓角。
  nextButton.style("border-radius", "10px");

  // 移除下一題按鈕外框。
  nextButton.style("border", "none");

  // 設定下一題按鈕背景顏色。
  nextButton.style("background", "#168aad");

  // 設定下一題按鈕文字顏色。
  nextButton.style("color", "#ffffff");

  // 設定滑鼠游標樣式。
  nextButton.style("cursor", "pointer");

  // 設定下一題按鈕點擊事件。
  nextButton.mousePressed(goToNextQuestion);

  // 建立重新開始按鈕。
  restartButton = createButton("重新開始");

  // 設定重新開始按鈕文字大小。
  restartButton.style("font-size", "20px");

  // 設定重新開始按鈕內距。
  restartButton.style("padding", "10px 28px");

  // 設定重新開始按鈕圓角。
  restartButton.style("border-radius", "10px");

  // 移除重新開始按鈕外框。
  restartButton.style("border", "none");

  // 設定重新開始按鈕背景顏色。
  restartButton.style("background", "#168aad");

  // 設定重新開始按鈕文字顏色。
  restartButton.style("color", "#ffffff");

  // 設定滑鼠游標樣式。
  restartButton.style("cursor", "pointer");

  // 設定重新開始按鈕點擊事件。
  restartButton.mousePressed(restartQuiz);

  // 初始隱藏下一題按鈕。
  nextButton.hide();

  // 初始隱藏重新開始按鈕。
  restartButton.hide();
}

// 更新按鈕位置與顯示狀態。
function updateButtonVisibility() {
  // 設定按鈕距離畫布底部的距離。
  const bottomMargin = 25;

  // 取得下一題按鈕寬度。
  const nextWidth = nextButton.width;

  // 取得下一題按鈕高度。
  const nextHeight = nextButton.height;

  // 取得重新開始按鈕寬度。
  const restartWidth = restartButton.width;

  // 取得重新開始按鈕高度。
  const restartHeight = restartButton.height;

  // 判斷是否為測驗結果畫面。
  if (quizFinished) {
    // 隱藏下一題按鈕。
    nextButton.hide();

    // 顯示重新開始按鈕。
    restartButton.show();

    // 計算重新開始按鈕水平位置。
    const restartX =
      width / 2 - restartWidth / 2;

    // 計算重新開始按鈕垂直位置。
    const restartY =
      height - restartHeight - bottomMargin;

    // 設定重新開始按鈕位置。
    restartButton.position(restartX, restartY);
  } else {
    // 隱藏重新開始按鈕。
    restartButton.hide();

    // 判斷目前題目是否已作答。
    if (answerLocked) {
      // 顯示下一題按鈕。
      nextButton.show();

      // 計算下一題按鈕水平位置。
      const nextX =
        width / 2 - nextWidth / 2;

      // 計算下一題按鈕垂直位置。
      const nextY =
        height - nextHeight - bottomMargin;

      // 設定下一題按鈕位置。
      nextButton.position(nextX, nextY);
    } else {
      // 尚未作答時隱藏下一題按鈕。
      nextButton.hide();
    }
  }
}

// 處理滑鼠按下事件。
function mousePressed() {
  // 將滑鼠位置交給選項判斷函式。
  handlePointer(mouseX, mouseY);
}

// 處理手機觸控事件。
function touchStarted() {
  // 將觸控位置交給選項判斷函式。
  handlePointer(mouseX, mouseY);

  // 阻止瀏覽器產生額外觸控行為。
  return false;
}

// 判斷使用者點選哪一個選項。
function handlePointer(pointerX, pointerY) {
  // 判斷測驗是否完成或目前題目已作答。
  if (quizFinished || answerLocked) {
    // 避免重複作答。
    return;
  }

  // 逐一檢查所有選項。
  for (let i = 0; i < optionAreas.length; i++) {
    // 取得目前選項位置。
    const area = optionAreas[i];

    // 判斷點擊位置是否在選項水平範圍內。
    const insideX =
      pointerX >= area.x &&
      pointerX <= area.x + area.w;

    // 判斷點擊位置是否在選項垂直範圍內。
    const insideY =
      pointerY >= area.y &&
      pointerY <= area.y + area.h;

    // 判斷使用者是否點擊目前選項。
    if (insideX && insideY) {
      // 記錄使用者選擇的選項。
      selectedIndex = i;

      // 鎖定目前題目。
      answerLocked = true;

      // 判斷答案是否正確。
      if (
        selectedIndex ===
        questions[currentQuestion].answerIndex
      ) {
        // 答對時增加分數。
        score++;
      }

      // 停止檢查其他選項。
      break;
    }
  }
}

// 前往下一題。
function goToNextQuestion() {
  // 判斷目前題目是否尚未作答。
  if (!answerLocked) {
    // 尚未作答時停止執行。
    return;
  }

  // 判斷是否還有下一題。
  if (currentQuestion < questions.length - 1) {
    // 題目編號增加一題。
    currentQuestion++;

    // 清除上一題的選擇。
    selectedIndex = -1;

    // 解鎖下一題。
    answerLocked = false;
  } else {
    // 最後一題完成後顯示結果。
    quizFinished = true;
  }
}

// 重新開始測驗。
function restartQuiz() {
  // 回到第一題。
  currentQuestion = 0;

  // 清除選項選擇。
  selectedIndex = -1;

  // 解鎖題目。
  answerLocked = false;

  // 將分數歸零。
  score = 0;

  // 設定測驗尚未完成。
  quizFinished = false;
}

// 當瀏覽器視窗大小改變時執行。
function windowResized() {
  // 重新調整畫布大小。
  resizeCanvas(windowWidth, windowHeight);

  // 重新計算畫面配置。
  updateLayout();

  // 重新設定按鈕位置。
  updateButtonVisibility();
}

```
:::


---

## 學習2：網頁設定為響應式網頁

https://cfchen58.synology.me/115/week4/stage2/

**這個階段的目標：** 讓網站在電腦、平板、手機（直向與橫向）都能正常顯示，視窗大小改變時版面自動調整。
**這個階段會修改的檔案：** index.html、sketch.js

### 執行截圖

![261008.....2](https://hackmd.io/_uploads/S1tubpNizg.gif)


![學習2截圖](請貼上截圖)

### 第一次問 AI

```tex!
讓網站在電腦、平板、手機（直向與橫向）都能正常顯示，視窗大小改變時版面自動調整。幫我修改，並生成一段程式碼
```

### 第二次問 AI

```tex!
（逐字貼上你第二次問 AI 的提示詞）
```

### 第三次問 AI

```tex!
（逐字貼上你第三次問 AI 的提示詞）
```

### 程式碼內容

:::info
:::spoiler 點開貼上學習2的程式碼
```javascript=
// 建立五題 p5.js 程式設計簡易指令測驗資料。
const questions = [
  // 建立第一題資料。
  {
    // 設定第一題題目。
    question: "在 p5.js 中，哪一個函式會在程式開始時執行一次？",

    // 設定第一題的四個選項。
    options: ["draw()", "setup()", "start()", "begin()"],

    // 設定第一題正確答案索引。
    answerIndex: 1
  },

  // 建立第二題資料。
  {
    // 設定第二題題目。
    question: "在 p5.js 中，哪一個函式會持續重複執行？",

    // 設定第二題的四個選項。
    options: ["loop()", "repeat()", "draw()", "run()"],

    // 設定第二題正確答案索引。
    answerIndex: 2
  },

  // 建立第三題資料。
  {
    // 設定第三題題目。
    question: "哪一個指令可以建立寬 400、高 300 的畫布？",

    // 設定第三題的四個選項。
    options: [
      "canvas(400, 300)",
      "createCanvas(400, 300)",
      "size(400, 300)",
      "makeCanvas(400, 300)"
    ],

    // 設定第三題正確答案索引。
    answerIndex: 1
  },

  // 建立第四題資料。
  {
    // 設定第四題題目。
    question: "哪一個指令可以設定繪圖填滿顏色？",

    // 設定第四題的四個選項。
    options: ["stroke()", "lineColor()", "fill()", "paint()"],

    // 設定第四題正確答案索引。
    answerIndex: 2
  },

  // 建立第五題資料。
  {
    // 設定第五題題目。
    question: "哪一個指令可以在畫布上畫出橢圓形？",

    // 設定第五題的四個選項。
    options: ["circle()", "ellipse()", "round()", "drawCircle()"],

    // 設定第五題正確答案索引。
    answerIndex: 1
  }
];

// 記錄目前題目編號。
let currentQuestion = 0;

// 記錄使用者選擇的選項。
let selectedIndex = -1;

// 記錄目前題目是否已經作答。
let answerLocked = false;

// 記錄答對題數。
let score = 0;

// 記錄測驗是否已完成。
let quizFinished = false;

// 儲存所有可點擊區域。
let hitAreas = [];

// 儲存目前畫面配置資料。
let layout = {};

// 儲存畫布物件。
let canvasElement;

// 儲存上一次畫布寬度。
let lastWidth = 0;

// 儲存上一次畫布高度。
let lastHeight = 0;

// 設定正確答案背景顏色。
const CORRECT_COLOR = "#90e0ef";

// 設定錯誤答案背景顏色。
const WRONG_COLOR = "#f26a8d";

// 設定題目框背景顏色。
const QUESTION_COLOR = "#fcf6bd";

// p5.js 初始化函式。
function setup() {
  // 建立全螢幕畫布並取得畫布物件。
  canvasElement = createCanvas(windowWidth, windowHeight);

  // 設定文字字型。
  textFont("Arial");

  // 設定文字水平與垂直置中。
  textAlign(CENTER, CENTER);

  // 設定矩形由左上角開始繪製。
  rectMode(CORNER);

  // 設定網頁外框距離為零。
  document.body.style.margin = "0";

  // 隱藏網頁捲軸，避免畫面出現捲動。
  document.body.style.overflow = "hidden";

  // 將畫布設定為區塊元素。
  canvasElement.style("display", "block");

  // 記錄目前畫布寬度。
  lastWidth = width;

  // 記錄目前畫布高度。
  lastHeight = height;

  // 計算響應式版面。
  updateLayout();
}

// p5.js 持續繪圖函式。
function draw() {
  // 判斷畫布大小是否改變。
  if (width !== lastWidth || height !== lastHeight) {
    // 更新畫布寬度記錄。
    lastWidth = width;

    // 更新畫布高度記錄。
    lastHeight = height;

    // 重新計算版面。
    updateLayout();
  }

  // 設定畫布背景顏色。
  background("#f7f9fc");

  // 判斷測驗是否完成。
  if (quizFinished) {
    // 顯示測驗結果畫面。
    drawResultScreen();
  } else {
    // 顯示測驗題目畫面。
    drawQuizScreen();
  }
}

// 計算響應式版面配置。
function updateLayout() {
  // 取得目前畫布寬度。
  const canvasWidth = width;

  // 取得目前畫布高度。
  const canvasHeight = height;

  // 判斷是否為矮的橫向畫面。
  const isShortLandscape =
    canvasWidth > canvasHeight && canvasHeight < 600;

  // 設定矮畫面的縮放比例。
  const compactFactor = isShortLandscape
    ? constrain(canvasHeight / 600, 0.58, 1)
    : 1;

  // 設定畫面左右邊距。
  const sidePadding = constrain(
    canvasWidth * 0.06,
    14,
    70
  );

  // 設定主要內容寬度。
  const contentWidth = min(
    canvasWidth - sidePadding * 2,
    950
  );

  // 設定標題文字大小。
  const titleSize = constrain(
    min(canvasWidth * 0.045, canvasHeight * 0.065),
    18,
    42
  ) * compactFactor;

  // 設定題數文字大小。
  const counterSize = constrain(
    min(canvasWidth * 0.025, canvasHeight * 0.038),
    13,
    23
  ) * compactFactor;

  // 設定題目文字大小。
  const questionSize = constrain(
    min(canvasWidth * 0.034, canvasHeight * 0.052),
    15,
    34
  ) * compactFactor;

  // 設定選項文字大小。
  const optionSize = constrain(
    min(canvasWidth * 0.027, canvasHeight * 0.042),
    13,
    27
  ) * compactFactor;

  // 設定提示文字大小。
  const feedbackSize = constrain(
    min(canvasWidth * 0.025, canvasHeight * 0.038),
    12,
    24
  ) * compactFactor;

  // 設定題目框高度。
  const questionBoxHeight = constrain(
    canvasHeight * 0.18 * compactFactor,
    78,
    165
  );

  // 設定標題位置。
  const titleY = max(28, canvasHeight * 0.06);

  // 設定題數位置。
  const counterY = titleY + titleSize * 0.9;

  // 設定題目框位置。
  const questionBoxY =
    counterY + counterSize * 1.25;

  // 設定選項高度。
  const optionHeight = constrain(
    canvasHeight * 0.07 * compactFactor,
    34,
    64
  );

  // 設定選項間距。
  const optionGap = constrain(
    canvasHeight * 0.016 * compactFactor,
    5,
    15
  );

  // 計算四個選項總高度。
  const optionsHeight =
    optionHeight * 4 + optionGap * 3;

  // 設定底部按鈕區高度。
  const bottomAreaHeight = constrain(
    canvasHeight * 0.13,
    70,
    120
  );

  // 設定內容底部限制位置。
  const bottomLimit =
    canvasHeight - bottomAreaHeight;

  // 設定選項起始位置。
  let optionTop =
    questionBoxY +
    questionBoxHeight +
    canvasHeight * 0.03 * compactFactor;

  // 如果選項超過底部限制，就往上移動。
  if (optionTop + optionsHeight > bottomLimit) {
    // 將選項位置往上調整。
    optionTop =
      bottomLimit -
      optionsHeight -
      canvasHeight * 0.02;
  }

  // 確保選項不會與題目框重疊。
  optionTop = max(
    optionTop,
    questionBoxY + questionBoxHeight + 8
  );

  // 設定提示文字位置。
  const feedbackY =
    optionTop +
    optionsHeight +
    max(10, canvasHeight * 0.022);

  // 設定按鈕寬度。
  const buttonWidth = constrain(
    canvasWidth * 0.3,
    120,
    220
  );

  // 設定按鈕高度。
  const buttonHeight = constrain(
    canvasHeight * 0.06,
    38,
    58
  );

  // 儲存全部版面配置。
  layout = {
    // 儲存內容寬度。
    contentWidth: contentWidth,

    // 儲存標題文字大小。
    titleSize: titleSize,

    // 儲存題數文字大小。
    counterSize: counterSize,

    // 儲存題目文字大小。
    questionSize: questionSize,

    // 儲存選項文字大小。
    optionSize: optionSize,

    // 儲存提示文字大小。
    feedbackSize: feedbackSize,

    // 儲存標題垂直位置。
    titleY: titleY,

    // 儲存題數垂直位置。
    counterY: counterY,

    // 儲存題目框水平位置。
    questionBoxX: (canvasWidth - contentWidth) / 2,

    // 儲存題目框垂直位置。
    questionBoxY: questionBoxY,

    // 儲存題目框寬度。
    questionBoxWidth: contentWidth,

    // 儲存題目框高度。
    questionBoxHeight: questionBoxHeight,

    // 儲存選項起始位置。
    optionTop: optionTop,

    // 儲存選項高度。
    optionHeight: optionHeight,

    // 儲存選項間距。
    optionGap: optionGap,

    // 儲存提示文字位置。
    feedbackY: feedbackY,

    // 儲存按鈕寬度。
    buttonWidth: buttonWidth,

    // 儲存按鈕高度。
    buttonHeight: buttonHeight,

    // 儲存按鈕垂直位置。
    buttonY: canvasHeight - buttonHeight - 18
  };
}

// 繪製測驗題目畫面。
function drawQuizScreen() {
  // 取得目前題目資料。
  const questionData = questions[currentQuestion];

  // 清除可點擊區域。
  hitAreas = [];

  // 設定標題文字顏色。
  fill("#263238");

  // 關閉外框。
  noStroke();

  // 設定標題文字大小。
  textSize(layout.titleSize);

  // 顯示測驗標題。
  text(
    "p5.js 程式設計簡易指令測驗",
    width / 2,
    layout.titleY
  );

  // 設定題數文字顏色。
  fill("#607d8b");

  // 設定題數文字大小。
  textSize(layout.counterSize);

  // 顯示目前題數。
  text(
    `第 ${currentQuestion + 1} 題／共 ${questions.length} 題`,
    width / 2,
    layout.counterY
  );

  // 設定題目框背景顏色。
  fill(QUESTION_COLOR);

  // 設定題目框外框顏色。
  stroke("#e0d866");

  // 設定題目框外框粗細。
  strokeWeight(3);

  // 繪製題目框。
  rect(
    layout.questionBoxX,
    layout.questionBoxY,
    layout.questionBoxWidth,
    layout.questionBoxHeight,
    18
  );

  // 設定題目文字顏色。
  fill("#263238");

  // 關閉題目文字外框。
  noStroke();

  // 設定題目文字大小。
  textSize(layout.questionSize);

  // 顯示置中的題目文字。
  drawCenteredWrappedText(
    questionData.question,
    width / 2,
    layout.questionBoxY +
      layout.questionBoxHeight / 2,
    layout.questionBoxWidth * 0.88
  );

  // 逐一繪製四個選項。
  for (let i = 0; i < questionData.options.length; i++) {
    // 計算選項水平位置。
    const x =
      (width - layout.contentWidth) / 2;

    // 計算選項垂直位置。
    const y =
      layout.optionTop +
      i * (layout.optionHeight + layout.optionGap);

    // 設定實際繪製水平位置。
    let drawX = x;

    // 設定實際繪製垂直位置。
    let drawY = y;

    // 設定選項預設背景顏色。
    let optionColor = "#ffffff";

    // 判斷是否顯示正確答案動畫。
    if (
      answerLocked &&
      i === questionData.answerIndex &&
      selectedIndex !== questionData.answerIndex
    ) {
      // 設定正確答案背景顏色。
      optionColor = CORRECT_COLOR;

      // 讓正確答案上下跳動。
      drawY += sin(frameCount * 0.18) * 8;
    }

    // 判斷是否顯示錯誤答案動畫。
    if (
      answerLocked &&
      i === selectedIndex &&
      selectedIndex !== questionData.answerIndex
    ) {
      // 設定錯誤答案背景顏色。
      optionColor = WRONG_COLOR;

      // 讓錯誤答案左右晃動。
      drawX += sin(frameCount * 0.22) * 8;
    }

    // 判斷使用者是否答對。
    if (
      answerLocked &&
      i === selectedIndex &&
      selectedIndex === questionData.answerIndex
    ) {
      // 設定答對答案背景顏色。
      optionColor = CORRECT_COLOR;
    }

    // 儲存選項點擊範圍。
    hitAreas.push({
      // 儲存選項水平位置。
      x: x,

      // 儲存選項垂直位置。
      y: y,

      // 儲存選項寬度。
      w: layout.contentWidth,

      // 儲存選項高度。
      h: layout.optionHeight
    });

    // 設定選項背景顏色。
    fill(optionColor);

    // 設定選項外框顏色。
    stroke("#cfd8dc");

    // 設定選項外框粗細。
    strokeWeight(2);

    // 繪製選項框。
    rect(
      drawX,
      drawY,
      layout.contentWidth,
      layout.optionHeight,
      14
    );

    // 設定選項文字顏色。
    fill("#263238");

    // 關閉選項文字外框。
    noStroke();

    // 設定選項文字大小。
    textSize(layout.optionSize);

    // 顯示選項文字。
    text(
      `${String.fromCharCode(65 + i)}. ${questionData.options[i]}`,
      width / 2,
      drawY + layout.optionHeight / 2
    );
  }

  // 設定提示文字大小。
  textSize(layout.feedbackSize);

  // 判斷是否已經作答。
  if (answerLocked) {
    // 判斷答案是否正確。
    const isCorrect =
      selectedIndex === questionData.answerIndex;

    // 設定提示文字顏色。
    fill(isCorrect ? "#168aad" : "#d1495b");

    // 顯示答題結果。
    text(
      isCorrect
        ? "答對了！"
        : "答錯了，正確答案已標示。",
      width / 2,
      layout.feedbackY
    );
  } else {
    // 設定操作提示文字顏色。
    fill("#78909c");

    // 顯示操作提示。
    text(
      "請點選一個答案開始作答",
      width / 2,
      layout.feedbackY
    );
  }

  // 繪製畫布按鈕。
  drawCanvasButtons();
}

// 繪製畫布上的按鈕。
function drawCanvasButtons() {
  // 判斷是否為結果畫面。
  if (quizFinished) {
    // 繪製重新開始按鈕。
    drawButton(
      width / 2 - layout.buttonWidth / 2,
      layout.buttonY,
      layout.buttonWidth,
      layout.buttonHeight,
      "重新開始",
      "#168aad"
    );
  } else if (answerLocked) {
    // 繪製下一題按鈕。
    drawButton(
      width / 2 - layout.buttonWidth / 2,
      layout.buttonY,
      layout.buttonWidth,
      layout.buttonHeight,
      "下一題",
      "#168aad"
    );
  }
}

// 繪製單一按鈕。
function drawButton(
  x,
  y,
  buttonWidth,
  buttonHeight,
  label,
  buttonColor
) {
  // 設定按鈕背景顏色。
  fill(buttonColor);

  // 關閉按鈕外框。
  noStroke();

  // 繪製圓角按鈕。
  rect(x, y, buttonWidth, buttonHeight, 12);

  // 設定按鈕文字顏色。
  fill("#ffffff");

  // 設定按鈕文字大小。
  textSize(
    constrain(buttonHeight * 0.38, 15, 22)
  );

  // 顯示按鈕文字。
  text(
    label,
    x + buttonWidth / 2,
    y + buttonHeight / 2
  );
}

// 繪製結果畫面。
function drawResultScreen() {
  // 設定結果標題顏色。
  fill("#263238");

  // 關閉文字外框。
  noStroke();

  // 設定結果標題大小。
  textSize(
    constrain(
      min(width * 0.07, height * 0.09),
      28,
      60
    )
  );

  // 顯示測驗完成文字。
  text(
    "測驗完成！",
    width / 2,
    height * 0.24
  );

  // 設定分數文字顏色。
  fill("#168aad");

  // 設定分數文字大小。
  textSize(
    constrain(
      min(width * 0.08, height * 0.11),
      34,
      76
    )
  );

  // 顯示答對題數。
  text(
    `答對 ${score}／${questions.length} 題`,
    width / 2,
    height * 0.4
  );

  // 設定評語文字顏色。
  fill("#607d8b");

  // 設定評語文字大小。
  textSize(
    constrain(
      min(width * 0.035, height * 0.05),
      15,
      28
    )
  );

  // 顯示測驗評語。
  text(
    getResultMessage(),
    width / 2,
    height * 0.53
  );

  // 顯示重新開始按鈕。
  drawCanvasButtons();
}

// 根據分數取得評語。
function getResultMessage() {
  // 判斷是否為滿分。
  if (score === questions.length) {
    // 回傳滿分評語。
    return "太棒了！你已經熟悉 p5.js 基礎指令！";
  }

  // 判斷是否答對三題以上。
  if (score >= 3) {
    // 回傳中高分評語。
    return "表現不錯！再練習一下就能全部答對。";
  }

  // 回傳鼓勵評語。
  return "繼續加油！建議重新練習 p5.js 基礎指令。";
}

// 將文字自動換行並置中。
function drawCenteredWrappedText(
  content,
  centerX,
  centerY,
  maxWidth
) {
  // 取得換行後的文字陣列。
  const lines = getWrappedLines(content, maxWidth);

  // 計算每行文字高度。
  const lineHeight = textSize() * 1.35;

  // 計算文字總高度。
  const totalHeight = lines.length * lineHeight;

  // 計算第一行文字位置。
  const startY =
    centerY -
    totalHeight / 2 +
    lineHeight / 2;

  // 設定文字置中。
  textAlign(CENTER, CENTER);

  // 逐行顯示文字。
  for (let i = 0; i < lines.length; i++) {
    // 計算目前文字行位置。
    const lineY = startY + i * lineHeight;

    // 顯示目前文字行。
    text(lines[i], centerX, lineY);
  }
}

// 將文字依最大寬度分行。
function getWrappedLines(content, maxWidth) {
  // 建立文字陣列。
  const lines = [];

  // 建立目前文字行。
  let currentLine = "";

  // 逐字處理文字。
  for (let i = 0; i < content.length; i++) {
    // 取得目前字元。
    const character = content[i];

    // 測試加入字元後的文字。
    const testLine = currentLine + character;

    // 判斷文字是否超過最大寬度。
    if (
      textWidth(testLine) > maxWidth &&
      currentLine.length > 0
    ) {
      // 儲存目前文字行。
      lines.push(currentLine);

      // 開始新的一行。
      currentLine = character;
    } else {
      // 將字元加入目前文字行。
      currentLine = testLine;
    }
  }

  // 儲存最後一行。
  if (currentLine.length > 0) {
    lines.push(currentLine);
  }

  // 回傳換行後的文字。
  return lines;
}

// 處理滑鼠按下事件。
function mousePressed() {
  // 處理滑鼠點擊位置。
  handlePointer(mouseX, mouseY);
}

// 處理觸控事件。
function touchStarted() {
  // 處理觸控點擊位置。
  handlePointer(mouseX, mouseY);

  // 阻止瀏覽器額外觸控行為。
  return false;
}

// 判斷使用者點擊位置。
function handlePointer(pointerX, pointerY) {
  // 判斷目前是否為結果畫面。
  if (quizFinished) {
    // 計算按鈕水平位置。
    const buttonX =
      width / 2 - layout.buttonWidth / 2;

    // 判斷是否點擊重新開始按鈕。
    if (
      pointerX >= buttonX &&
      pointerX <= buttonX + layout.buttonWidth &&
      pointerY >= layout.buttonY &&
      pointerY <=
        layout.buttonY + layout.buttonHeight
    ) {
      // 重新開始測驗。
      restartQuiz();
    }

    // 結束結果畫面判斷。
    return;
  }

  // 判斷目前題目是否尚未作答。
  if (!answerLocked) {
    // 逐一檢查選項。
    for (let i = 0; i < hitAreas.length; i++) {
      // 取得選項範圍。
      const area = hitAreas[i];

      // 判斷點擊是否位於水平範圍。
      const insideX =
        pointerX >= area.x &&
        pointerX <= area.x + area.w;

      // 判斷點擊是否位於垂直範圍。
      const insideY =
        pointerY >= area.y &&
        pointerY <= area.y + area.h;

      // 判斷是否點擊選項。
      if (insideX && insideY) {
        // 記錄使用者選擇。
        selectedIndex = i;

        // 鎖定目前題目。
        answerLocked = true;

        // 判斷答案是否正確。
        if (
          selectedIndex ===
          questions[currentQuestion].answerIndex
        ) {
          // 答對時增加分數。
          score++;
        }

        // 結束選項判斷。
        return;
      }
    }
  }

  // 判斷是否已作答。
  if (answerLocked) {
    // 計算下一題按鈕水平位置。
    const buttonX =
      width / 2 - layout.buttonWidth / 2;

    // 判斷是否點擊下一題按鈕。
    if (
      pointerX >= buttonX &&
      pointerX <= buttonX + layout.buttonWidth &&
      pointerY >= layout.buttonY &&
      pointerY <=
        layout.buttonY + layout.buttonHeight
    ) {
      // 前往下一題。
      goToNextQuestion();
    }
  }
}

// 前往下一題。
function goToNextQuestion() {
  // 確認目前題目已經作答。
  if (!answerLocked) {
    // 尚未作答時停止執行。
    return;
  }

  // 判斷是否仍有下一題。
  if (currentQuestion < questions.length - 1) {
    // 題目編號增加。
    currentQuestion++;

    // 清除選項選擇。
    selectedIndex = -1;

    // 解鎖下一題。
    answerLocked = false;

    // 重新計算版面。
    updateLayout();
  } else {
    // 最後一題完成後顯示結果。
    quizFinished = true;
  }
}

// 重新開始測驗。
function restartQuiz() {
  // 回到第一題。
  currentQuestion = 0;

  // 清除選項選擇。
  selectedIndex = -1;

  // 解鎖題目。
  answerLocked = false;

  // 將分數歸零。
  score = 0;

  // 設定測驗尚未完成。
  quizFinished = false;

  // 重新計算版面。
  updateLayout();
}

// 當瀏覽器視窗大小改變時執行。
function windowResized() {
  // 重新調整畫布大小。
  resizeCanvas(windowWidth, windowHeight);

  // 更新寬度記錄。
  lastWidth = width;

  // 更新高度記錄。
  lastHeight = height;

  // 重新計算響應式版面。
  updateLayout();
}

```
:::


---

## 學習3：設定嵌入 Google 字型，網頁文字採用這些字型

https://cfchen58.synology.me/115/week4/stage3/

**這個階段的目標：** 從 Google Fonts 嵌入繁體中文字型，並讓畫布上的題目與選項文字使用這些字型。
**這個階段會修改的檔案：** index.html、sketch.js

### 執行截圖

（把截圖拖曳到這裡，或貼上圖片連結）

![學習3截圖](請貼上截圖)

### 第一次問 AI

```tex!
（逐字貼上你第一次問 AI 的提示詞）
```

### 第二次問 AI

```tex!
（逐字貼上你第二次問 AI 的提示詞）
```

### 第三次問 AI

```tex!
（逐字貼上你第三次問 AI 的提示詞）
```

### 程式碼內容

:::info
:::spoiler 點開貼上學習3的程式碼
```javascript=
//學習3程式碼所在

```
:::


---

## 學習4：設定題庫並抽題顯示題目網頁（CSV 檔案）

https://cfchen58.synology.me/115/week4/stage4/

**這個階段的目標：** 把題目移到 questions.csv，網站讀取題庫後每次隨機抽出 5 題。
**這個階段會修改的檔案：** index.html、sketch.js、questions.csv

### 執行截圖

（把截圖拖曳到這裡，或貼上圖片連結）

![學習4截圖](請貼上截圖)

### 第一次問 AI

```tex!
（逐字貼上你第一次問 AI 的提示詞）
```

### 第二次問 AI

```tex!
（逐字貼上你第二次問 AI 的提示詞）
```

### 第三次問 AI

```tex!
（逐字貼上你第三次問 AI 的提示詞）
```

### 程式碼內容

:::info
:::spoiler 點開貼上學習4的程式碼
```javascript=
//學習4程式碼所在

```
:::


---

## 學習5：利用 Google Sheets 當題庫

https://cfchen58.synology.me/115/week4/stage5/

**這個階段的目標：** 把題庫放在 Google 試算表，網站直接讀取，老師改試算表，網站題目就跟著更新。
**這個階段會修改的檔案：** index.html、sketch.js（questions.csv 當備用題庫）

### 執行截圖

（把截圖拖曳到這裡，或貼上圖片連結）

![學習5截圖](請貼上截圖)

### 第一次問 AI

```tex!
（逐字貼上你第一次問 AI 的提示詞）
```

### 第二次問 AI

```tex!
（逐字貼上你第二次問 AI 的提示詞）
```

### 第三次問 AI

```tex!
（逐字貼上你第三次問 AI 的提示詞）
```

### 程式碼內容

:::info
:::spoiler 點開貼上學習5的程式碼
```javascript=
//學習5程式碼所在

```
:::


---

## 我的心得

這五個學習中，哪一個最困難？你是怎麼解決的？（請寫出實際發生的事）

＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿
