# ब्राउज़र एक्सटेंशन प्रोजेक्ट पार्ट 2: एक एपीआई को कॉल करें, स्थानीय भंडारण का उपयोग करें

## पूर्व व्याख्यान प्रश्नोत्तरी

[पूर्व व्याख्यान प्रश्नोत्तरी](https://happy-mud-02d95f10f.azurestaticapps.net/quiz/25?loc=hi)

### परिचय

इस पाठ में, आप अपने ब्राउज़र एक्सटेंशन के फ़ॉर्म को सबमिट करके एक एपीआई कॉल करेंगे और अपने ब्राउज़र एक्सटेंशन में परिणाम प्रदर्शित करेंगे। इसके अलावा, आप भविष्य के संदर्भ और उपयोग के लिए अपने ब्राउज़र के स्थानीय भंडारण में डेटा कैसे संग्रहीत कर सकते हैं, इसके बारे में जानेंगे।

✅ अपना कोड कहां रखना है, यह जानने के लिए उपयुक्त फाइलों में क्रमांकित खंडों का पालन करें

### एक्सटेंशन में हेरफेर करने के लिए तत्वों को सेट करें:

इस समय तक आपने अपने ब्राउज़र एक्सटेंशन के लिए फॉर्म और <div> के लिए HTML बना लिया है। अब से, आपको `/src/index.js` फ़ाइल में काम करना होगा और बिट द्वारा अपना एक्सटेंशन बिट बनाना होगा। अपनी परियोजना स्थापित करने और निर्माण की प्रक्रिया पर [पिछले पाठ](../../1-about-browsers/translations/README.hi.md) देखें।

अपने `index.js` फ़ाइल में काम करना, विभिन्न क्षेत्रों से जुड़े मूल्यों को धारण करने के लिए कुछ `const` चर बनाकर शुरू करें:

```JavaScript
// form fields
const form = document.querySelector('.form-data');
const region = document.querySelector('.region-name');
const apiKey = document.querySelector('.api-key');

// results
const errors = document.querySelector('.errors');
const loading = document.querySelector('.loading');
const results = document.querySelector('.result-container');
const usage = document.querySelector('.carbon-usage');
const fossilfuel = document.querySelector('.fossil-fuel');
const myregion = document.querySelector('.my-region');
const clearBtn = document.querySelector('.clear-btn');
```

इन सभी क्षेत्रों को उनके सीएसएस वर्ग द्वारा संदर्भित किया जाता है, जैसा कि आपने इसे पिछले पाठ में HTML में सेट किया था।

### लिस्टेनेर जोड़े

इसके बाद, ईवेंट श्रोताओं को फ़ॉर्म और स्पष्ट बटन में जोड़ें, जो फ़ॉर्म को रीसेट करता है, ताकि यदि कोई उपयोगकर्ता फ़ॉर्म को सबमिट करता है या उस रीसेट बटन को क्लिक करता है, तो कुछ घटित होगा, और फ़ाइल के निचले भाग में ऐप को आरंभीकृत करने के लिए कॉल जोड़ें:

```JavaScript
form.addEventListener('submit', (e) => handleSubmit(e));
clearBtn.addEventListener('click', (e) => reset(e));
init();
```

✅ ध्यान दें कि शॉर्टहैंड एक सबमिट या क्लिक करने के लिए सुनने के लिए प्रयोग किया जाता है, और यह कैसे घटना को हैंडल या रीसेट कार्यों के लिए पारित किया जाता है। क्या आप इस शॉर्टहैंड के बराबर लंबे प्रारूप में लिख सकते हैं? आप क्या पसंद करेंगे?

### Init() फ़ंक्शन और reset() फ़ंक्शन का निर्माण करें:

अब आप उस फ़ंक्शन का निर्माण करने जा रहे हैं जो एक्सटेंशन को इनिशियलाइज़ करता है, जिसे init() कहा जाता है:

```JavaScript
function init() {
	//if anything is in localStorage, pick it up
	const storedApiKey = localStorage.getItem('apiKey');
	const storedRegion = localStorage.getItem('regionName');

	//set icon to be generic green
	//todo

	if (storedApiKey === null || storedRegion === null) {
		//if we don't have the keys, show the form
		form.style.display = 'block';
		results.style.display = 'none';
		loading.style.display = 'none';
		clearBtn.style.display = 'none';
		errors.textContent = '';
	} else {
        //if we have saved keys/regions in localStorage, show results when they load
        displayCarbonUsage(storedApiKey, storedRegion);
		results.style.display = 'none';
		form.style.display = 'none';
		clearBtn.style.display = 'block';
	}
};

function reset(e) {
	e.preventDefault();
	//clear local storage for region only
	localStorage.removeItem('regionName');
	init();
}

```
इस फ़ंक्शन में, कुछ दिलचस्प तर्क है। इसके माध्यम से पढ़ना, क्या आप देख सकते हैं कि क्या होता है?

- यदि उपयोगकर्ता ने APIKey और क्षेत्र कोड को स्थानीय संग्रहण में संग्रहीत किया है, तो यह जांचने के लिए दो `const` लगाए गए हैं।
- यदि उनमें से कोई भी नल है, तो अपनी शैली को 'ब्लॉक' के रूप में प्रदर्शित करने के लिए रूप बदलकर दिखाएं
- रिजल्ट, लोडिंग और clearBtn को छिपाएं और किसी भी एरर टेक्स्ट को खाली स्ट्रिंग पर सेट करें
- यदि कोई की और क्षेत्र मौजूद है, तो एक दिनचर्या शुरू करें:
  - कार्बन उपयोग डेटा प्राप्त करने के लिए एपीआई को कॉल करें
  - परिणाम क्षेत्र छिपाएँ
  - फॉर्म को छिपाएं
  - रीसेट बटन दिखाएं

आगे बढ़ने से पहले, ब्राउज़रों में उपलब्ध एक बहुत ही महत्वपूर्ण अवधारणा के बारे में सीखना उपयोगी है: [लोकलस्टोरेज](https://developer.mozilla.org/docs/Web/API/Window/localStorage).लोकलस्टोरेज, ब्राउज़र में स्ट्रिंगस को 'की-वैल्यू' पेयर के रूप में स्टोर करने का एक उपयोगी तरीका है। इस प्रकार के वेब स्टोरेज को ब्राउजर में डेटा को मैनेज करने के लिए जावास्क्रिप्ट द्वारा मैनिपुलेट किया जा सकता है। लोकलस्टोरीज की समय सीमा समाप्त नहीं होती है, जबकि ब्राउजर के बंद होने पर एक अन्य तरह का वेब स्टोरेज, स्टोरेज साफ हो जाता है। विभिन्न प्रकार के भंडारण में उनके उपयोग के पक्ष और विपक्ष हैं।

> नोट - आपके ब्राउज़र एक्सटेंशन का अपना स्थानीय भंडारण है; मुख्य ब्राउज़र विंडो एक अलग उदाहरण है और अलग-अलग व्यवहार करता है।

उदाहरण के लिए, आपने अपने APIKey को एक स्ट्रिंग मान दिया है, और आप देख सकते हैं कि यह एज पर "एक वेब पेज का निरीक्षण" (आप एक ब्राउज़र का निरीक्षण करने के लिए राइट-क्लिक कर सकते हैं) पर सेट है और एप्लिकेशन टैब पर जाकर भंडारण देख सकते हैं।

![स्थानीय भंडारण फलक](../images/localstorage.png)

✅ उन स्थितियों के बारे में सोचें जहां आप लोकलस्टोरेज में कुछ डेटा स्टोर नहीं करना चाहेंगे। सामान्य तौर पर, लोकलस्टेज में एपीआई कीज रखना एक बुरा विचार है! क्या आप देख सकते हैं क्यों? हमारे मामले में, चूंकि हमारा ऐप विशुद्ध रूप से सीखने के लिए है और ऐप स्टोर में तैनात नहीं किया जाएगा, इसलिए हम इस पद्धति का उपयोग करेंगे।

ध्यान दें कि आप लोकलस्टोरेज में हेरफेर करने के लिए वेब एपीआई का उपयोग करते हैं, या तो `getItem()`, `setItem()` या `removeItem()` का उपयोग करके। यह ब्राउज़रों में व्यापक रूप से समर्थित है।

`DisplayCarbonUsage()` फ़ंक्शन का निर्माण करने से पहले जिसे `init()` कहा जाता है, चलो प्रारंभिक फॉर्म सबमिशन को संभालने के लिए कार्यक्षमता का निर्माण करते हैं।

### फॉर्म सबमिट करें

एक फ़ंक्शन बनाएं जिसे `handleSubmit` कहा जाता है जो एक ईवेंट तर्क `(e)` को स्वीकार करता है । ईवेंट को प्रचार करने से रोकें (इस मामले में, हम ब्राउज़र को रिफ्रेश होने से रोकना चाहते हैं) और एक नया फ़ंक्शन, `setUpUser`, तर्कों में गुजरते हुए `apiKey.value` और `region.value` को कॉल करें। इस प्रकार, आप उन दो मानों का उपयोग करते हैं, जो प्रारंभिक फ़ील्ड के माध्यम से लाए जाते हैं जब उपयुक्त फ़ील्ड पॉपुलेट किए जाते हैं।

```JavaScript
function handleSubmit(e) {
	e.preventDefault();
	setUpUser(apiKey.value, region.value);
}
```
✅ अपनी मेमोरी को रीफ़्रेश करें - आपने अंतिम पाठ में जो HTML सेट किया है, उसमें दो इनपुट फ़ील्ड हैं, जिनके `value` को फ़ाइल के शीर्ष पर स्थापित किए गए `const` के माध्यम से कैप्चर किया गया है, और वे दोनों `required` हैं, इसलिए ब्राउज़र उपयोगकर्ताओं को रोकता है शून्य मानों को इनपुट करने से।

### उपयोगकर्ता सेट करें

`SetUpUser` फ़ंक्शन पर चलते हुए, यहां वह स्थान है जहां आप apiKey और क्षेत्रनाम के लिए स्थानीय संग्रहण मान सेट करते हैं। एक नया फ़ंक्शन जोड़ें:

```JavaScript
function setUpUser(apiKey, regionName) {
	localStorage.setItem('apiKey', apiKey);
	localStorage.setItem('regionName', regionName);
	loading.style.display = 'block';
	errors.textContent = '';
	clearBtn.style.display = 'block';
	//make initial call
	displayCarbonUsage(apiKey, regionName);
}
```
यह फ़ंक्शन API को दिखाने के लिए एक लोडिंग संदेश सेट करता है। इस बिंदु पर, आप इस ब्राउज़र एक्सटेंशन के सबसे महत्वपूर्ण कार्य को बनाने पर पहुंचे हैं!

### कार्बन उपयोग प्रदर्शित करें

अंत में यह एपीआई क्वेरी करने का समय है!

आगे जाने से पहले, हमें एपीआई पर चर्चा करनी चाहिए। एपीआई, या [एप्लीकेशन प्रोग्रामिंग इंटरफेस] (https://www.webopedia.com/TERM/A/API.html), वेब डेवलपर के टूलबॉक्स का एक महत्वपूर्ण तत्व है। वे एक दूसरे के साथ बातचीत और इंटरफ़ेस करने के लिए कार्यक्रमों के लिए मानक तरीके प्रदान करते हैं। उदाहरण के लिए, यदि आप एक वेब साइट का निर्माण कर रहे हैं, जिसे डेटाबेस से क्वेरी करने की आवश्यकता है, तो किसी ने आपके उपयोग के लिए एक एपीआई बनाया होगा। जबकि कई प्रकार के API हैं, जिनमें से एक सबसे लोकप्रिय है [REST API](https://www.smashingmagazine.com/2018/01/understanding-use-rest-api/)

✅ 'REST' शब्द 'Representational State Transfer'  के लिए है और इसमें डेटा लाने के लिए विभिन्न कॉन्फ़िगर किए गए URL का उपयोग करने की सुविधा है। डेवलपर्स के लिए उपलब्ध विभिन्न प्रकार के एपीआई पर थोड़ा शोध करें। क्या प्रारूप आपको अपील करता है?

इस फ़ंक्शन के बारे में ध्यान देने योग्य महत्वपूर्ण बातें हैं। पहले [`async` कीवर्ड](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Statements/async_function) को नोटिस करें। अपने कार्यों को लिखना ताकि वे अतुल्यकालिक रूप से चलाते हैं इसका मतलब है कि वे एक कार्रवाई की प्रतीक्षा करते हैं, जैसे डेटा लौटाए जाने से पहले, पूरा होने से पहले।

यहाँ `async` के बारे में एक त्वरित वीडियो है:

[![promises के प्रबंधन के लिए Async और Await](https://img.youtube.com/vi/YwmlRkrxvkk/0.jpg)](https://youtube.com/watch?v=YwmlRkrxvkk "promises के प्रबंधन के लिए Async और Await")

> Async/wait के वीडियो के लिए ऊपर दी गई छवि पर क्लिक करें।

C02Signal API को क्वेरी करने के लिए एक नया फ़ंक्शन बनाएँ:

```JavaScript
import axios from '../node_modules/axios';

async function displayCarbonUsage(apiKey, region) {
	try {
		await axios
			.get('https://api.co2signal.com/v1/latest', {
				params: {
					countryCode: region,
				},
				headers: {
					'auth-token': apiKey,
				},
			})
			.then((response) => {
				let CO2 = Math.floor(response.data.data.carbonIntensity);

				//calculateColor(CO2);

				loading.style.display = 'none';
				form.style.display = 'none';
				myregion.textContent = region;
				usage.textContent =
					Math.round(response.data.data.carbonIntensity) + ' grams (grams C02 emitted per kilowatt hour)';
				fossilfuel.textContent =
					response.data.data.fossilFuelPercentage.toFixed(2) +
					'% (percentage of fossil fuels used to generate electricity)';
				results.style.display = 'block';
			});
	} catch (error) {
		console.log(error);
		loading.style.display = 'none';
		results.style.display = 'none';
		errors.textContent = 'Sorry, we have no data for the region you have requested.';
	}
}
```

यह एक बड़ा फंगक्शन है। यहाँ क्या चल रहा है?

- सर्वोत्तम कार्यप्रणालियों का पालन करते हुए, आप इस फ़ंक्शन का उपयोग करने के लिए एक `async` कीवर्ड का उपयोग करते हैं। फ़ंक्शन में एक `try/catch` ब्लॉक होता है क्योंकि यह एक वादा लौटाएगा जब एपीआई डेटा लौटाएगा। चूँकि आपके पास उस गति पर नियंत्रण नहीं है जो एपीआई प्रतिक्रिया देगा (यह प्रतिक्रिया नहीं दे सकता है!), आपको इस अनिश्चितता को असंगत रूप से कॉल करने की आवश्यकता है।
- आप अपने API की का उपयोग करके अपने क्षेत्र का डेटा प्राप्त करने के लिए co2signal API की क्वेरी कर रहे हैं। उस की का उपयोग करने के लिए, आपको अपने हेडर मापदंडों में एक प्रकार के प्रमाणीकरण का उपयोग करना होगा।
- एपीआई के जवाब देने के बाद, आप इस डेटा को दिखाने के लिए अपने स्क्रीन के कुछ हिस्सों में इसके रिस्पॉन्स डेटा के विभिन्न तत्वों को असाइन करते हैं।
- यदि कोई त्रुटि है, या कोई परिणाम नहीं है, तो आप एक त्रुटि संदेश दिखाते हैं।

✅ Asyncronous प्रोग्रामिंग पैटर्न का उपयोग करना आपके टूलबॉक्स में एक और बहुत उपयोगी उपकरण है। [विभिन्न तरीकों के बारे में](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Statements/async_function) पढ़ें आप इस प्रकार के कोड को कॉन्फ़िगर कर सकते हैं।

बधाई हो! यदि आप अपना एक्सटेंशन बनाते हैं (`npm run build`) और इसे अपने एक्सटेंशन पेन में रिफ्रेश करें, तो आपके पास काम करने का एक्सटेंशन है! केवल एक चीज जो काम नहीं कर रही है वह आइकन है, और आप इसे अगले पाठ में ठीक कर देंगे।

---

## 🚀 चुनौती

हमने इन पाठों में अब तक कई प्रकार के एपीआई पर चर्चा की है। एक वेब एपीआई चुनें और गहराई से शोध करें कि यह क्या प्रदान करता है। उदाहरण के लिए, [HTML ड्रैग एंड ड्रॉप एपीआई](https://developer.mozilla.org/docs/Web/API/HTML_Drag_and_Drop_API) जैसे ब्राउज़रों के भीतर उपलब्ध एपीआई पर एक नज़र डालें। आपकी राय में एक महान एपीआई क्या है?

## व्याख्यान उपरांत प्रश्नोत्तरी

[व्याख्यान उपरांत प्रश्नोत्तरी](https://happy-mud-02d95f10f.azurestaticapps.net/quiz/26?loc=hi)

## समीक्षा और स्व अध्ययन

आपने इस पाठ में लोकलस्टोरेज और एपीआई के बारे में सीखा, दोनों पेशेवर वेब डेवलपर के लिए बहुत उपयोगी हैं। क्या आप सोच सकते हैं कि ये दोनों चीजें एक साथ कैसे काम करती हैं? इस बारे में सोचें कि आप एक वेब साइट को कैसे आर्किटेक्ट करेंगे जो एक एपीआई द्वारा उपयोग की जाने वाली वस्तुओं को संग्रहीत करेगी।

## असाइनमेंट

[एक एपीआई अपनाएं](assignment.hi.md)

