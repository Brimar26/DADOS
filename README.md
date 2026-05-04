<div id="matevida-game">
    <div class="header-game">
        <span id="score">Puntos: 0</span>
        <span id="level">Nivel: Suma</span>
    </div>

    <div class="dice-container">
        <div id="dado1" class="dice">?</div>
        <div id="dado2" class="dice">?</div>
    </div>

    <div id="challenge-area">
        <p id="instruction">¡Haz clic en Lanzar para empezar!</p>
        <div class="input-group">
            <input type="number" id="answer" placeholder="?">
            <button onclick="checkResult()" id="btn-check" disabled>Validar</button>
        </div>
    </div>

    <button onclick="startTurn()" id="btn-roll">Lanzar Dados 🎲</button>
    <button onclick="resetGame()" id="btn-reset" class="hidden">Jugar de Nuevo 🔄</button>
    
    <p id="msg"></p>
</div>

<style>
    #matevida-game {
        background: #ffffff;
        border: 4px solid #ffd700;
        border-radius: 20px;
        padding: 25px;
        max-width: 450px;
        margin: 20px auto;
        text-align: center;
        font-family: 'Arial', sans-serif;
        box-shadow: 0 10px 20px rgba(0,0,0,0.1);
    }
    .header-game { display: flex; justify-content: space-between; font-weight: bold; color: #2c3e50; margin-bottom: 15px; }
    .dice-container { display: flex; justify-content: center; gap: 15px; margin: 20px 0; }
    .dice {
        width: 70px; height: 70px; background: #2196F3; color: white;
        font-size: 35px; line-height: 70px; border-radius: 12px;
        box-shadow: 0 5px #1565C0; transition: transform 0.2s;
    }
    .input-group { margin: 15px 0; }
    input { padding: 10px; width: 80px; font-size: 20px; border-radius: 8px; border: 2px solid #ccc; text-align: center; }
    button {
        padding: 12px 25px; font-size: 16px; border: none; border-radius: 8px;
        cursor: pointer; font-weight: bold; transition: 0.2s; margin: 5px;
    }
    #btn-roll { background: #4CAF50; color: white; }
    #btn-check { background: #ffd700; color: #333; }
    #btn-reset { background: #e74c3c; color: white; }
    .hidden { display: none; }
    #msg { height: 25px; font-weight: bold; margin-top: 15px; }
</style>

<script>
    let score = 0;
    let currentOp = 0; // 0: Suma, 1: Resta, 2: Multi, 3: Div
    let ops = ['+', '-', 'x', '÷'];
    let names = ['Suma', 'Resta', 'Multiplicación', 'División'];
    let n1, n2, result;

    function speak(text) {
        const speech = new SpeechSynthesisUtterance(text);
        speech.lang = 'es-ES';
        window.speechSynthesis.speak(speech);
    }

    function startTurn() {
        document.getElementById('msg').innerText = "";
        n1 = Math.floor(Math.random() * 10) + 1;
        n2 = Math.floor(Math.random() * 10) + 1;

        // Ajuste para resta (no negativos) y división (exacta)
        if(currentOp === 1 && n1 < n2) [n1, n2] = [n2, n1];
        if(currentOp === 3) { n1 = n1 * n2; } // Asegura división exacta

        document.getElementById('dado1').innerText = n1;
        document.getElementById('dado2').innerText = n2;
        
        document.getElementById('instruction').innerText = `¿Cuánto es ${n1} ${ops[currentOp]} ${n2}?`;
        document.getElementById('btn-check').disabled = false;
        document.getElementById('answer').focus();
        speak(`¿Cuánto es ${n1} ${names[currentOp]} ${n2}?`);
    }

    function checkResult() {
        let userAns = parseInt(document.getElementById('answer').value);
        
        switch(currentOp) {
            case 0: result = n1 + n2; break;
            case 1: result = n1 - n2; break;
            case 2: result = n1 * n2; break;
            case 3: result = n1 / n2; break;
        }

        if(userAns === result) {
            score += 100;
            document.getElementById('msg').style.color = "green";
            document.getElementById('msg').innerText = "¡Excelente! +100 puntos";
            speak("¡Correcto! Muy bien hecho.");
            nextLevel();
        } else {
            document.getElementById('msg').style.color = "red";
            document.getElementById('msg').innerText = "Inténtalo de nuevo";
            speak("Oh no, intenta otra vez.");
        }
        document.getElementById('score').innerText = `Puntos: ${score}`;
        document.getElementById('answer').value = "";
    }

    function nextLevel() {
        currentOp++;
        if(currentOp > 3) {
            endGame();
        } else {
            document.getElementById('level').innerText = `Nivel: ${names[currentOp]}`;
        }
    }

    function endGame() {
        document.getElementById('instruction').innerHTML = "<h3>¡FELICIDADES MAESTRO!</h3>";
        document.getElementById('btn-roll').classList.add('hidden');
        document.getElementById('btn-check').classList.add('hidden');
        document.getElementById('btn-reset').classList.remove('hidden');
        speak(`¡Felicidades! Terminaste el juego con ${score} puntos.`);
    }

    function resetGame() {
        score = 0;
        currentOp = 0;
        document.getElementById('score').innerText = "Puntos: 0";
        document.getElementById('level').innerText = "Nivel: Suma";
        document.getElementById('btn-roll').classList.remove('hidden');
        document.getElementById('btn-check').classList.remove('hidden');
        document.getElementById('btn-reset').classList.add('hidden');
        document.getElementById('instruction').innerText = "¡Haz clic en Lanzar para empezar!";
        document.getElementById('dado1').innerText = "?";
        document.getElementById('dado2').innerText = "?";
        document.getElementById('msg').innerText = "";
    }
</script>
