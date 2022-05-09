# 瀏覽器擴充功能專案 Part 1：學習背景工作與效能

## 課前測驗

[課前測驗](https://happy-mud-02d95f10f.azurestaticapps.net/quiz/27?loc=zh_tw)

### 大綱

在前兩堂課程中，你學會如何建立表單、顯示 API 回覆的資料在結果區塊中，這是網頁處理網頁資訊的標準行為。你甚至學會了如何非同步性地抓取資料。你的擴充套件就快完成了。

它只剩管理背景工作：包括刷新套件的圖示顏色，我們來討論瀏覽器是如何處理這類的工作。也讓我們探討你所建立的網頁，瀏覽器會多有效地處理其中的內容。

## 網頁處理效能的基礎

> "網頁處理效能攸關兩件事：網頁多快地載入，與程式多快地執行。" -- [Zack Grossbart](https://www.smashingmagazine.com/2012/06/javascript-profiling-chrome-developer-tools/)

關於如何讓你的網頁能快速地運作在各類裝置、各個使用者以及各種情況，是一件難以想像的龐大主題。這裡有一些要點確保你在開發網頁或是擴充功能時，銘記在心。

第一件事為確保網頁收集關於網頁效能的資料，在瀏覽器的開發者工具中可以實現它。在 Edge 中，選擇「設定及更多」按鈕(瀏覽器上三個點的圖示)，並選擇更多工具 > 開發人員工具並開啟 Performance 分頁。你也可以使用鍵盤快捷鍵，Windows 上的 `Ctrl` + `Shift` + `I` 與 Mac 上的 `Option` + `Command` + `I` 來開啟開發人員工具。

Performance 分頁包括了效能分析工具。開啟一個網頁，例如 https://www.microsoft.com ，點擊 'Record' 按鈕並重新整理網頁。停止錄製後你就能取得網頁的 'script'、'render' 與 'paint' 的過程與資訊：

![Edge 性能分析工具](../images/profiler.png)

✅ 造訪 [Microsoft 文件](https://docs.microsoft.com/microsoft-edge/devtools-guide/performance?WT.mc_id=academic-13441-cxa)觀看 Edge 的 Performance 分頁資訊

> 提示：要取得真正的網頁開啟時間，記得清除你的瀏覽器快取。

選擇一樣網頁在載入時，時間列中出現的事件物件。

觀看它的總覽面板並截圖你的網頁效能。

![Edge 性能分析工具截圖](../images/snapshot.png)

檢查 Event Log 面板，是否有網頁事件花超過 15 毫秒：

![Edge event log](../images/log.png)

✅ 了解你的性能分析工具！在這個網頁中，開啟開發者工具，檢查是否有任何 bottleneck。什麼是載入最久的物件？哪個又是最快的？

## 效能分析

總體而言，每一位網頁開發者一定要注意一些「有問題的地方」，避免在發布作品時有令人意想不到的驚喜。

**資產(Asset)大小**：過去幾年來，網頁「變重」了，也因此變慢了。有些負擔來自於圖片的使用。

✅ 查詢 [Internet Archive](https://httparchive.org/reports/page-weight)，看看過去的網頁負擔等資訊。

一個好的習慣是確保你的圖片有做最佳化，呈現合理的檔案大小及解析度影像給你的使用者。

**DOM 查找元素(Traversal)**：瀏覽器必須依照你的程式碼建立 Document Object Model，請確保你的 tags 最小化，網頁只使用必須的功能與造型。另外，過量的網頁 CSS 也可以被最佳化，舉例來說，造型樣板只用在單頁上，而非全域上。

**JavaScript**：每一位 JavaScript 開發者會觀察 'render-blocking' 腳本，它會在 DOM 查找與瀏覽器呈現前被載入好。請考慮使用 `defer` 在你的程式碼中，我們的盆栽盒專案就有實踐這行。

✅ 在[網頁測速網](https://www.webpagetest.org/)上測試一些網頁，學習確認網頁效能的基本檢查。

現在你了解瀏覽器如何呈現你所提供的資產，我們來看看我們的擴充功能最後需要補齊的項目：

### 建立函式計算顏色

編輯 `/src/index.js`，新增函式 `calculateColor()` 在一系列為了 DOM 存取的 `const` 變數之後：

```JavaScript
function calculateColor(value) {
	let co2Scale = [0, 150, 600, 750, 800];
	let colors = ['#2AA364', '#F5EB4D', '#9E4229', '#381D02', '#381D02'];

	let closestNum = co2Scale.sort((a, b) => {
		return Math.abs(a - value) - Math.abs(b - value);
	})[0];
	console.log(value + ' is closest to ' + closestNum);
	let num = (element) => element > closestNum;
	let scaleIndex = co2Scale.findIndex(num);

	let closestColor = colors[scaleIndex];
	console.log(scaleIndex, closestColor);

	chrome.runtime.sendMessage({ action: 'updateIcon', value: { color: closestColor } });
}
```

發生了什麼事？你傳遞了 API 回傳的二氧化碳濃度數值，計算出它最適合對應的顏色矩陣索引位置。之後，你將這個顏色數值傳給了 chrome runtime。

chrome.runtime 有[一個 API](https://developer.chrome.com/extensions/runtime)處理所有的背景工作，你的擴充套件借助了此功能：

> "在應用程式中，使用 chrome.runtime API 來接收背景頁面，回傳關於 manifest 的資訊，監聽並回應事件。你也可以利用此 API 轉換 URL 的相對路徑成絕對路徑。"

✅ 如果你正打算開發此專案給 Edge 瀏覽器上使用，你會訝異你使用的是 chrome API。新的 Edge 瀏覽器執行在 Chromium browser 引擎上，所以你也能使用這些工具。

> 注意，如果你想要剖析瀏覽器擴充功能，請在擴充套件上執行開發者工具，它與瀏覽器主視窗為不同的個體。

### 設定圖示預設顏色

現在，在函式 `init()` 中，利用呼叫 chrome `updateIcon` 設定圖示顏色為通用綠：

```JavaScript
chrome.runtime.sendMessage({
	action: 'updateIcon',
		value: {
			color: 'green',
		},
});
```
### 呼叫函式、執行呼叫

接下來，在 C02Signal API 回傳的 promise 物件下方呼叫函式：

```JavaScript
//let CO2...
calculateColor(CO2);
```
最後，在檔案 `/dist/background.js` 中，新增事件監聽者給這些背景行為的呼叫：

```JavaScript
chrome.runtime.onMessage.addListener(function (msg, sender, sendResponse) {
	if (msg.action === 'updateIcon') {
		chrome.browserAction.setIcon({ imageData: drawIcon(msg.value) });
	}
});
//參考 energy lollipop extension，很好的程式！
function drawIcon(value) {
	let canvas = document.createElement('canvas');
	let context = canvas.getContext('2d');

	context.beginPath();
	context.fillStyle = value.color;
	context.arc(100, 100, 50, 0, 2 * Math.PI);
	context.fill();

	return context.getImageData(50, 50, 100, 100);
}
```
在此程式中，你建立了事件監聽者給任何前到背景工作管理者的訊息。若 'updateIcon' 被呼叫，則接下來的程式會被執行，利用 Canvas API 繪製出對應顏色的圖示。

✅ 你會學習更多關於 Canvas API 在往後的[太空遊戲課程](../../../6-space-game/2-drawing-to-canvas/translations/README.zh-tw.md)。

現在，重新建制你的擴充功能(`npm run build`)，刷新並運行你的套件，觀察圖示的顏色變化。現在是時候去跑腿或是洗碗嗎？現在你知道了！

恭喜你，你已經建立了一款實用的瀏覽器擴充功能，並學到更多瀏覽器的運作方式與監測它的效能分析。

---

## 🚀 挑戰

調查一些悠久的開源網站，並根據它們的 GitHub 歷史，你能分辨它們過去幾年以來效能上的調整嗎？什麼它們是共同的痛點？

## 課後測驗

[課後測驗](https://happy-mud-02d95f10f.azurestaticapps.net/quiz/28?loc=zh_tw)

## 複習與自學

請考慮註冊[performance newsletter](https://perf.email/)

調查瀏覽器測量網頁效能的方法，查看開發者工具內的 Performance 分頁。你能找到什麼巨大的差別嗎？

## 作業

[分析網頁效能](assignment.zh-tw.md)

