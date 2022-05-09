# एक अंतरिक्ष खेल बनाएँ भाग 1: परिचय

![वीडियो](../../images/pewpew.gif)

## लेक्चर से पहलेकी क्विज

[लेक्चर से पहलेकी क्विज ](https://happy-mud-02d95f10f.azurestaticapps.net/quiz/29?loc=hi)

### खेल के विकास में इन्हेरिटेंस और कम्पोजीशन

पहले के पाठों में, आपके द्वारा बनाए गए ऐप्स के डिज़ाइन आर्किटेक्चर के बारे में ज्यादा चिंता करने की ज़रूरत नहीं थी, क्योंकि प्रोजेक्ट बहुत कम दायरे में थे. हालाँकि, जब आपके अनुप्रयोग आकार और दायरे में बढ़ते हैं, तो वास्तु निर्णय एक बड़ी चिंता बन जाते हैं. जावास्क्रिप्ट में बड़ा अनुप्रयोग बनाने के लिए दो प्रमुख दृष्टिकोण हैं: _composition_ या _inheritance_. दोनों के पक्ष और विपक्ष हैं लेकिन किसी खेल के संदर्भ में उन्हें समझते हैं .

✅ अब तक लिखी गई सबसे प्रसिद्ध प्रोग्रामिंग किताबों में से एक है [डिजाइन पैटर्न](https://en.wikipedia.org/wiki/Design_Patterns)

एक गेम में आपके पास `game objects` हैं जो एक स्क्रीन पर मौजूद ऑब्जेक्ट हैं. इसका मतलब है कि उनके पास एक कार्टेशियन समन्वय प्रणाली पर एक स्थान है, जिसमें एक `x` और` y` समन्वय है. जैसा कि आप एक गेम विकसित करते हैं, आप देखेंगे कि आपके सभी गेम ऑब्जेक्ट में एक मानक गुण है, जो आपके द्वारा बनाए जाने वाले प्रत्येक गेम के लिए सामान्य है, अर्थात् तत्व हैं:

- **location-based** अधिकांश, यदि सभी नहीं हैं, तो खेल तत्व स्थान आधारित हैं. इसका मतलब है कि उनके पास एक स्थान है, एक `x` और` y`.
- **movable** ये ऐसी वस्तुएं हैं जो एक नए स्थान पर जा सकती हैं. यह आम तौर पर एक नायक, एक राक्षस या एक एनपीसी (एक गैर खिलाड़ी चरित्र) है, लेकिन उदाहरण के लिए, एक पेड़ जैसी स्थिर वस्तु नहीं.
- **self-destructing** ये ऑब्जेक्ट केवल समय की एक निर्धारित अवधि के लिए मौजूद होते हैं, इससे पहले कि वे स्वयं को हटाने के लिए सेट करें. आमतौर पर यह एक `मृत` या` नष्ट` बूलियन द्वारा दर्शाया जाता है जो गेम इंजन को संकेत देता है कि इस ऑब्जेक्ट को अब प्रस्तुत नहीं किया जाना चाहिए.
- **cool-down** 'कूल-डाउन' एक अल्पकालिक वस्तुओं में से एक विशिष्ट गुण है. एक विशिष्ट उदाहरण एक विस्फोट की तरह पाठ या चित्रमय प्रभाव का एक टुकड़ा है जिसे केवल कुछ मिलीसेकंड के लिए देखा जाना चाहिए.

✅ पैक मैन जैसे खेल के बारे में सोचें. क्या आप इस खेल में ऊपर सूचीबद्ध चार ऑब्जेक्ट प्रकारों की पहचान कर सकते हैं?

### व्यवहार व्यक्त करना

हम ऊपर वर्णित सभी व्यवहार हैं जो गेम ऑब्जेक्ट्स हो सकते हैं . तो हम उन सबको को कैसे एनकोड करेंगे? हम इस व्यवहार को या तो क्लासेज या objects से संबंधित विधियों के रूप में व्यक्त कर सकते हैं.

**क्लासेज**

विचार एक क्लास के लिए एक निश्चित व्यवहार को जोड़ने के लिए `inheritence` के साथ संयोजन में `inheritence` का उपयोग करने के लिए है.

✅ समझने के लिए इन्हेरिटेंस एक महत्वपूर्ण अवधारणा है। [एमडीएन के इन्हेरिटेंस के बारे में लेख](https://developer.mozilla.org/docs/Web/JavaScript/Inheritance_and_the_prototyp_chain) पर और जानें.

कोड के माध्यम से व्यक्त, एक गेम ऑब्जेक्ट आमतौर पर इस तरह दिख सकता है:

```javascript
//set up the class GameObject
class GameObject {
  constructor(x, y, type) {
    this.x = x;
    this.y = y;
    this.type = type;
  }
}

//this class will extend the GameObject's inherent class properties
class Movable extends GameObject {
  constructor(x, y, type) {
    super(x, y, type);
  }

  //this movable object can be moved on the screen
  moveTo(x, y) {
    this.x = x;
    this.y = y;
  }
}

//this is a specific class that extends the Movable class, so it can take advantage of all the properties that it inherits
class Hero extends Movable {
  constructor(x, y) {
    super(x, y, "Hero");
  }
}

//this class, on the other hand, only inherits the GameObject properties
class Tree extends GameObject {
  constructor(x, y) {
    super(x, y, "Tree");
  }
}

//a hero can move...
const hero = new Hero();
hero.moveTo(5, 5);

//but a tree cannot
const tree = new Tree();
```

✅ एक पैक-मैन हीरो (इंकी, पिंकी या ब्लंकी, उदाहरण के लिए) को फिर से लागू करने के लिए कुछ मिनट लें और यह जावास्क्रिप्ट में कैसे लिखा जाएगा.

**कम्पोजीशन**

ऑब्जेक्ट इन्हेरिटेंस को संभालने का एक अलग तरीका _Composition_ का उपयोग करते है. फिर, ऑब्जेक्ट अपने व्यवहार को इस तरह व्यक्त करते हैं:

```javascript
//create a constant gameObject
const gameObject = {
  x: 0,
  y: 0,
  type: ''
};

//...and a constant movable
const movable = {
  moveTo(x, y) {
    this.x = x;
    this.y = y;
  }
}
//then the constant movableObject is composed of the gameObject and movable constants
const movableObject = {...gameObject, ...movable};

//then create a function to create a new Hero who inherits the movableObject properties
function createHero(x, y) {
  return {
    ...movableObject,
    x,
    y,
    type: 'Hero'
  }
}
//...and a static object that inherits only the gameObject properties
function createStatic(x, y, type) {
  return {
    ...gameObject
    x,
    y,
    type
  }
}
//create the hero and move it
const hero = createHero(10,10);
hero.moveTo(5,5);
//and create a static tree which only stands around
const tree = createStatic(0,0, 'Tree');
```

**मुझे किस पैटर्न का उपयोग करना चाहिए?**

यह आपके ऊपर है कि आप किस पैटर्न को चुनते हैं । जावास्क्रिप्ट इन दोनों प्रतिमानों का समर्थन करता है.

--

खेल के विकास में एक और पैटर्न आम है जो गेम के उपयोगकर्ता अनुभव और प्रदर्शन को संभालने की समस्या को संबोधित करता है.

## Pub/sub पैटर्न

✅ Pub/Sub अर्थ 'publish-subscribe' है

यह पैटर्न इस विचार को संबोधित करता है कि आपके आवेदन के असमान भागों को एक दूसरे के बारे में पता नहीं होना चाहिए. यह देखने के लिए बहुत आसान बनाता है कि सामान्य रूप से क्या हो रहा है अगर विभिन्न भागों को अलग किया जाता है. यह भी अगर आप की जरूरत है अचानक व्यवहार बदलने के लिए आसान बनाता है. हम इसे कैसे पूरा करेंगे? हम कुछ अवधारणाओं को स्थापित करके ऐसा करते हैं:

- **message**: एक संदेश आमतौर पर एक टेक्स्ट स्ट्रिंग होता है जिसमें वैकल्पिक पेलोड (डेटा का एक टुकड़ा जो स्पष्ट करता है कि संदेश क्या है). एक खेल में एक विशिष्ट संदेश `KEY_PRESSED_ENTER` हो सकता है।
- **publisher**: यह तत्व एक संदेश \_प्रकाशित\_ करता है और इसे सभी ग्राहकों को भेजता है.
- **subscriber**: यह तत्व विशिष्ट संदेशों को \_सूचीबद्ध\_ करता है और इस संदेश को प्राप्त करने के परिणामस्वरूप कुछ कार्य करता है, जैसे कि लेजर फायरिंग.

कार्यान्वयन आकार में काफी छोटा है लेकिन यह एक बहुत शक्तिशाली पैटर्न है। यहां बताया गया है कि इसे कैसे लागू किया जा सकता है:

```javascript
//set up an EventEmitter class that contains listeners
class EventEmitter {
  constructor() {
    this.listeners = {};
  }
  //when a message is received, let the listener to handle its payload
  on(message, listener) {
    if (!this.listeners[message]) {
      this.listeners[message] = [];
    }
    this.listeners[message].push(listener);
  }
  //when a message is sent, send it to a listener with some payload
  emit(message, payload = null) {
    if (this.listeners[message]) {
      this.listeners[message].forEach((l) => l(message, payload));
    }
  }
}
```

उपरोक्त कोड का उपयोग करने के लिए हम एक बहुत छोटा कार्यान्वयन बना सकते हैं:

```javascript
//set up a message structure
const Messages = {
  HERO_MOVE_LEFT: "HERO_MOVE_LEFT",
};
//invoke the eventEmitter you set up above
const eventEmitter = new EventEmitter();
//set up a hero
const hero = createHero(0, 0);
//let the eventEmitter know to watch for messages pertaining to the hero moving left, and act on it
eventEmitter.on(Messages.HERO_MOVE_LEFT, () => {
  hero.move(5, 0);
});

//set up the window to listen for the keyup event, specifically if the left arrow is hit, emit a message to move the hero left
window.addEventListener("keyup", (evt) => {
  if (evt.key === "ArrowLeft") {
    eventEmitter.emit(Messages.HERO_MOVE_LEFT);
  }
});
```

ऊपर हम एक कीबोर्ड ईवेंट कनेक्ट करते हैं, `ArrowLeft` और `HERO_MOVE_LEFT` संदेश भेजें. हम उस संदेश को सुनते हैं और परिणामस्वरूप `hero` को स्थानांतरित करते हैं. इस पैटर्न के साथ ताकत यह है कि इवेंट लिस्टनेर और हीरो एक दूसरे के बारे में नहीं जानते हैं. आप `ArrowLeft` को `A` की के लिए रीमैप कर सकते हैं. इसके अतिरिक्त यह `ArrowLeft` पर पूरी तरह से अलग कुछ करने के लिए संभव हो जाएगा करने के लिए EventEmitter की `on` समारोह में कुछ संपादन:

```javascript
eventEmitter.on(Messages.HERO_MOVE_LEFT, () => {
  hero.move(5, 0);
});
```

जब आपका गेम बढ़ता है तो चीजें अधिक जटिल हो जाती हैं, यह पैटर्न जटिलता में समान रहता है और आपका कोड साफ रहता है. यह वास्तव में इस पैटर्न को अपनाने की सिफारिश की गई है.

---

## 🚀 चुनौती

इस बारे में सोचें कि pub-sub पैटर्न एक खेल को कैसे बढ़ा सकते हैं. किन हिस्सों को घटनाओं का उत्सर्जन करना चाहिए, और खेल को उन्हें कैसे प्रतिक्रिया देनी चाहिए? अब आपका मौका रचनात्मक बनने का है, एक नए खेल के बारे में सोचकर और उसके भागों का व्यवहार कैसे हो सकता है.

## लेक्चर बाद की क्विज

[लेक्चर बाद की क्विज](https://happy-mud-02d95f10f.azurestaticapps.net/quiz/30?loc=hi)

## समीक्षा और स्व अध्ययन

pub/sub[के बारे में पढ़े](https://docs.microsoft.com/azure/altecture/patterns/publisher-subscriber?WT.mc_id=academic-13441-cxa) और अधिक जानें .

## असाइनमेंट

[एक खेल का नका](assignment.hi.md)
