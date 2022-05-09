# एक अंतरिक्ष खेल भाग 2 बनाएँ: कैनवस के लिए नायक और राक्षस बनाएँ

## लेक्चर पाहिले की क्विज

[लेक्चर पाहिले की क्विज](https://happy-mud-02d95f10f.azurestaticapps.net/quiz/31?loc=hi)

## कैनवास

कैनवास एक HTML तत्व है जो डिफ़ॉल्ट रूप से कोई सामग्री नहीं है; यह एक खाली स्लेट है. आपको उस पर ड्राइंग करके इसे जोड़ना होगा.

✅ MDN पर [कैनवस एपीआई के बारे में और पढ़ें](https://developer.mozilla.org/docs/Web/API/Canvas_API).

यहां बताया गया है कि पेज के मुख्य भाग के रूप में यह आमतौर पर घोषित किया जाता है:

```html
<canvas id="myCanvas" width="200" height="100"></canvas>
```

ऊपर हम `id`,`width` और `height` सेट कर रहे हैं

- `id`: इसे सेट करें ताकि आप एक संदर्भ प्राप्त कर सकें जब आपको इसके साथ परस्पर करने की आवश्यकता हो.
- `width`: यह तत्व की चौड़ाई है.
- `height`: यह तत्व की ऊंचाई है.

## सरल ज्यामिति खींचना

कैनवस चीजों को आकर्षित करने के लिए एक कार्टेशियन समन्वय प्रणाली का उपयोग कर रहा है.
इस प्रकार यह x- अक्ष और y- अक्ष का उपयोग करता है जहां कुछ स्थित है व्यक्त करने के लिए.स्थान `0,0` शीर्ष बाईं स्थिति है और नीचे दायाँ हिस्सा है जिसे आपने कैनवास का WIDTH और HEIGHT कहा है.

![कैनवास का ग्रिड](../canvas_grid.png)

> [MDN](https://developer.mozilla.org/docs/Web/API/Canvas_API/Tutorial/Drawing_shapes) से छवि

कैनवास तत्व पर आकर्षित करने के लिए आपको निम्नलिखित चरणों से गुजरना होगा:

1. **एक संदर्भ प्राप्त करें** कैनवास तत्व के लिए.
2. **एक संदर्भ प्राप्त करें** संदर्भ तत्व पर जो कैनवास तत्व पर बैठता है.
3. **एक ड्राइंग ऑपरेशन** संदर्भ तत्व का उपयोग करके.

उपरोक्त चरणों के लिए कोड आमतौर पर ऐसा दिखता है:

```javascript
// draws a red rectangle
//1. get the canvas reference
canvas = document.getElementById("myCanvas");

//2. set the context to 2D to draw basic shapes
ctx = canvas.getContext("2d");

//3. fill it with the color red
ctx.fillStyle = "red";

//4. and draw a rectangle with these parameters, setting location and size
ctx.fillRect(0, 0, 200, 200); // x,y,width, height
```

✅ कैनवस एपीआई ज्यादातर 2डी आकृतियों पर केंद्रित है, लेकिन आप एक वेब साइट पर 3डी तत्वों को भी आकर्षित कर सकते हैं; इसके लिए, आप [वेबगियल एपीआई](https://developer.mozilla.org/docs/Web/API/WebGL_API) का उपयोग कर सकते हैं.

आप कैनवस एपीआई के साथ सभी प्रकार की चीजें आकर्षित कर सकते हैं जैसे की :

- **Geometrical shapes**, हमने पहले ही दिखाया है कि एक आयत कैसे खींचना है, लेकिन बहुत कुछ है जो आप खींच सकते हैं.
- **Text**, आप अपनी इच्छानुसार किसी भी फ़ॉन्ट और रंग के साथ एक पाठ खींच सकते हैं.
- **Images**, आप उदाहरण के लिए .jpg या .png जैसी छवि परिसंपत्ति के आधार पर चित्र बना सकते हैं.

✅ कोशिश करो! आप जानते हैं कि एक आयत कैसे बनाई जाती है, क्या आप किसी पृष्ठ पर एक वृत्त खींच सकते हैं? कोडपेन पर कुछ दिलचस्प कैनवस ड्रॉइंग पर एक नज़र डालें. यहाँ एक [विशेष रूप से प्रभावशाली उदाहरण है](https://codepen.io/dissimulate/pen/KrAwx).

## छवि एसेट लोड और ड्रा करे

आप एक 'Image' ऑब्जेक्ट बनाकर एक छवि एसेट लोड करते हैं और इसकी `src` गुण सेट करते हैं.तब आप यह जानने के लिए `load` घटना सुनते हैं कि यह कब उपयोग होने के लिए तैयार है. कोड इस तरह दिखता है:

### लोड एसेट

```javascript
const img = new Image();
img.src = "path/to/my/image.png";
img.onload = () => {
  // image loaded and ready to be used
};
```

### लोड एसेट पैटर्न

इसे एक निर्माण में ऊपर से लपेटने की सिफारिश की गई है, इसलिए इसका उपयोग करना आसान है और आप इसे पूरी तरह से लोड होने पर केवल हेरफेर करने का प्रयास करते हैं:

```javascript
function loadAsset(path) {
  return new Promise((resolve) => {
    const img = new Image();
    img.src = path;
    img.onload = () => {
      // image loaded and ready to be used
      resolve(img);
    };
  });
}

// use like so

async function run() {
  const heroImg = await loadAsset("hero.png");
  const monsterImg = await loadAsset("monster.png");
}
```

गेम एसेट्स को स्क्रीन पर खींचने के लिए, आपका कोड इस तरह दिखेगा:

```javascript
async function run() {
  const heroImg = await loadAsset("hero.png");
  const monsterImg = await loadAsset("monster.png");

  canvas = document.getElementById("myCanvas");
  ctx = canvas.getContext("2d");
  ctx.drawImage(heroImg, canvas.width / 2, canvas.height / 2);
  ctx.drawImage(monsterImg, 0, 0);
}
```

## अब अपने खेल का निर्माण शुरू करने का समय आ गया है

### क्या बनना है

आप कैनवास तत्व के साथ एक वेब पेज बनाएंगे। यह एक काली स्क्रीन `1024 * 768` को प्रस्तुत करना चाहिए। हमने आपको दो चित्र प्रदान किए हैं:

- हीरो शिप

  ![हीरो शिप ](../solution/assets/player.png)

- 5\*5 मॉन्स्टर

  ![मॉन्स्टर शिप](../solution/assets/enemyShip.png)

### विकास शुरू करने के लिए अनुशंसित कदम

उन फ़ाइलों का पता लगाएँ जो आपके लिए `your-work` सब फ़ोल्डर में बनाई गई हैं. इसमें निम्नलिखित शामिल होना चाहिए:

```bash
-| assets
  -| enemyShip.png
  -| player.png
-| index.html
-| app.js
-| package.json
```

विजुअल स्टूडियो कोड में इस फ़ोल्डर की कॉपी खोलें। आपके पास स्थानीय विकास पर्यावरण सेटअप होना चाहिए, अधिमानतः एनपीएम और नोड के साथ विजुअल स्टूडियो कोड स्थापित किया जाना चाहिए. यदि आपके पास अपने कंप्यूटर पर `npm` सेट नहीं है, तो [यहाँ है कि कैसे करें](https://www.npmjs.com/get-npm).

`Your_work` फ़ोल्डर में नेविगेट करके अपनी परियोजना शुरू करें:

```bash
cd your-work
npm start
```

उपरोक्त पते पर एक HTTP सर्वर शुरू होगा `http: // localhost: 5000`। एक ब्राउज़र और उस पते पर इनपुट खोलें। यह अभी एक रिक्त पृष्ठ है, लेकिन यह बदल जाएगा

> नोट: अपनी स्क्रीन पर परिवर्तन देखने के लिए, अपने ब्राउज़र को ताज़ा करें.

### कोड जोड़ें

नीचे हल करने के लिए `your-work/app.js` के लिए आवश्यक कोड जोड़ें

1. काली पृष्ठभूमि के साथ एक कैनवास **ड्रा** करे
   > टिप: `/app.js` में उपयुक्त TODO के तहत दो लाइनें जोड़ें,`ctx` तत्व को काला बनाने के लिए और शीर्ष/बाएँ निर्देशांक 0,0 पर हो और ऊँचाई और चौड़ाई कैनवास के बराबर हो।
2. **भार** बनावट
   > टिप: `await loadTexture` का उपयोग करके खिलाड़ी और दुश्मन की छवियों को जोड़ें और छवि पथ में पास करें। आप उन्हें अभी तक स्क्रीन पर नहीं देखेंगे!
3. नीचे आधे में स्क्रीन के केंद्र में नायक **ड्रा** करे
   > टिप: स्क्रीन पर HeroImg ड्रा करने के लिए `drawImage` API का उपयोग करें, `canvas.width / 2 - 45` और `canvas.height - canvas.height / 4)` की सेटिंग;
4. 5\*5 मॉन्स्टर **ड्रा** करे

   > टिप: अब आप स्क्रीन पर दुश्मनों को आकर्षित करने के लिए कोड की टिप्पणी हटा सकते हैं। अगला, `createEnemy` फ़ंक्शन पर जाएं और इसे बनाएं.

   सबसे पहले, कुछ स्थिरांक स्थापित करें:

   ```javascript
   const MONSTER_TOTAL = 5;
   const MONSTER_WIDTH = MONSTER_TOTAL * 98;
   const START_X = (canvas.width - MONSTER_WIDTH) / 2;
   const STOP_X = START_X + MONSTER_WIDTH;
   ```

   फिर, स्क्रीन पर राक्षसों की सरणी खींचने के लिए एक लूप बनाएं:

   ```javascript
   for (let x = START_X; x < STOP_X; x += 98) {
     for (let y = 0; y < 50 * 5; y += 50) {
       ctx.drawImage(enemyImg, x, y);
     }
   }
   ```

## परिणाम

समाप्त परिणाम ऐसा दिखना चाहिए:

![एक नायक और 5 * 5 राक्षसों के साथ काली स्क्रीन](../partI-solution.png)

## घोल

कृपया पहले इसे स्वयं हल करने का प्रयास करें, लेकिन यदि आप अटक जाते हैं, तो एक [समाधान] (../solution/app.js) पर एक नज़र डालें

---

## 🚀 चुनौती

आपने 2डी-केंद्रित कैनवस एपीआई के साथ ड्राइंग के बारे में सीखा है; [वेबगियल एपीआई](https://developer.mozilla.org/docs/Web/API/WebGL_API) पर एक नज़र डालें, और एक 3D ऑब्जेक्ट खींचने का प्रयास करें.

## पोस्ट-व्याख्यान प्रश्नोत्तरी

[पोस्ट-व्याख्यान प्रश्नोत्तरी](https://happy-mud-02d95f10f.azurestaticapps.net/quiz/32?loc=hi)

## समीक्षा और स्व अध्ययन

कैनवस एपीआई के बारे में अधिक जानकारी के लिए [इसके बारे में पढ़े](https://developer.mozilla.org/docs/Web/API/Canvas_API).

## असाइनमेंट

[कैनवस एपीआई के साथ खेले](assignment.hi.md)
