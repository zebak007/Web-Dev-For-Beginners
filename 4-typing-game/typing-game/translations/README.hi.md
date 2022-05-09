# इवेंट्स का उपयोग करके एक गेम बनाना

## पूर्व व्याख्यान प्रश्नोत्तरी

[पूर्व व्याख्यान प्रश्नोत्तरी](https://happy-mud-02d95f10f.azurestaticapps.net/quiz/21?loc=hi)

## इवेंट संचालित प्रोग्रामिंग

ब्राउज़र आधारित एप्लिकेशन बनाते समय, हम उपयोगकर्ता के लिए एक ग्राफिकल यूजर इंटरफेस (जीयूआई) प्रदान करते हैं, जो कि हमने जो बनाया है, उसके साथ इंटरैक्ट करने के लिए। ब्राउज़र के साथ बातचीत करने का सबसे आम तरीका विभिन्न तत्वों में क्लिक और टाइपिंग है। एक डेवलपर के रूप में हमारे सामने जो चुनौती है, वह यह है कि हम नहीं जानते कि वे कब इन ऑपरेशनों को करने जा रहे हैं!

[ईवेंट संचालित प्रोग्रामिंग] (https://en.wikipedia.org/wiki/Event-driven_programming) प्रोग्रामिंग का प्रकार जो हमें अपने GUI को बनाने के लिए करने की आवश्यकता है। यदि हम इस वाक्यांश को थोड़ा तोड़ते हैं, तो हम यहाँ मुख्य शब्द ** ईवेंट ** देखते हैं। [ईवेंट](https://www.merriam-webster.com/dEDIA/event), मरियम-वेबस्टर के अनुसार, "कुछ ऐसा होता है" के रूप में परिभाषित किया गया है। यह हमारी स्थिति का पूरी तरह से वर्णन करता है। हम जानते हैं कि कुछ ऐसा होने जा रहा है जिसके लिए हम प्रतिक्रिया में कुछ कोड निष्पादित करना चाहते हैं, लेकिन हम नहीं जानते कि यह कब होगा।

जिस तरह से हम कोड के एक भाग को चिह्नित करते हैं जिसे हम निष्पादित करना चाहते हैं वह एक फ़ंक्शन बनाकर है। जब हम [प्रक्रियात्मक प्रोग्रामिंग] (https://en.wikipedia.org/wiki/Procedural_programming) के बारे में सोचते हैं, तो कार्यों को एक विशिष्ट क्रम में बुलाया जाता है। यही बात इवेंट संचालित प्रोग्रामिंग के साथ सही होने वाली है। अंतर **कैसे** कार्यों को कहा जाएगा।

ईवेंट्स (बटन क्लिकिंग, टाइपिंग आदि) को संभालने के लिए, हम **ईवेंट श्रोताओं** को रजिस्टर करते हैं। एक ईवेंट श्रोता एक ऐसा फंक्शन है जो किसी घटना को होने के लिए सुनता है और प्रतिक्रिया में निष्पादित करता है। इवेंट श्रोता यूआई को अपडेट कर सकते हैं, सर्वर पर कॉल कर सकते हैं, या उपयोगकर्ता की कार्रवाई के जवाब में और कुछ भी किया जा सकता है। हम एक घटना श्रोता को [addEventListener](https://developer.mozilla.org/docs/Web/API/EventTarget/addEventListener) का उपयोग करके, और निष्पादित करने के लिए एक फ़ंक्शन प्रदान करके जोड़ते हैं।

> **ध्यान दें:** यह ध्यान देने योग्य है कि घटना श्रोताओं को बनाने के कई तरीके हैं। आप अनाम फ़ंक्शंस का उपयोग कर सकते हैं, या नामित नाम बना सकते हैं। आप विभिन्न शॉर्टकट का उपयोग कर सकते हैं, जैसे कि `click` प्रॉपर्टी सेट करना, या `addEventListener` का उपयोग करना। हमारे अभ्यास में हम `addEventLister` और अनाम कार्यों पर ध्यान केंद्रित करने जा रहे हैं, क्योंकि यह संभवतः सबसे आम तकनीक वेब डेवलपर्स का उपयोग है। यह सबसे अधिक लचीली भी है, क्योंकि सभी घटनाओं के लिए `addEventListener` काम करता है, और इवेंट नाम को एक पैरामीटर के रूप में प्रदान किया जा सकता है।

### आम इवेंट्स

एप्लिकेशन बनाते समय आपको सुनने के लिए [दर्जनों इवेंट](https://developer.mozilla.org/docs/Web/Events) उपलब्ध हैं। मूल रूप से एक पृष्ठ पर एक उपयोगकर्ता कुछ भी करता है, एक घटना को बढ़ाता है, जो आपको यह सुनिश्चित करने के लिए बहुत शक्ति देता है कि वे आपकी इच्छा का अनुभव प्राप्त करें। सौभाग्य से, आपको आम तौर पर केवल कुछ मुट्ठी भर घटनाओं की आवश्यकता होगी। यहां कुछ सामान्य बातें हैं (दोनों में से एक का उपयोग हम अपने खेल को बनाते समय करेंगे)

- [click](https://developer.mozilla.org/docs/Web/API/Element/click_event): उपयोगकर्ता ने कुछ पर क्लिक किया, आमतौर पर एक बटन या हाइपरलिंक
- [contextmenu](https://developer.mozilla.org/docs/Web/API/Element/contextmenu_event): उपयोगकर्ता ने सही माउस बटन क्लिक किया
- [select](https://developer.mozilla.org/docs/Web/API/Element/select_event): उपयोगकर्ता ने कुछ टेक्स्ट पर प्रकाश डाला
- [input](https://developer.mozilla.org/docs/Web/API/Element/input_event): उपयोगकर्ता कुछ टेक्स्ट इनपुट करता है

## खेल का निर्माण

हम जावास्क्रिप्ट में ईवेंट कैसे काम करते हैं, यह जानने के लिए एक गेम बनाने जा रहे हैं। हमारा खेल एक खिलाड़ी के टाइपिंग कौशल का परीक्षण करने जा रहा है, जो सभी डेवलपर्स के पास सबसे कम क्षमता वाले कौशल में से एक है। हम सभी को अपनी टाइपिंग का अभ्यास करना चाहिए! खेल का सामान्य प्रवाह इस तरह दिखेगा:

- प्लेयर स्टार्ट बटन पर क्लिक करता है और टाइप करने के लिए एक उद्धरण के साथ प्रस्तुत किया जाता है
- प्लेयर टेक्स्ट बॉक्स में जितनी जल्दी हो सके उद्धरण टाइप करें
  - जैसा कि प्रत्येक शब्द पूरा हो गया है, अगले एक को हाइलाइट किया गया है
  - यदि खिलाड़ी के पास टाइपो है, तो टेक्स्टबॉक्स को लाल रंग में अपडेट किया जाता है
  - जब खिलाड़ी बोली को पूरा करता है, तो एक सफल संदेश बीते हुए समय के साथ प्रदर्शित होता है

चलो हमारे खेल का निर्माण करें, और घटनाओं के बारे में जानें!

### फ़ाइल संरचना

हमें कुल तीन फ़ाइलों की आवश्यकता है: **index.html**, **script.js** और **style.css**. आइए उन लोगों की स्थापना करके शुरू करें, जो हमारे लिए जीवन को थोड़ा आसान बनाते हैं।

- कंसोल या टर्मिनल विंडो खोलकर और निम्न आदेश जारी करके अपने काम के लिए एक नया फ़ोल्डर बनाएँ:

```bash
# Linux or macOS
mkdir typing-game && cd typing-game

# Windows
md typing-game && cd typing-game
```

- विजुअल स्टूडियो कोड खोलें

```bash
code .
```

- विज़ुअल स्टूडियो कोड के फ़ोल्डर में निम्नलिखित नामों के साथ तीन फाइलें जोड़ें:
  - index.html
  - script.js
  - style.css

## उपयोगकर्ता इंटरफ़ेस बनाएँ

यदि हम आवश्यकताओं का पता लगाते हैं, तो हम जानते हैं कि हमें अपने HTML पृष्ठ पर मुट्ठी भर तत्वों की आवश्यकता है। यह एक रेसिपी की तरह है, जहाँ हमें कुछ सामग्री की आवश्यकता होती है:

- उपयोगकर्ता टाइप करने के लिए बोली प्रदर्शित करने के लिए कहीं
- कहीं कोई संदेश प्रदर्शित करने के लिए, जैसे कोई सफलता संदेश
- टाइपिंग के लिए एक टेक्स्टबॉक्स
- एक स्टार्ट बटन

उनमें से प्रत्येक को आईडी की आवश्यकता होगी ताकि हम अपने जावास्क्रिप्ट में उनके साथ काम कर सकें। हम सीएसएस और जावास्क्रिप्ट फ़ाइलों के संदर्भ भी जोड़ेंगे जिन्हें हम बनाने जा रहे हैं।

**index.html** नामक एक नई फ़ाइल बनाएँ। निम्नलिखित HTML जोड़ें:

```html
<!-- inside index.html -->
<html>
<head>
  <title>Typing game</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <h1>Typing game!</h1>
  <p>Practice your typing skills with a quote from Sherlock Holmes. Click **start** to begin!</p>
  <p id="quote"></p> <!-- This will display our quote -->
  <p id="message"></p> <!-- This will display any status messages -->
  <div>
    <input type="text" aria-label="current word" id="typed-value" /> <!-- The textbox for typing -->
    <button type="button" id="start">Start</button> <!-- To start the game -->
  </div>
  <script src="script.js"></script>
</body>
</html>
```

### एप्लिकेशन लॉन्च करें

यह देखना हमेशा सबसे अच्छा होता है कि कैसे चीजें देखें। चलो हमारे आवेदन का शुभारंभ करें। विजुअल स्टूडियो कोड के लिए एक अद्भुत एक्सटेंशन है जिसे [लाइव सर्वर](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer) कहा जाता है, जो आपके आवेदन को सहेजने और हर बार सहेजने के लिए ब्राउज़र को ताज़ा करेगा।

- लिंक का पालन करके और **स्थापित** पर क्लिक करके [लाइव सर्वर](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer) स्थापित करें
  - आपको विज़ुअल स्टूडियो कोड खोलने के लिए ब्राउज़र द्वारा और फिर इंस्टॉलेशन करने के लिए विज़ुअल स्टूडियो कोड द्वारा संकेत दिया जाएगा
  - संकेत मिलने पर विज़ुअल स्टूडियो कोड को पुनरारंभ करें
- विजुअल स्टूडियो कोड में स्थापित होने के बाद, कमांड पलेट खोलने के लिए Ctrl-Shift-P (या Cmd-Shift-P) पर क्लिक करें।
- **Live Server: Open with Live Server** लिखे
  - लाइव सर्वर आपके एप्लिकेशन को होस्ट करना शुरू कर देगा
- एक ब्राउज़र खोलें और **https://localhost:5500** पर नेविगेट करें
- अब आपको आपके द्वारा बनाया गया पेज देखना चाहिए!

चलो कुछ कार्यक्षमता जोड़ते हैं।

## सीएसएस जोड़ें

हमारे HTML के निर्माण के साथ, मुख्य स्टाइलिंग के लिए CSS जोड़ते हैं। हमें उस शब्द को हाइलाइट करने की आवश्यकता है जो खिलाड़ी को टाइप करना चाहिए, और यदि उन्होंने टाइप किया है तो टेक्स्टबॉक्स को रंगीन करें। हम इसे दो वर्गों के साथ करेंगे।

**style.css** नाम की एक नई फ़ाइल बनाएं और निम्न सिंटैक्स जोड़ें।

```css
/* inside style.css */
.highlight {
  background-color: yellow;
}

.error {
  background-color: lightcoral;
  border: red;
}
```

✅ जब यह सीएसएस की बात आती है तो आप अपने पेज को लेआउट कर सकते हैं लेकिन आप इसे पसंद कर सकते हैं थोड़ा समय लें और पृष्ठ को अधिक आकर्षक बनाएं:

- एक अलग फ़ॉन्ट चुनें
- हेडर को कलर करें
- आइटम का आकार बदलें

## जावास्क्रिप्ट

हमारे यूआई के साथ, यह जावास्क्रिप्ट पर हमारा ध्यान केंद्रित करने का समय है जो तर्क प्रदान करेगा। हम इसे मुट्ठी भर चरणों में तोड़ने जा रहे हैं:

- [स्थिरांक बनाएँ]](#add-the-constants)
- [खेल शुरू करने के लिए इवेंट श्रोता](#add-start-logic)
- [टाइप करने के लिए ईवेंट श्रोता](#add-typing-logic)

लेकिन सबसे पहले, **script.js** नामक एक नई फ़ाइल बनाएं।

### स्थिरांक जोड़ें

प्रोग्रामिंग के लिए हमारे जीवन को थोड़ा आसान बनाने के लिए हमें कुछ वस्तुओं की आवश्यकता है। फिर, एक नुस्खा के समान, यहाँ हम क्या करेंगे:

- सभी उद्धरणों की सूची के साथ अरै
- वर्तमान बोली के लिए सभी शब्दों को संग्रहीत करने के लिए खाली अरै
- खिलाड़ी शब्द के सूचकांक को संग्रहीत करने के लिए स्थान वर्तमान में टाइप कर रहा है
- जिस समय खिलाड़ी ने शुरुआत पर क्लिक किया

हम UI तत्वों के संदर्भ भी चाहते हैं:

- टेक्सटबॉक्स (**typed-value**)
- क्वोट डिस्प्ले (**quote**)
- मैसेज (**message**)

```javascript
// inside script.js
// all of our quotes
const quotes = [
    'When you have eliminated the impossible, whatever remains, however improbable, must be the truth.',
    'There is nothing more deceptive than an obvious fact.',
    'I ought to know by this time that when a fact appears to be opposed to a long train of deductions it invariably proves to be capable of bearing some other interpretation.',
    'I never make exceptions. An exception disproves the rule.',
    'What one man can invent another can discover.',
    'Nothing clears up a case so much as stating it to another person.',
    'Education never ends, Watson. It is a series of lessons, with the greatest for the last.',
];
// store the list of words and the index of the word the player is currently typing
let words = [];
let wordIndex = 0;
// the starting time
let startTime = Date.now();
// page elements
const quoteElement = document.getElementById('quote');
const messageElement = document.getElementById('message');
const typedValueElement = document.getElementById('typed-value');
```

✅ आगे बढ़ो और अपने खेल के लिए अधिक उद्धरण जोड़ें

> **नोट:** हम तत्वों को पुनः प्राप्त कर सकते हैं जब भी हम `document.getElementById` का उपयोग करके कोड में चाहते हैं। इस तथ्य के कारण कि हम नियमित रूप से इन तत्वों को संदर्भित करने जा रहे हैं, हम स्थिरांक के साथ स्थिरांक का उपयोग करके स्थिरांक से बचने जा रहे हैं। फ्रेमवर्क जैसे कि [Vue.js](https://vuejs.org/) या [रिएक्ट](https://reactjs.org/) आपको अपने कोड को बेहतर बनाने में मदद कर सकते हैं।

`const`,` let` और `var` का उपयोग करके वीडियो देखने के लिए एक मिनट का समय लें

[![चर के प्रकार](https://img.youtube.com/vi/JNIXfGiDWM8/0.jpg)](https://youtube.com/watch?v=JNIXfGiDWM8 "चर के प्रकार")

> चरों के बारे में वीडियो के लिए ऊपर दी गई छवि पर क्लिक करें।

### प्रारंभ तर्क जोड़ें

गेम शुरू करने के लिए, प्लेयर स्टार्ट पर क्लिक करेगा। बेशक, हम नहीं जानते कि वे कब शुरू करने जा रहे हैं। यह वह जगह है जहाँ एक [इवेंट श्रोता](https://developer.mozilla.org/docs/Web/API/EventTarget/addEventListener) खेल में आता है। एक ईवेंट श्रोता हमें कुछ होने (किसी घटना) के लिए सुनने और प्रतिक्रिया में कोड निष्पादित करने की अनुमति देगा। हमारे मामले में, हम उस कोड को निष्पादित करना चाहते हैं जब उपयोगकर्ता प्रारंभ पर क्लिक करता है।

जब उपयोगकर्ता **प्रारंभ** पर क्लिक करता है, तो हमें एक उद्धरण का चयन करना होगा, उपयोगकर्ता इंटरफ़ेस सेटअप करना और वर्तमान शब्द और समय के लिए सेटअप ट्रैकिंग करना होगा। नीचे दिए गए जावास्क्रिप्ट को आपको जोड़ना होगा; हम स्क्रिप्ट ब्लॉक के ठीक बाद इस पर चर्चा करते हैं।

```javascript
// at the end of script.js
document.getElementById('start').addEventListener('click', () => {
  // get a quote
  const quoteIndex = Math.floor(Math.random() * quotes.length);
  const quote = quotes[quoteIndex];
  // Put the quote into an array of words
  words = quote.split(' ');
  // reset the word index for tracking
  wordIndex = 0;

  // UI updates
  // Create an array of span elements so we can set a class
  const spanWords = words.map(function(word) { return `<span>${word} </span>`});
  // Convert into string and set as innerHTML on quote display
  quoteElement.innerHTML = spanWords.join('');
  // Highlight the first word
  quoteElement.childNodes[0].className = 'highlight';
  // Clear any prior messages
  messageElement.innerText = '';

  // Setup the textbox
  // Clear the textbox
  typedValueElement.value = '';
  // set focus
  typedValueElement.focus();
  // set the event handler

  // Start the timer
  startTime = new Date().getTime();
});
```

चलो कोड को तोड़ते है

- ट्रैकिंग शब्द सेट करें
  - [math.floor](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Math/floor) और [math.random](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Math/random) का उपयोग करके हम `quotes` सरणी से एक उद्धरण का चयन करने के लिए अनुमति देते हैं।
  - हम `quote` को `words` की एक सरणी में बदलते हैं, इसलिए हम उस खिलाड़ी को ट्रैक कर सकते हैं जो खिलाड़ी वर्तमान में टाइप कर रहा है
  - `wordIndex` 0 पर सेट है, क्योंकि खिलाड़ी पहले शब्द पर शुरू होगा
- यूआई सेटअप करें
  - `spanWords` की एक सरणी बनाएं, जिसमें `span` तत्व के अंदर प्रत्येक शब्द होता है
    - यह हमें प्रदर्शन पर शब्द को उजागर करने की अनुमति देगा
  - एक स्ट्रिंग बनाने के लिए `join` करने के लिए अरै का उपयोग करें जिसे हम `quoteElement` पर `innerHTML` अपडेट करने के लिए उपयोग कर सकते हैं
    - यह खिलाड़ी को बोली प्रदर्शित करेगा
  - पीले रंग के रूप में हाइलाइट करने के लिए `highlight` के लिए पहले `span` तत्व का `className` सेट करें
  - `messageElement` को `''` पर सेट करके `messageElement` को साफ करें
- टेक्स्टबॉक्स सेट करें
  - `typedValueElement` पर वर्तमान `value` को साफ़ करें
  - `focus` को 'typedValueElement' पर सेट करें
- `getTime` कहकर टाइमर शुरू करें

### टाइपिंग तर्क जोड़ें

खिलाड़ी के प्रकार के रूप में, एक `input` घटना को उठाया जाएगा। यह ईवेंट श्रोता यह सुनिश्चित करने के लिए जांच करेगा कि खिलाड़ी शब्द को सही ढंग से टाइप कर रहा है, और गेम की वर्तमान स्थिति को संभाल सकता है। **script.js** पर लौटकर, निम्नलिखित कोड को अंत में जोड़ें। हम इसे बाद में तोड़ देंगे।

```javascript
// at the end of script.js
typedValueElement.addEventListener('input', () => {
  // Get the current word
  const currentWord = words[wordIndex];
  // get the current value
  const typedValue = typedValueElement.value;

  if (typedValue === currentWord && wordIndex === words.length - 1) {
    // end of sentence
    // Display success
    const elapsedTime = new Date().getTime() - startTime;
    const message = `CONGRATULATIONS! You finished in ${elapsedTime / 1000} seconds.`;
    messageElement.innerText = message;
  } else if (typedValue.endsWith(' ') && typedValue.trim() === currentWord) {
    // end of word
    // clear the typedValueElement for the new word
    typedValueElement.value = '';
    // move to the next word
    wordIndex++;
    // reset the class name for all elements in quote
    for (const wordElement of quoteElement.childNodes) {
      wordElement.className = '';
    }
    // highlight the new word
    quoteElement.childNodes[wordIndex].className = 'highlight';
  } else if (currentWord.startsWith(typedValue)) {
    // currently correct
    // highlight the next word
    typedValueElement.className = '';
  } else {
    // error state
    typedValueElement.className = 'error';
  }
});
```

कोड को तोड़ दो! हम वर्तमान शब्द को पकड़कर शुरू करते हैं और खिलाड़ी ने इस प्रकार अब तक टाइप किया है। फिर हमारे पास झरना तर्क है, जहां हम जांचते हैं कि क्या उद्धरण पूरा है, शब्द पूरा है, शब्द सही है, या (अंत में), अगर कोई त्रुटि है।

- उद्धरण पूर्ण है, `typedValue` द्वारा `currentWord` के बराबर होने का संकेत दिया गया है, और `wordIndex` को  `words` की `length` से कम के बराबर किया जा रहा है
  - वर्तमान समय से `startTime` घटाकर` elapsedTime` की गणना करें
  - मिलीसेकंड से सेकंड में परिवर्तित करने के लिए `elapsedTime` को 1,000 से विभाजित करें
  - एक सफलता संदेश प्रदर्शित करें
- शब्द पूरा हो गया है, जो `typedValue` द्वारा एक स्थान के साथ समाप्त होने का संकेत है (एक शब्द का अंत) और `typedValue` को` currentWord` के बराबर किया जा रहा है
  - अगले शब्द को टाइप करने की अनुमति देने के लिए `typedElement` to `''` पर `value` सेट करें
  - वृद्धि `wordIndex` अगले शब्द पर ले जाने के लिए
  - प्रदर्शन को फिर से प्रदर्शित करने के लिए `className` को `''` के लिए `quoteElement` के सभी `childNodes` के माध्यम से लूप करें
  - वर्तमान शब्द के `className` को टाइप करने के लिए अगले शब्द के रूप में फ़्लैग करने के लिए `highlight` पर सेट करें
- वर्तमान में शब्द सही ढंग से टाइप किया गया है (लेकिन पूरा नहीं), `currentWord` द्वारा इंगित `typedValue` से शुरू हुआ
  - सुनिश्चित करें कि `typeNalueElement` को `className` को क्लीयर करके डिफ़ॉल्ट के रूप में प्रदर्शित किया गया है
- यदि हमने इसे दूर किया है, तो हमारे पास एक त्रुटि है
  - `className` पर  `typedValueElement` से `error` सेट करे

## अपने ऐप्लकैशनको टेसेट करे

आपने इसे अंत तक बना दिया है! अंतिम चरण हमारे आवेदन कार्यों को सुनिश्चित करना है। इसे आजमा कर देखें! अगर वहाँ त्रुटियां हैं तो चिंता न करें; **सभी डेवलपर्स** में त्रुटियां हैं। संदेशों की जांच करें और आवश्यकतानुसार डिबग करें।

**start** पर क्लिक करें, और दूर टाइप करना शुरू करें! यह हमें पहले देखे गए एनीमेशन की तरह दिखना चाहिए।

![खेल का एनीमेशन](/4-typing-game/images/demo.gif)

---

## 🚀 चुनौती

अधिक कार्यक्षमता जोड़ें

- पूर्ण होने पर `input` ईवेंट श्रोता को अक्षम करें, और बटन पर क्लिक करने पर इसे फिर से सक्षम करें
- खिलाड़ी द्वारा बोली पूरा करने पर टेक्स्टबॉक्स को अक्षम करें
- सफलता संदेश के साथ एक मॉडल संवाद बॉक्स प्रदर्शित करें
- [localStorage](https://developer.mozilla.org/docs/Web/API/Window/localStorage) का उपयोग करके उच्च स्कोर स्टोर करें

## व्याख्यान उपरांत प्रश्नोत्तरी

[व्याख्यान उपरांत प्रश्नोत्तरी](https://happy-mud-02d95f10f.azurestaticapps.net/quiz/22?loc=hi)

## समीक्षा और स्व अध्ययन

वेब ब्राउज़र के माध्यम से डेवलपर के लिए [उपलब्ध सभी घटनाओं](https://developer.mozilla.org/docs/Web/Events) को पढ़ें, और उन परिदृश्यों पर विचार करें जिनमें आप प्रत्येक का उपयोग करेंगे।

## असाइनमेंट

[एक नया कीबोर्ड गेम बनाए](assignment.hi.md)
