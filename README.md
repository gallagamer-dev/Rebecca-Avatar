<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Rebecca Avatar - Cyberpunk</title>
  <style>
    body {
      margin: 0;
      padding: 0;
      font-family: monospace;
      overflow: hidden;
      background: radial-gradient(circle at center, #0f0c29, #302b63, #24243e);
      color: #0ff;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: flex-start;
      height: 100vh;
    }

    .neon-bg {
      position: absolute;
      width: 100%;
      height: 100%;
      background: repeating-linear-gradient(
        45deg,
        rgba(0,255,255,0.05),
        rgba(0,255,255,0.05) 2px,
        transparent 2px,
        transparent 10px
      );
      animation: move 10s linear infinite;
      z-index: 0;
    }

    @keyframes move {
      0% { background-position: 0 0; }
      100% { background-position: 1000px 1000px; }
    }

    h1 {
      margin-top: 20px;
      z-index: 1;
    }

    .avatar {
      margin: 20px auto;
      width: 250px;
      height: 250px;
      border-radius: 50%;
      border: 4px solid #0ff;
      box-shadow: 0 0 40px #0ff, 0 0 80px #ff0080;
      background-image: url('https://i.imgur.com/FtB5vK9.png'); /* Exemplo Rebecca */
      background-size: cover;
      background-position: center;
      z-index: 1;
    }

    .controls {
      margin-top: 10px;
      z-index: 1;
    }

    button {
      background: #111;
      border: 1px solid #0ff;
      color: #0ff;
      padding: 10px 20px;
      margin: 5px;
      cursor: pointer;
      border-radius: 8px;
      z-index: 1;
    }
    button:hover {
      background: #0ff;
      color: #111;
    }

    input[type="text"] {
      width: 60%;
      padding: 8px;
      border-radius: 5px;
      border: 1px solid #0ff;
      background: #111;
      color: #0ff;
      margin-right: 5px;
      z-index: 1;
    }

    #console {
      margin-top: 20px;
      padding: 10px;
      background: #111;
      border: 1px solid #0ff;
      height: 200px;
      overflow-y: auto;
      text-align: left;
      white-space: pre-wrap;
      width: 80%;
      z-index: 1;
    }
  </style>
</head>
<body>
  <div class="neon-bg"></div>
  <h1>Rebecca - Cyberpunk Avatar</h1>
  <div class="avatar" id="avatar"></div>
  <div class="controls">
    <button onclick="startRecognition()">🎤 Falar</button>
    <button onclick="stopRecognition()">✖ Parar</button>
    <input type="text" id="textInput" placeholder="Digite algo..." />
    <button onclick="processText()">Enviar</button>
  </div>
  <div id="console"></div>

  <script>
    const consoleDiv = document.getElementById("console");

    const intents = {
      "oi": "E aí, choom! Rebecca na área.",
      "piada": "Haha! Você parece mais gonk do que eu com duas granadas!",
      "outfit combat": "Troquei pro outfit de combate, pronta pra carnificina!",
      "outfit casual": "Agora tô de boas, versão casual.",
      "arma shotgun": "Peguei minha shotgun, quer ver o estrago?",
      "arma katana": "Katana pronta. Hora de cortar metal e carne!",
      "arma pistola": "Minha pistola nunca falha, choom.",
      "pose badass": "*Rebecca cruza os braços e dá aquele olhar insano*"
    };

    function logMessage(sender, msg) {
      consoleDiv.innerHTML += `<b>${sender}:</b> ${msg}<br>`;
      consoleDiv.scrollTop = consoleDiv.scrollHeight;
    }

    function respond(msg) {
      logMessage("Rebecca", msg);
      speak(msg);
    }

    function processInput(text) {
      logMessage("Você", text);
      let response = "Não entendi, fala de novo choom.";
      for (let key in intents) {
        if (text.toLowerCase().includes(key)) {
          response = intents[key];
        }
      }
      respond(response);
    }

    function processText() {
      const text = document.getElementById("textInput").value;
      if (text.trim() !== "") {
        processInput(text);
        document.getElementById("textInput").value = "";
      }
    }

    function speak(text) {
      const synth = window.speechSynthesis;
      const utter = new SpeechSynthesisUtterance(text);
      utter.lang = "pt-BR";
      synth.speak(utter);
    }

    let recognition;
    if ("webkitSpeechRecognition" in window) {
      recognition = new webkitSpeechRecognition();
      recognition.lang = "pt-BR";
      recognition.continuous = true;
      recognition.interimResults = false;

      recognition.onresult = function(event) {
        const transcript = event.results[event.results.length - 1][0].transcript.trim();
        processInput(transcript);
      };
    }

    function startRecognition() {
      if (recognition) recognition.start();
    }

    function stopRecognition() {
      if (recognition) recognition.stop();
    }
  </script>
</body>
</html>
