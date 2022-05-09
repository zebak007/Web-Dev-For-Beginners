# एक अंतरिक्ष खेल भाग 3 बनाएँ: गति जोड़ना

## प्री-लेक्चर क्विज

[प्री-लेक्चर क्विज](https://happy-mud-02d95f10f.azurestaticapps.net/quiz/33?loc=hi)

जब तक आप परदे पर चारों ओर एलियंस चल रहे हैं तब तक गेम्स बहुत मज़ेदार नहीं हैं! इस खेल में, हम दो प्रकार के आंदोलनों का उपयोग करेंगे:

- **Keyboard/Mouse movement**: जब उपयोगकर्ता स्क्रीन पर किसी ऑब्जेक्ट को स्थानांतरित करने के लिए कीबोर्ड या माउस के साथ इंटरैक्ट करता है.
- **Game induced movement**: जब खेल एक निश्चित समय अंतराल के साथ एक वस्तु को स्थानांतरित करता है.

तो हम चीजों को स्क्रीन पर कैसे स्थानांतरित करते हैं? यह सब कार्तीय निर्देशांक के बारे में है:हम ऑब्जेक्ट का स्थान (x, y) बदलते हैं और फिर स्क्रीन को फिर से खोलते हैं।

आमतौर पर आपको स्क्रीन पर _गति_ को पूरा करने के लिए निम्न चरणों की आवश्यकता होती है:

1. एक वस्तु के लिए **एक नया स्थान निर्धारित करें**; यह वस्तु को स्थानांतरित करने के रूप में अनुभव करने के लिए आवश्यक है.
2. **स्क्रीन को साफ़ करें**, ड्रॉ के बीच स्क्रीन को साफ़ करना होगा. We can clear it by drawing a rectangle that we fill with a background color.
3. नए स्थान पर **Redraw ऑब्जेक्ट**। ऐसा करने से हम अंत में वस्तु को एक स्थान से दूसरे स्थान तक ले जाते हैं.

यहाँ यह कोड में कैसा दिख सकता है:

```javascript
//set the hero's location
hero.x += 5;
// clear the rectangle that hosts the hero
ctx.clearRect(0, 0, canvas.width, canvas.height);
// redraw the game background and hero
ctx.fillRect(0, 0, canvas.width, canvas.height);
ctx.fillStyle = "black";
ctx.drawImage(heroImg, hero.x, hero.y);
```

✅ क्या आप एक कारण के बारे में सोच सकते हैं कि अपने नायक को प्रति सेकंड कई फ़्रेमों को फिर से तैयार करने से प्रदर्शन की लागत बढ़ सकती है? [इस पैटर्न के विकल्प](https://www.html5rocks.com/en/tutorials/canvas/performance/) के बारे में पढ़ें.

## कीबोर्ड घटनाओं को संभालें

आप विशिष्ट घटनाओं को कोड में संलग्न करके घटनाओं को संभालते हैं. कीबोर्ड की घटनाओं को पूरी विंडो पर ट्रिगर किया जाता है जबकि एक `click` जैसे माउस इवेंट को एक विशिष्ट तत्व को क्लिक करने के लिए जोड़ा जा सकता है. हम इस पूरे प्रोजेक्ट में कीबोर्ड इवेंट का उपयोग करेंगे.

एक घटना को संभालने के लिए आपको विंडो के `addEventListener()` विधि का उपयोग करना होगा और इसे दो इनपुट मापदंडों के साथ प्रदान करना होगा. पहला पैरामीटर घटना का नाम है, उदाहरण के लिए `keyup`. दूसरा पैरामीटर वह फ़ंक्शन होता है, जिसे ईवेंट होने के परिणामस्वरूप लागू किया जाना चाहिए.

यहाँ एक उदाहरण है:

```javascript
window.addEventListener("keyup", (evt) => {
  // `evt.key` = string representation of the key
  if (evt.key === "ArrowUp") {
    // do something
  }
});
```

प्रमुख घटनाओं के लिए, इस घटना पर दो गुण हैं जिन्हें आप यह देखने के लिए उपयोग कर सकते हैं कि किस कुंजी को दबाया गया था:

- `key`, यह दबाए गए कुंजी का एक स्ट्रिंग प्रतिनिधित्व है, उदाहरण के लिए` ArrowUp`
- `KeyCode`, यह एक संख्या प्रतिनिधित्व है, उदाहरण के लिए` 37`, `ArrowLeft` से मेल खाती है.

✅ की इवेंट हेरफेर खेल के विकास के बाहर उपयोगी है। इस तकनीक के लिए आप और क्या उपयोग कर सकते हैं?

### विशेष कुंजी: एक चेतावनी

कुछ _special_ किइस हैं जो विंडोज को प्रभावित करती हैं. इसका मतलब है कि यदि आप एक `keyup` घटना सुन रहे हैं और आप अपने नायक को स्थानांतरित करने के लिए इन विशेष किइस का उपयोग करते हैं तो यह क्षैतिज स्क्रॉलिंग भी करेगा.इस कारण से आप अपने गेम को बनाते समय इस बिल्ट-इन ब्राउज़र व्यवहार को बंद कर सकते हैं। आपको इस तरह कोड की आवश्यकता है:

```javascript
let onKeyDown = function (e) {
  console.log(e.keyCode);
  switch (e.keyCode) {
    case 37:
    case 39:
    case 38:
    case 40: // Arrow keys
    case 32:
      e.preventDefault();
      break; // Space
    default:
      break; // do not block other keys
  }
};

window.addEventListener("keydown", onKeyDown);
```

उपरोक्त कोड यह सुनिश्चित करेगा कि एरो किइस और स्पेस की का डिफ़ॉल्ट व्यवहार बंद हो। शट-ऑफ तंत्र तब होता है जब हम `e.preventDefault ()` कहते हैं.

## खेल प्रेरित चाल

हम टाइम सेट का उपयोग करके चीजों को खुद से आगे बढ़ा सकते हैं जैसे `setTimeout()` या `setInterval()` फ़ंक्शन जो प्रत्येक टिक पर वस्तु के स्थान को अपडेट करते हैं, या समय अंतराल।. यहां ऐसा है जो दिख सकता है:

```javascript
let id = setInterval(() => {
  //move the enemy on the y axis
  enemy.y += 10;
});
```

## गेम लूप

गेम लूप एक अवधारणा है जो अनिवार्य रूप से एक फ़ंक्शन है जिसे नियमित अंतराल पर लागू किया जाता है. इसे गेम लूप कहा जाता है क्योंकि उपयोगकर्ता को दिखाई देने वाली सभी चीज़ों को लूप में खींचा जाना चाहिए. गेम लूप उन सभी गेम ऑब्जेक्ट्स का उपयोग करता है जो गेम का हिस्सा हैं, उन सभी को ड्रॉ करना जब तक कि किसी कारण से गेम का हिस्सा नहीं होना चाहिए. उदाहरण के लिए यदि कोई वस्तु एक शत्रु है जो लेजर से टकराती है और उड़ती है, तो यह वर्तमान गेम लूप का हिस्सा नहीं है (बाद के पाठों में आप इस पर और अधिक सीखेंगे).

यहाँ एक गेम लूप आमतौर पर कैसा दिख सकता है, कोड में व्यक्त किया गया है:

```javascript
let gameLoopId = setInterval(
  () =>
    function gameLoop() {
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      ctx.fillStyle = "black";
      ctx.fillRect(0, 0, canvas.width, canvas.height);
      drawHero();
      drawEnemies();
      drawStaticObjects();
    },
  200
);
```

उपरोक्त लूप को कैनवास को फिर से बनाने के लिए प्रत्येक `200` मिलीसेकेंड पर लगाया जाता है. आपके पास सबसे अच्छा अंतराल चुनने की क्षमता है जो आपके खेल के लिए समझ में आता है.

## अंतरिक्ष खेल को जारी रखना

आप मौजूदा कोड लेंगे और उसे विस्तारित करेंगे। या तो उस कोड से शुरू करें जो आपने भाग I के दौरान पूरा किया था या [भाग II- स्टार्टर](../your-work) में कोड का उपयोग करें.

- **Moving the hero**: आप यह सुनिश्चित करने के लिए कोड जोड़ेंगे कि आप तीर किइस का उपयोग करके नायक को स्थानांतरित कर सकते हैं.
- **Move enemies**: दुश्मनों को किसी दिए गए दर पर ऊपर से नीचे ले जाने के लिए आपको कोड जोड़ने की भी आवश्यकता होगी.

## अनुशंसित कदम

उन फ़ाइलों का पता लगाएँ जो आपके लिए `your-work` सब फ़ोल्डर में बनाई गई हैं. इसमें निम्नलिखित शामिल होना चाहिए:

```bash
-| assets
  -| enemyShip.png
  -| player.png
-| index.html
-| app.js
-| package.json
```

आप टाइप करके अपना प्रोजेक्ट `your_work` फ़ोल्डर शुरू करें:

```bash
cd your-work
npm start
```

उपरोक्त पते पर एक HTTP सर्वर शुरू होगा `http: // localhost: 5000`. एक ब्राउज़र खोलें और उस पते को इनपुट करें, अभी उसे नायक और सभी दुश्मनों को प्रस्तुत करना चाहिए; कुछ भी नहीं चल रहा है - फिर भी!

### कोड जोड़ें

1. `हीरो` और `दुश्मन` और `गेम ऑब्जेक्ट` के लिए **समर्पित ऑब्जेक्ट्स जोड़ें**, उनके पास` x` और `y` गुण होने चाहिए ([इन्हेरिटेंस या कम्पोजीशन](../../README.md) पर भाग याद रखें ).

   _संकेत_ `गेम ऑब्जेक्ट` `x` और `y` के साथ एक होना चाहिए और एक कैनवास पर खुद को आकर्षित करने की क्षमता होनी चाहिए

   > टिप: नीचे के रूप में देलिनेटेड कंस्ट्रक्टर के साथ एक नया ग़मओब्जेक्ट वर्ग जोड़कर शुरू करें, और फिर इसे कैनवास पर ड्रा करें:

   ```javascript
   class GameObject {
     constructor(x, y) {
       this.x = x;
       this.y = y;
       this.dead = false;
       this.type = "";
       this.width = 0;
       this.height = 0;
       this.img = undefined;
     }

     draw(ctx) {
       ctx.drawImage(this.img, this.x, this.y, this.width, this.height);
     }
   }
   ```

अब, इस गेमओब्जेक्ट को हीरो और दुश्मन बनाने के लिए विस्तारित करें.

```javascript
class Hero extends GameObject {
  constructor(x, y) {
    ...it needs an x, y, type, and speed
  }
}
```

```javascript
class Enemy extends GameObject {
  constructor(x, y) {
    super(x, y);
    (this.width = 98), (this.height = 50);
    this.type = "Enemy";
    let id = setInterval(() => {
      if (this.y < canvas.height - this.height) {
        this.y += 5;
      } else {
        console.log("Stopped at", this.y);
        clearInterval(id);
      }
    }, 300);
  }
}
```

2. **की ईवेंट हैंडलर जोड़ें** की नेविगेशन को संभालने के लिए (बाएं / दाएं हीरो ऊपर ले जाएं)

   _REMEMBER_ यह कार्टेशियन सिस्टम है, टॉप-लेफ्ट `0,0` है. यह भी याद रखें कि _डिफ़ॉल्ट व्यवहार_ को रोकने के लिए कोड जोड़ना

   > टिप: अपने onKeyDown फ़ंक्शन को बनाएं और इसे विंडो में अटैच करें:

   ```javascript
    let onKeyDown = function (e) {
         console.log(e.keyCode);
           ...add the code from the lesson above to stop default behavior
         }
    };

    window.addEventListener("keydown", onKeyDown);
   ```

   इस बिंदु पर अपने ब्राउज़र कंसोल की जाँच करें, और लॉग किए जा रहे कीस्ट्रोक्स को देखें.

3. \*\* लागू करें [[पब उप पैटर्न](../../ README.md), शेष भागों का पालन करते हुए यह आपके कोड को साफ रखेगा.

   यह अंतिम भाग करने के लिए, आप कर सकते हैं:

   1. विंडो पर **एक घटना श्रोता जोड़ें**:

      ```javascript
      window.addEventListener("keyup", (evt) => {
        if (evt.key === "ArrowUp") {
          eventEmitter.emit(Messages.KEY_EVENT_UP);
        } else if (evt.key === "ArrowDown") {
          eventEmitter.emit(Messages.KEY_EVENT_DOWN);
        } else if (evt.key === "ArrowLeft") {
          eventEmitter.emit(Messages.KEY_EVENT_LEFT);
        } else if (evt.key === "ArrowRight") {
          eventEmitter.emit(Messages.KEY_EVENT_RIGHT);
        }
      });
      ```

   2. संदेशों को प्रकाशित करने और सदस्यता लेने के लिए **एक EventEmitter वर्ग बनाएं**:

      ```javascript
      class EventEmitter {
        constructor() {
          this.listeners = {};
        }

        on(message, listener) {
          if (!this.listeners[message]) {
            this.listeners[message] = [];
          }
          this.listeners[message].push(listener);
        }

        emit(message, payload = null) {
          if (this.listeners[message]) {
            this.listeners[message].forEach((l) => l(message, payload));
          }
        }
      }
      ```

   3. **स्थिरांक जोड़ें** और EventEmitter सेट करें:

      ```javascript
      const Messages = {
        KEY_EVENT_UP: "KEY_EVENT_UP",
        KEY_EVENT_DOWN: "KEY_EVENT_DOWN",
        KEY_EVENT_LEFT: "KEY_EVENT_LEFT",
        KEY_EVENT_RIGHT: "KEY_EVENT_RIGHT",
      };

      let heroImg,
        enemyImg,
        laserImg,
        canvas,
        ctx,
        gameObjects = [],
        hero,
        eventEmitter = new EventEmitter();
      ```

   4. **खेल को प्रारंभ करें**

   ```javascript
   function initGame() {
     gameObjects = [];
     createEnemies();
     createHero();

     eventEmitter.on(Messages.KEY_EVENT_UP, () => {
       hero.y -= 5;
     });

     eventEmitter.on(Messages.KEY_EVENT_DOWN, () => {
       hero.y += 5;
     });

     eventEmitter.on(Messages.KEY_EVENT_LEFT, () => {
       hero.x -= 5;
     });

     eventEmitter.on(Messages.KEY_EVENT_RIGHT, () => {
       hero.x += 5;
     });
   }
   ```

4. **गेम लूप सेट करें**

   गेम को प्रारंभ करने के लिए window.onload फ़ंक्शन को रिफ्लेक्टर करें और एक अच्छे अंतराल पर गेम लूप सेट करें। आप एक लेजर बीम भी जोड़ जोड़ सकते है:

   ```javascript
   window.onload = async () => {
     canvas = document.getElementById("canvas");
     ctx = canvas.getContext("2d");
     heroImg = await loadTexture("assets/player.png");
     enemyImg = await loadTexture("assets/enemyShip.png");
     laserImg = await loadTexture("assets/laserRed.png");

     initGame();
     let gameLoopId = setInterval(() => {
       ctx.clearRect(0, 0, canvas.width, canvas.height);
       ctx.fillStyle = "black";
       ctx.fillRect(0, 0, canvas.width, canvas.height);
       drawGameObjects(ctx);
     }, 100);
   };
   ```

5. एक निश्चित अंतराल पर दुश्मनों को स्थानांतरित करने के लिए **कोड जोड़ें**

   शत्रुओं को बनाने और उन्हें नए गेमऑब्जेक्ट्स क्लास में धकेलने के लिए `createEnemies()` फ़ंक्शन को रिफलेक्‍टर करें:

   ```javascript
   function createEnemies() {
     const MONSTER_TOTAL = 5;
     const MONSTER_WIDTH = MONSTER_TOTAL * 98;
     const START_X = (canvas.width - MONSTER_WIDTH) / 2;
     const STOP_X = START_X + MONSTER_WIDTH;

     for (let x = START_X; x < STOP_X; x += 98) {
       for (let y = 0; y < 50 * 5; y += 50) {
         const enemy = new Enemy(x, y);
         enemy.img = enemyImg;
         gameObjects.push(enemy);
       }
     }
   }
   ```

   और हीरो के लिए एक समान प्रक्रिया करने के लिए एक `createHero()` फ़ंक्शन जोड़ें.

   ```javascript
   function createHero() {
     hero = new Hero(canvas.width / 2 - 45, canvas.height - canvas.height / 4);
     hero.img = heroImg;
     gameObjects.push(hero);
   }
   ```

   और अंत में, ड्राइंग शुरू करने के लिए एक `drawGameObjects()` फ़ंक्शन जोड़ें:

   ```javascript
   function drawGameObjects(ctx) {
     gameObjects.forEach((go) => go.draw(ctx));
   }
   ```

   अपने दुश्मनों को अपने नायक अंतरिक्ष यान पर आगे बढ़ना शुरू कर देना चाहिए!

---

## 🚀 चुनौती

जैसा कि आप देख सकते हैं, जब आप फ़ंक्शन और चर और कक्षाएं जोड़ना शुरू करते हैं तो आपका कोड 'स्पेगेटी कोड' में बदल सकता है. आप अपने कोड को बेहतर तरीके से कैसे व्यवस्थित कर सकते हैं ताकि यह अधिक पठनीय हो? अपने कोड को व्यवस्थित करने के लिए एक सिस्टम स्केच करें, भले ही वह अभी भी एक फ़ाइल में रहता हो.

## व्याख्यान के बाद की क्विज

[व्याख्यान के बाद की क्विज](https://happy-mud-02d95f10f.azurestaticapps.net/quiz/34?loc=hi)

## समीक्षा और स्व अध्ययन

जब हम फ्रेमवर्क का उपयोग किए बिना अपना गेम लिख रहे हैं, तो गेम के विकास के लिए कई जावास्क्रिप्ट-आधारित कैनवास फ्रेमवर्क हैं. कुछ करने के लिए कुछ समय ले लो [इन के बारे में पढ़ना](https://github.com/collections/javascript-game-engines).

## असाइनमेंट

[अपना कोड कमेंट करें](assignment.hi.md)
