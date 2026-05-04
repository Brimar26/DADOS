
<div id="game-container">
    <h2>🎲 Reto de Dados: MateVida Digital</h2>
    <div class="dice-wrapper">
        <div id="dice1" class="dice">1</div>
        <div id="dice2" class="dice">1</div>
    </div>
    
    <button id="roll-btn" onclick="playGame()">Lanzar Dados</button>
    
    <div id="challenge-box" class="hidden">
        <p id="challenge-text"></p>
        <input type="number" id="user-answer" placeholder="Tu respuesta...">
        <button onclick="checkAnswer()">Verificar</button>
    </div>
    
    <p id="feedback"></p>
</div>

<!-- Estilos Visuales (Vibrantes) -->
<style>
    #game-container {
        background: #ffffff;
        border: 4px solid #ffd700; /* Dorado MateVida */
        border-radius: 15px;
        padding: 20px;
        text-align: center;
        max-width: 400px;
        margin: auto;
        font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    }
    .dice-wrapper {
        display: flex;
        justify-content: center;
        gap: 20px;
        margin: 20px 0;
    }
    .dice {
        width: 60px;
        height: 60px;
        background: #2196F3;
        color: white;
        font-size: 30px;
        line-height: 60px;
        border-radius: 10px;
        font-weight: bold;
        box-shadow: 0 4px #1565C0;
    }
    #roll-btn {
        background: #4CAF50;
        color: white;
        border: none;
        padding: 10px 20px;
        font-size: 18px;
        border-radius: 5px;
        cursor: pointer;
        transition: 0.3s;
    }
    #roll-btn:hover { background: #45a049; }
    #challenge-box { margin-top: 20px; padding: 15px; background: #f9f9f9; border-radius: 8px; }
    .hidden { display: none; }
    #feedback { font-weight: bold; margin-top: 10px; }
</style>

<!-- Lógica del Juego (Nivel Intermedio) -->
<script>
    let val1, val2, correctAnswer;

    function playGame() {
        // Generar números aleatorios del 1 al 6
        val1 = Math.floor(Math.random() * 6) + 1;
        val2 = Math.floor(Math.random() * 6) + 1;
        
        // Actualizar visualmente los dados
        document.getElementById('dice1').innerText = val1;
        document.getElementById('dice2').innerText = val2;
        
        // Crear reto: Sumar y multiplicar por 5 (Nivel 4to/5to grado)
        correctAnswer = (val1 + val2) * 5;
        
        document.getElementById('challenge-text').innerHTML = 
            `<strong>Reto de Lógica:</strong><br>Suma los dados y multiplica el resultado por 5.`;
        
        document.getElementById('challenge-box').classList.remove('hidden');
        document.getElementById('feedback').innerText = "";
        document.getElementById('user-answer').value = "";
    }

    function checkAnswer() {
        const userAns = parseInt(document.getElementById('user-answer').value);
        const feedback = document.getElementById('feedback');
        
        if (userAns === correctAnswer) {
            feedback.innerText = "¡Correcto! +200 Saberes Ganados 🌟";
            feedback.style.color = "green";
        } else {
            feedback.innerText = "Casi... ¡Intenta de nuevo! 🤔";
            feedback.style.color = "red";
        }
    }
</script>
