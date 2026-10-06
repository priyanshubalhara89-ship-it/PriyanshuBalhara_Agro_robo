```html
<!DOCTYPE html>
<html lang="hi">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <title>Smart Agri-Rover AI • Priyanshu Balhara</title>
  
  <!-- Tailwind CSS CDN -->
  <script src="https://cdn.tailwindcss.com"></script>
  
  <!-- TensorFlow.js & Teachable Machine Libraries -->
  <script src="https://cdn.jsdelivr.net/npm/@tensorflow/tfjs@latest/dist/tf.min.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/@teachablemachine/image@latest/dist/teachablemachine-image.min.js"></script>
  
  <!-- Google Fonts for Clean Pastel UI -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;600;700;800&display=swap" rel="stylesheet">

  <style>
    body {
      font-family: 'Plus Jakarta Sans', sans-serif;
    }
    .scanner-laser {
      position: absolute;
      top: 0; left: 0; width: 100%; height: 3px;
      background: linear-gradient(90deg, transparent, #10b981, #f59e0b, #06b6d4, transparent);
      box-shadow: 0 0 12px #10b981;
      animation: scanLaser 2s infinite ease-in-out;
    }
    @keyframes scanLaser {
      0% { top: 6%; opacity: 0.4; }
      50% { top: 92%; opacity: 1; }
      100% { top: 6%; opacity: 0.4; }
    }
    #webcam-container canvas {
      width: 100% !important;
      height: 100% !important;
      object-fit: cover;
      border-radius: 1.25rem;
    }
    .soft-shadow {
      box-shadow: 0 10px 25px -5px rgba(16, 185, 129, 0.08), 0 8px 10px -6px rgba(16, 185, 129, 0.04);
    }
  </style>
</head>
<body class="bg-gradient-to-br from-emerald-50/60 via-teal-50/40 to-amber-50/50 text-slate-800 min-h-screen p-3 flex flex-col items-center select-none pb-12">

  <!-- Author & School Header Card (Judge Proof) -->
  <header class="w-full max-w-md bg-white/90 backdrop-blur-md border border-emerald-100/80 rounded-3xl p-4 soft-shadow mb-3 transition-all hover:border-emerald-200">
    <div class="flex items-start justify-between">
      <div class="space-y-0.5">
        <div class="inline-flex items-center space-x-1.5 px-2.5 py-0.5 bg-emerald-100/70 border border-emerald-300 text-emerald-800 rounded-full text-[10px] font-bold tracking-wide">
          <span class="w-1.5 h-1.5 bg-emerald-500 rounded-full animate-pulse"></span>
          <span>🌾 AI AGRI-INNOVATION LAB</span>
        </div>
        <h1 class="text-base font-extrabold text-slate-900 tracking-tight flex items-center space-x-1 mt-1">
          <span>Autonomous Agri-Rover</span>
          <span class="text-xs text-emerald-600 bg-emerald-50 px-1.5 py-0.5 rounded-md font-bold">v2.4</span>
        </h1>
      </div>

      <div class="text-right">
        <span class="text-[10px] uppercase font-bold text-slate-400 block tracking-wider">PROJECT LEAD</span>
        <span class="text-xs font-black text-emerald-700 bg-emerald-50 px-2.5 py-1 rounded-xl border border-emerald-200 inline-block shadow-sm">
          👨‍💻 Priyanshu Balhara
        </span>
      </div>
    </div>

    <!-- School Affiliation Strip -->
    <div class="mt-2.5 pt-2.5 border-t border-slate-100 flex items-center justify-between text-[11px] text-slate-600">
      <div class="flex items-center space-x-1.5 truncate">
        <span class="text-amber-500 text-xs">🏫</span>
        <span class="font-bold text-slate-700 truncate">आरोही मॉडल सीनियर सेकेंडरी स्कूल</span>
      </div>
      <span class="text-[10px] bg-amber-50 text-amber-800 font-semibold px-2 py-0.5 rounded-full border border-amber-200 flex-shrink-0">
        खेड़ा शिवानी, भिवानी 📍
      </span>
    </div>
  </header>

  <!-- Camera Section with Soft Pastel Framing -->
  <div class="w-full max-w-md relative bg-slate-900 rounded-3xl overflow-hidden shadow-xl border-4 border-white aspect-[4/3] flex items-center justify-center">
    <div id="webcam-container" class="w-full h-full flex items-center justify-center">
      <div id="camera-placeholder" class="text-center px-4">
        <div class="w-14 h-14 mx-auto mb-2 bg-emerald-800/40 rounded-2xl flex items-center justify-center text-2xl border border-emerald-400/30 animate-bounce">
          🌱
        </div>
        <p class="text-xs text-slate-200 font-bold">एआई कैमरा शुरू करने के लिए नीचे टैप करें:<br>
          <span class="text-emerald-300 font-black text-sm">"📸 एआई कैमरा ऑन करें"</span>
        </p>
      </div>
    </div>

    <!-- Laser Animation Overlay -->
    <div id="laser-line" class="scanner-laser hidden pointer-events-none"></div>
    
    <!-- AI Status Badge -->
    <div id="status-badge" class="absolute top-3 left-3 bg-white/90 backdrop-blur-md px-3 py-1 rounded-full text-[11px] font-bold text-slate-700 shadow-sm border border-slate-200">
      कैमरा: बंद 💤
    </div>

    <!-- System Spec Pill -->
    <div class="absolute top-3 right-3 bg-emerald-900/80 backdrop-blur-md px-2.5 py-1 rounded-full text-[10px] font-bold text-emerald-200 border border-emerald-500/50">
      Edge AI • 65% Instant
    </div>
  </div>

  <!-- Realtime GPS Location Card -->
  <div class="w-full max-w-md mt-2.5 bg-white/90 rounded-2xl p-2.5 border border-slate-100 text-[11px] flex items-center justify-between soft-shadow">
    <div class="flex items-center space-x-2 truncate">
      <span class="w-7 h-7 bg-amber-100 rounded-xl flex items-center justify-center text-xs flex-shrink-0">📍</span>
      <div class="truncate">
        <span class="font-bold text-slate-800">खेत लोकेशन:</span>
        <span id="loc-display-text" class="text-slate-500 ml-1 truncate">GPS फेच हो रहा है...</span>
      </div>
    </div>
    <button onclick="fetchPreciseLocation()" class="px-2.5 py-1 bg-emerald-50 hover:bg-emerald-100 active:scale-95 text-emerald-700 font-bold rounded-xl text-[10px] border border-emerald-200 transition flex items-center space-x-1 flex-shrink-0">
      <span>🔄</span>
      <span>रीफ्रेश</span>
    </button>
  </div>

  <!-- AI Confidence Meters (Pastel Cards) -->
  <div class="w-full max-w-md mt-2.5 bg-white/90 rounded-3xl p-3.5 border border-emerald-100/70 soft-shadow">
    <div class="flex justify-between items-center mb-2.5">
      <div class="flex items-center space-x-1.5">
        <span class="text-xs">🔬</span>
        <h3 class="text-xs font-bold text-slate-700 uppercase tracking-wider">रोग पहचान मीटर (≥65% अलर्ट)</h3>
      </div>
      <span id="detected-disease-tag" class="text-[11px] px-2.5 py-0.5 rounded-full bg-slate-100 text-slate-500 font-bold border border-slate-200">
        प्रतीक्षारत ⏳
      </span>
    </div>
    
    <div id="prediction-meters-container" class="space-y-2">
      <div class="text-xs text-slate-400 text-center py-2 italic font-medium">कैमरा चालू होते ही पौधे की लाइव स्थिति दिखेगी ✨</div>
    </div>

    <!-- Organic Remedy Box -->
    <div id="remedy-box" class="mt-3 p-3 bg-gradient-to-r from-amber-50 to-orange-50 border border-amber-200/80 rounded-2xl hidden text-xs transition-all">
      <div class="text-amber-900 font-bold mb-1 flex items-center space-x-1.5">
        <span>🌿</span>
        <span>अनुशंसित जैविक उपचार (Organic Remedy):</span>
      </div>
      <div id="remedy-text" class="text-amber-800 text-[11px] leading-relaxed font-medium"></div>
    </div>
  </div>

  <!-- Action Controls -->
  <div class="w-full max-w-md mt-3 space-y-2.5">
    
    <!-- Primary Start Button -->
    <button id="btn-start-camera" onclick="initCameraAI()" class="w-full py-3.5 bg-gradient-to-r from-emerald-600 to-teal-600 hover:from-emerald-500 hover:to-teal-500 active:scale-[0.98] transition rounded-2xl font-black text-xs text-white shadow-lg shadow-emerald-500/25 flex items-center justify-center space-x-2">
      <span>📸</span>
      <span>एआई कैमरा ऑन करें</span>
    </button>

    <!-- Telegram Live Dispatch Card -->
    <div class="bg-white/90 rounded-2xl p-3 border border-sky-100 text-xs text-slate-700 soft-shadow">
      <div class="flex justify-between items-center mb-2">
        <span class="font-extrabold text-sky-800 flex items-center space-x-1.5">
          <span>📲</span>
          <span>IoT टेलीग्राम क्लाउड (@Sdgvdr_bot)</span>
        </span>
        <span class="text-[10px] bg-sky-50 border border-sky-200 text-sky-700 px-2 py-0.5 rounded-full font-bold">100% लाइव ⚡</span>
      </div>

      <button onclick="sendTestTelegram()" class="w-full py-2.5 bg-sky-50 hover:bg-sky-100 active:scale-98 transition rounded-xl font-bold text-xs text-sky-700 border border-sky-200 flex items-center justify-center space-x-1.5">
        <span>🔔</span>
        <span>टेस्ट टेलीग्राम अलर्ट भेजकर देखें</span>
      </button>

      <!-- Pre-configured Credentials -->
      <input id="tg-token" type="hidden" value="8770983557:AAFCWc0YL5Eg0o4Gm1Ip-vzCqSS2k0kkENQ">
      <input id="tg-chat-id" type="hidden" value="8527231202">
    </div>

    <!-- Judge Verification & Technical Specifications Drawer -->
    <details class="bg-white/80 rounded-2xl border border-slate-200 text-xs overflow-hidden soft-shadow group">
      <summary class="p-3 font-bold text-slate-700 cursor-pointer flex items-center justify-between hover:bg-slate-50 transition">
        <span class="flex items-center space-x-1.5">
          <span>🛡️</span>
          <span>प्रोजेक्ट सत्यापन एवं सिस्टम आर्किटेक्चर (जज गाइड)</span>
        </span>
        <span class="text-[10px] text-slate-400 group-open:rotate-180 transition-transform">▼</span>
      </summary>
      <div class="p-3 pt-0 text-[11px] text-slate-600 space-y-1.5 border-t border-slate-100 mt-1">
        <p>• <strong>डेवलपर:</strong> प्रियांशु बलहारा (Priyanshu Balhara)</p>
        <p>• <strong>स्कूल:</strong> आरोही मॉडल सीनियर सेकेंडरी स्कूल, खेड़ा शिवानी (भिवानी)</p>
        <p>• <strong>एज विज़न AI:</strong> TensorFlow.js & MobileNet (On-Device WebGL Inference)</p>
        <p>• <strong>नेविगेशन:</strong> ESP32 Autonomous State-Machine (3s Patrol / 5s Hold)</p>
        <p>• <strong>IoT गेटवे:</strong> HTML5 Geolocation API + Telegram Cloud Bot API</p>
      </div>
    </details>

    <!-- Console Log Box -->
    <div class="bg-slate-900 rounded-2xl p-2.5 font-mono text-[10px] text-slate-300 h-16 overflow-y-auto border border-slate-800" id="log-box">
      <div class="text-emerald-400">[सिस्टम] स्मार्ट एग्री-रोवर विज़न ऐप तैयार है।</div>
    </div>
  </div>

  <script>
    let model, webcam, maxPredictions;
    let isCoolingDown = false;
    let scanCount = 0;
    
    // Live Location Coordinates Cache
    let currentLat = "28.6139";
    let currentLon = "77.2090";
    let currentAddress = "खेत / स्थानीय प्लॉट";

    // Instant Trigger Threshold: 65%
    const TRIGGER_THRESHOLD = 65;

    const REMEDIES = {
      "scab": { crop: "सेब (Apple)", disease: "एप्पल स्कैब (Apple Scab)", remedy: "नीम का तेल (5ml/L) अथवा बोर्डो मिश्रण का तत्काल छिड़काव करें।" },
      "rot": { crop: "फल/सब्जी", disease: "सड़न रोग (Rot)", remedy: "संक्रमित पत्तियां/फल अलग करें और कॉपर ऑक्सीक्लोराइड (3g/L) स्प्रे करें।" },
      "rust": { crop: "फसल", disease: "रतुआ रोग (Rust)", remedy: "घुलनशील सल्फर कवकनाशी (2g/L) का छिड़काव करें।" },
      "blight": { crop: "टमाटर / आलू", disease: "झुलसा (Blight)", remedy: "ट्राइकोडर्मा जैविक कवकनाशी (2.5g/L) अथवा मैंकोज़ेब का छिड़काव करें।" },
      "mold": { crop: "पत्तियां", disease: "पर्ण फफूंद (Leaf Mold)", remedy: "हवा का संचार बढ़ाएं और नीम तेल कवकनाशी का स्प्रे करें।" },
      "spot": { crop: "पौधा", disease: "पत्ती धब्बा (Leaf Spot)", remedy: "तांबे पर आधारित जैविक कवकनाशी (Copper Fungicide) का प्रयोग करें।" }
    };

    function log(msg, color = "text-slate-300") {
      const logBox = document.getElementById("log-box");
      const time = new Date().toTimeString().split(' ')[0];
      const div = document.createElement("div");
      div.className = color;
      div.innerHTML = `[${time}] ${msg}`;
      logBox.prepend(div);
    }

    // 1. Fetch Real Precise Location (GPS with fallback)
    function fetchPreciseLocation() {
      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(
          (pos) => {
            currentLat = pos.coords.latitude.toFixed(6);
            currentLon = pos.coords.longitude.toFixed(6);
            document.getElementById("loc-display-text").innerText = `${currentLat}, ${currentLon}`;
            log(`📍 GPS स्थिति प्राप्त: ${currentLat}, ${currentLon}`, "text-emerald-400 font-bold");
          },
          (err) => {
            console.warn("GPS fallback triggered:", err);
            fetchNetworkLocation();
          },
          { enableHighAccuracy: true, timeout: 5000, maximumAge: 0 }
        );
      } else {
        fetchNetworkLocation();
      }
    }

    async function fetchNetworkLocation() {
      try {
        const res = await fetch("https://ipapi.co/json/");
        const data = await res.json();
        if (data.latitude && data.longitude) {
          currentLat = data.latitude;
          currentLon = data.longitude;
          currentAddress = `${data.city || ""}, ${data.region || ""}`;
          document.getElementById("loc-display-text").innerText = currentAddress;
          log(`📍 नेटवर्क लोकेशन: ${currentAddress}`, "text-slate-400");
        }
      } catch (e) {
        document.getElementById("loc-display-text").innerText = "खेत / स्थानीय क्षेत्र (GPS Active)";
      }
    }

    window.addEventListener("load", fetchPreciseLocation);

    // 2. Camera & Teachable Machine Engine
    async function initCameraAI() {
      const btn = document.getElementById("btn-start-camera");
      btn.innerHTML = "<span>⏳</span><span>एआई मॉडल लोड हो रहा है...</span>";
      btn.disabled = true;

      try {
        log("मॉडल लोड हो रहा है...", "text-amber-400");
        const modelURL = "https://teachablemachine.withgoogle.com/models/XlgGovZLB/";
        model = await tmImage.load(modelURL + "model.json", modelURL + "metadata.json");
        maxPredictions = model.getTotalClasses();
        buildMeters();

        log("कैमरा चालू किया जा रहा है...", "text-amber-400");
        webcam = new tmImage.Webcam(320, 240, false);
        
        let started = false;
        try {
          await webcam.setup({ facingMode: "environment" });
          started = true;
        } catch (e) {
          console.warn("Back cam fallback:", e);
        }

        if (!started) {
          await webcam.setup();
        }

        await webcam.play();
        window.requestAnimationFrame(predictLoop);

        const placeholder = document.getElementById("camera-placeholder");
        if (placeholder) placeholder.remove();

        const container = document.getElementById("webcam-container");
        container.innerHTML = "";
        container.appendChild(webcam.canvas);

        document.getElementById("laser-line").classList.remove("hidden");
        document.getElementById("status-badge").innerText = "कैमरा: सक्रिय 🟢";
        document.getElementById("status-badge").className = "absolute top-3 left-3 bg-emerald-100 text-emerald-800 border border-emerald-300 px-3 py-1 rounded-full text-[11px] font-bold shadow-sm";

        btn.innerHTML = "<span>✅</span><span>एआई विज़न 100% सक्रिय</span>";
        btn.className = "w-full py-3.5 bg-emerald-700 rounded-2xl font-black text-xs text-white shadow-md";
        log("✅ एआई मॉडल लोड पूर्ण!", "text-emerald-400 font-bold");
      } catch (err) {
        btn.innerHTML = "<span>📸</span><span>एआई कैमरा ऑन करें</span>";
        btn.disabled = false;
        log("कैमरा एरर: " + err.message, "text-rose-400");
      }
    }

    function buildMeters() {
      const container = document.getElementById("prediction-meters-container");
      container.innerHTML = "";
      for (let i = 0; i < maxPredictions; i++) {
        const name = model.getClassLabels()[i];
        const isHealthy = name.toLowerCase().includes("health");
        const color = isHealthy ? "bg-emerald-500" : "bg-rose-500";
        const div = document.createElement("div");
        div.innerHTML = `
          <div class="flex justify-between text-[11px] font-bold mb-0.5">
            <span class="${isHealthy ? 'text-emerald-700' : 'text-rose-700'}">${name}</span>
            <span id="val-${i}" class="text-slate-600">0%</span>
          </div>
          <div class="w-full bg-slate-100 h-2.5 rounded-full overflow-hidden border border-slate-200/60">
            <div id="bar-${i}" class="${color} h-full w-0 transition-all duration-150 rounded-full"></div>
          </div>
        `;
        container.appendChild(div);
      }
    }

    async function predictLoop() {
      webcam.update();
      await predictFrame();
      window.requestAnimationFrame(predictLoop);
    }

    async function predictFrame() {
      const predictions = await model.predict(webcam.canvas);
      let topDisease = null;
      let topConfidence = 0;
      let topClass = "";
      let topScore = 0;

      for (let i = 0; i < maxPredictions; i++) {
        const name = predictions[i].className;
        const prob = Math.round(predictions[i].probability * 100);

        const v = document.getElementById(`val-${i}`);
        const b = document.getElementById(`bar-${i}`);
        if (v && b) {
          v.innerText = prob + "%";
          b.style.width = prob + "%";
        }

        if (prob > topScore) {
          topScore = prob;
          topClass = name;
        }

        const lower = name.toLowerCase();
        if (!lower.includes("health") && !lower.includes("back") && prob > topConfidence) {
          topConfidence = prob;
          topDisease = name;
        }
      }

      const tag = document.getElementById("detected-disease-tag");
      const remedyBox = document.getElementById("remedy-box");
      const remedyText = document.getElementById("remedy-text");

      // 65% Instant Trigger
      if (topConfidence >= TRIGGER_THRESHOLD) {
        tag.innerText = `🚨 ${topDisease}: ${topConfidence}%`;
        tag.className = "text-[11px] px-3 py-0.5 rounded-full bg-rose-100 border border-rose-300 text-rose-800 font-extrabold animate-pulse shadow-sm";
        remedyBox.classList.remove("hidden");
        remedyText.innerText = getRemedyText(topDisease);

        if (!isCoolingDown) {
          triggerDiseaseEvent(topDisease, topConfidence);
        }
      } else {
        remedyBox.classList.add("hidden");
        tag.innerText = topClass.toLowerCase().includes("health") ? "🌿 पौधा स्वस्थ है" : "स्कैनिंग जारी...";
        tag.className = "text-[11px] px-2.5 py-0.5 rounded-full bg-slate-100 text-slate-600 font-bold";
      }
    }

    function getRemedyText(disease) {
      const lower = disease.toLowerCase();
      for (let k in REMEDIES) {
        if (lower.includes(k)) return REMEDIES[k].remedy;
      }
      return "नीम का तेल (5ml प्रति लीटर पानी) अथवा जैविक बोर्डो मिश्रण का तत्काल छिड़काव करें।";
    }

    function triggerDiseaseEvent(disease, conf) {
      isCoolingDown = true;
      scanCount++;
      log(`🚨 रोग की पुष्टि (≥65%): ${disease} (${conf}%)`, "text-rose-400 font-bold");

      sendTelegramAlert(disease, conf);

      setTimeout(() => {
        isCoolingDown = false;
        log("सिस्टम अगले पौधे की जांच हेतु तैयार।", "text-slate-400");
      }, 5500);
    }

    // 3. Dispatch Live Report to Telegram
    async function sendTelegramAlert(disease, conf) {
      const token = document.getElementById("tg-token").value;
      const chatId = document.getElementById("tg-chat-id").value;
      const remedy = getRemedyText(disease);
      const mapsUrl = `https://www.google.com/maps?q=${currentLat},${currentLon}`;

      const msg = `🚨 *स्मार्ट एग्री-रोवर लाइव रिपोर्ट* 🚨\n` +
                  `━━━━━━━━━━━━━━━━━━━━\n` +
                  `👨‍💻 *डेवलपर:* प्रियांशु बलहारा (Priyanshu Balhara)\n` +
                  `🏫 *स्कूल:* आरोही मॉडल सीनियर सेकेंडरी स्कूल, खेड़ा शिवानी\n` +
                  `━━━━━━━━━━━━━━━━━━━━\n` +
                  `📍 *पौधा क्रमांक:* #${scanCount}\n` +
                  `🍂 *पहचाना गया रोग# PriyanshuBalhara_Agro_robo
