# एक अंतरिक्ष खेल बनाएँ भाग ६: अंत और पुनः आरंभ

## प्री-रीडिंग क्विज

[प्री-रीडिंग क्विज](https://happy-mud-02d95f10f.azurestaticapps.net/quiz/39?loc=hi)

एक खेल में व्यक्त करने और _अंतिम स्थिति_ के विभिन्न तरीके हैं। यह गेम के निर्माता के रूप में यह कहना है कि खेल क्यों समाप्त हो गया है। यहाँ कुछ कारण हैं, अगर हम मान लें कि हम उस अंतरिक्ष खेल के बारे में बात कर रहे हैं जो आप अभी तक बना रहे हैं:

- **`N` दुश्मन के जहाज तबाह हो गए हैं**: यदि आप एक गेम को विभिन्न स्तरों में विभाजित करते हैं तो यह काफी सामान्य है कि आपको एक स्तर पूरा करने के लिए `N` दुश्मन जहाजों को नष्ट करने की आवश्यकता है
- **आपका जहाज नष्ट हो गया है**: यदि आपका जहाज नष्ट हो जाता है तो निश्चित रूप से ऐसे खेल हैं जहाँ आप खेल को खो देते हैं। एक और आम दृष्टिकोण यह है कि आपके पास जीवन की अवधारणा है। हर बार जब आपका जहाज नष्ट हो जाता है तो यह जीवन काट देता है। एक बार जब सभी जान चली गई तो आप खेल खो देते हैं.
- **आपने `N` अंक एकत्र किए हैं**: एक और सामान्य अंतिम स्थिति आपके लिए अंक एकत्र करने की है। आप कैसे अंक प्राप्त करते हैं, यह आपके ऊपर है, लेकिन दुश्मन के जहाज को नष्ट करने या शायद वस्तुओं को इकट्ठा करने जैसी विभिन्न गतिविधियों के लिए अंक प्रदान करना काफी सामान्य है, जब वे _गिर_ जाते हैं।.
- **एक स्तर पूरा करें**: इसमें कई स्थितियां शामिल हो सकती हैं जैसे कि `X` दुश्मन के जहाज नष्ट,` Y` अंक एकत्र या शायद एक विशिष्ट आइटम एकत्र किया गया है.

## पुनरारंभ

यदि लोग आपके खेल का आनंद लेते हैं, तो वे इसे फिर से खेलना चाहते हैं। एक बार खेल जो भी कारण से आप को पुनरारंभ करने के लिए एक विकल्प की पेशकश के लिए समाप्त होता है.

✅ इस बारे में थोड़ा सोचें कि आपको किन परिस्थितियों में गेम समाप्त होता है, और फिर आपको कैसे पुनरारंभ करने के लिए प्रेरित किया जाता है

## क्या बनना है

आप इन नियमों को अपने खेल में शामिल करेंगे:

1. **खेल जीतना**. एक बार सभी दुश्मन जहाजों को नष्ट कर दिए जाने के बाद, आप गेम जीतते हैं। इसके अतिरिक्त किसी प्रकार का विजय संदेश प्रदर्शित करें.
1. **पुनरारंभ**. एक बार जब आपका सारा जीवन खो जाता है या खेल जीत जाता है, तो आपको खेल को पुनः आरंभ करने का एक तरीका पेश करना चाहिए। याद है! आपको खेल को फिर से संगठित करना होगा और पिछले खेल की स्थिति को साफ करना चाहिए.

## अनुशंसित कदम

उन फ़ाइलों का पता लगाएँ जो आपके लिए `your-work` सब फ़ोल्डर में बनाई गई हैं। इसमें निम्नलिखित शामिल होना चाहिए:

```bash
-| assets
  -| enemyShip.png
  -| player.png
  -| laserRed.png
  -| life.png
-| index.html
-| app.js
-| package.json
```

आप टाइप करके अपना प्रोजेक्ट `your_work` फ़ोल्डर शुरू करें:

```bash
cd your-work
npm start
```

उपरोक्त पते पर एक HTTP सर्वर शुरू होगा `http: // localhost: 5000`। एक ब्राउज़र खोले और उस पतेको खोलें। आपका खेल खेलने योग्य अवस्था में होना चाहिए

> टिप: विज़ुअल स्टूडियो कोड में चेतावनियों से बचने के लिए, `window.onload` फ़ंक्शन को` gameLoopId` के रूप में (`let` के बिना) के रूप में संपादित करने के लिए संपादित करें, और स्वतंत्र रूप से फ़ाइल के शीर्ष पर gameLoopId की घोषणा करें, `let gameLoopId`;

### कोड जोड़े

1. **ट्रैक एंड कंडीशन**. उन कोडों को जोड़ें जो दुश्मनों की संख्या का ट्रैक रखते हैं, या यदि इन दो कार्यों को जोड़ते हुए नायक जहाज को नष्ट कर दिया गया है:

   ```javascript
   function isHeroDead() {
     return hero.life <= 0;
   }

   function isEnemiesDead() {
     const enemies = gameObjects.filter(
       (go) => go.type === "Enemy" && !go.dead
     );
     return enemies.length === 0;
   }
   ```

1. **संदेश संचालकों में तर्क जोड़ें**. इन स्थितियों को संभालने के लिए `EventEmitter` संपादित करें:

   ```javascript
   eventEmitter.on(Messages.COLLISION_ENEMY_LASER, (_, { first, second }) => {
     first.dead = true;
     second.dead = true;
     hero.incrementPoints();

     if (isEnemiesDead()) {
       eventEmitter.emit(Messages.GAME_END_WIN);
     }
   });

   eventEmitter.on(Messages.COLLISION_ENEMY_HERO, (_, { enemy }) => {
     enemy.dead = true;
     hero.decrementLife();
     if (isHeroDead()) {
       eventEmitter.emit(Messages.GAME_END_LOSS);
       return; // loss before victory
     }
     if (isEnemiesDead()) {
       eventEmitter.emit(Messages.GAME_END_WIN);
     }
   });

   eventEmitter.on(Messages.GAME_END_WIN, () => {
     endGame(true);
   });

   eventEmitter.on(Messages.GAME_END_LOSS, () => {
     endGame(false);
   });
   ```

1. **नए संदेश प्रकार जोड़ें**. इन संदेशों को स्थिरांक वस्तु में जोड़ें:

   ```javascript
   GAME_END_LOSS: "GAME_END_LOSS",
   GAME_END_WIN: "GAME_END_WIN",
   ```

1. **पुनः आरंभ कोड जोड़ें** कोड जो चयनित बटन के प्रेस पर गेम को पुनरारंभ करता है.

   1. **`Enter` की प्रेस सुनो**. इस प्रेस को सुनने के लिए अपनी विंडो के इवेंटलिस्ट को एडिट करें:

   ```javascript
    else if(evt.key === "Enter") {
       eventEmitter.emit(Messages.KEY_EVENT_ENTER);
     }
   ```

   1. **पुनः आरंभ संदेश जोड़ें**. इस संदेश को अपने संदेशों में लगातार जोड़ें:

      ```javascript
      KEY_EVENT_ENTER: "KEY_EVENT_ENTER",
      ```

1. **खेल के नियमों को लागू करें**. निम्नलिखित खेल नियमों को लागू करें:

   1. **खिलाड़ी शर्त जीता**. जब सभी दुश्मन जहाज नष्ट हो जाते हैं, तो एक जीत संदेश प्रदर्शित करें.

      1. सबसे पहले, एक `displayMessage()` फ़ंक्शन बनाएँ:

      ```javascript
      function displayMessage(message, color = "red") {
        ctx.font = "30px Arial";
        ctx.fillStyle = color;
        ctx.textAlign = "center";
        ctx.fillText(message, canvas.width / 2, canvas.height / 2);
      }
      ```

      1. एक `endGame()` फ़ंक्शन बनाएँ:

      ```javascript
      function endGame(win) {
        clearInterval(gameLoopId);

        // set a delay so we are sure any paints have finished
        setTimeout(() => {
          ctx.clearRect(0, 0, canvas.width, canvas.height);
          ctx.fillStyle = "black";
          ctx.fillRect(0, 0, canvas.width, canvas.height);
          if (win) {
            displayMessage(
              "Victory!!! Pew Pew... - Press [Enter] to start a new game Captain Pew Pew",
              "green"
            );
          } else {
            displayMessage(
              "You died !!! Press [Enter] to start a new game Captain Pew Pew"
            );
          }
        }, 200);
      }
      ```

   1. **तर्क पुनः आरंभ**. जब सभी जीवन खो जाते हैं या खिलाड़ी खेल जीत जाता है, तो प्रदर्शित करें कि खेल को फिर से शुरू किया जा सकता है। इसके अलावा खेल को पुनरारंभ करें जब _पुनरारंभ_ की हिट होती है (आप तय कर सकते हैं कि पुनरारंभ करने के लिए किस की को मैप किया जाना चाहिए).

      1. `ResetGame()` फ़ंक्शन बनाएँ:

      ```javascript
      function resetGame() {
        if (gameLoopId) {
          clearInterval(gameLoopId);
          eventEmitter.clear();
          initGame();
          gameLoopId = setInterval(() => {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            ctx.fillStyle = "black";
            ctx.fillRect(0, 0, canvas.width, canvas.height);
            drawPoints();
            drawLife();
            updateGameObjects();
            drawGameObjects(ctx);
          }, 100);
        }
      }
      ```

   1. `InitGame()` में गेम को रीसेट करने के लिए` EventEmitter` में कॉल जोड़ें:

      ```javascript
      eventEmitter.on(Messages.KEY_EVENT_ENTER, () => {
        resetGame();
      });
      ```

   1. EventEmitter में `clear()` फ़ंक्शन जोड़ें:

      ```javascript
      clear() {
        this.listeners = {};
      }
      ```

👽 💥 🚀 बधाई हो, कैप्टन! आपका खेल पूरा हो गया है! बहुत बढ़िया! 🚀 💥 👽

---

## 🚀 चुनौती

एक ध्वनि जोड़ें! क्या आप अपने गेम खेलने को बढ़ाने के लिए एक ध्वनि जोड़ सकते हैं, हो सकता है कि जब कोई लेजर हिट हो, या नायक मर जाए या जीत जाए? जावास्क्रिप्ट का उपयोग करके ध्वनि कैसे खेलें, यह जानने के लिए इस [सैंडबॉक्स](https://www.w3schools.com/jsref/tryit.asp?filename=tryjsref_audio_play) पर एक नज़र डालें

## पोस्ट-व्याख्यान प्रश्नोत्तरी

[पोस्ट-व्याख्यान प्रश्नोत्तरी](https://happy-mud-02d95f10f.azurestaticapps.net/quiz/40?loc=hi)

## समीक्षा और स्व अध्ययन

आपका असाइनमेंट एक फ्रेश सैंपल गेम बनाना है, इसलिए वहां के कुछ दिलचस्प गेम्स को देखें कि आप किस प्रकार के गेम का निर्माण कर सकते हैं.

## असाइनमेंट

[एक नमूना खेल बनाएँ](assignment.hi.md)
