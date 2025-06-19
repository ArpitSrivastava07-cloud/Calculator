<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>3D Glossy Calculator</title>
    <style>
        body {
            background: linear-gradient(135deg, #232526 0%, #414345 100%);
            min-height: 100vh;
            display: flex;
            align-items: center;
            justify-content: center;
            font-family: 'Segoe UI', Arial, sans-serif;
        }
        .calculator {
            background: linear-gradient(145deg, #232526 60%, #3a3a3a 100%);
            border-radius: 30px;
            box-shadow: 0 10px 30px #000a, 0 2px 8px #fff2 inset;
            padding: 32px 24px 24px 24px;
            width: 380px;
            display: flex;
            flex-direction: column;
            align-items: center;
            position: relative;
            animation: popin 0.7s cubic-bezier(.68,-0.55,.27,1.55);
        }
        @keyframes popin {
            0% { transform: scale(0.7) rotateX(30deg); opacity: 0; }
            100% { transform: scale(1) rotateX(0); opacity: 1; }
        }
        .display {
            width: 100%;
            background: linear-gradient(90deg, #222 60%, #444 100%);
            border-radius: 16px;
            box-shadow: 0 2px 8px #0006 inset, 0 0 16px #00ffe0a0 inset;
            color: #fff;
            font-size: 2.2rem;
            text-align: right;
            padding: 18px 16px;
            margin-bottom: 24px;
            letter-spacing: 1px;
            min-height: 48px;
            overflow-x: auto;
            text-shadow: 0 2px 8px #00ffe0a0, 0 1px 0 #fff2;
        }
        .buttons {
            width: 100%;
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            grid-template-rows: repeat(6, 60px);
            gap: 14px;
        }
        .btn {
            background: rgba(80, 120, 255, 0.18);
            border: 1.5px solid rgba(255,255,255,0.25);
            border-radius: 14px;
            box-shadow: 0 6px 18px #0007, 0 2px 8px #fff2 inset, 0 0 8px #00ffe0a0;
            color: #fff;
            font-size: 1.3rem;
            font-weight: 600;
            padding: 0;
            cursor: pointer;
            transition: transform 0.12s cubic-bezier(.68,-0.55,.27,1.55), box-shadow 0.12s, background 0.2s;
            outline: none;
            user-select: none;
            position: relative;
            z-index: 1;
            backdrop-filter: blur(8px) saturate(1.5);
            -webkit-backdrop-filter: blur(8px) saturate(1.5);
            overflow: hidden;
            min-width: 0;
            min-height: 0;
            height: 100%;
            width: 100%;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        .btn:before {
            content: '';
            position: absolute;
            top: 6px; left: 6px; right: 6px; bottom: 6px;
            border-radius: 10px;
            background: linear-gradient(120deg, #fff5 0%, #fff0 100%);
            z-index: 0;
            pointer-events: none;
        }
        .btn:after {
            content: '';
            position: absolute;
            top: 0; left: 0; right: 0; bottom: 0;
            border-radius: 14px;
            background: linear-gradient(120deg, rgba(255,255,255,0.18) 0%, rgba(255,255,255,0.05) 100%);
            z-index: 0;
            pointer-events: none;
        }
        .btn:hover {
            animation: btn-hover-glow 0.35s cubic-bezier(.68,-0.55,.27,1.55) forwards;
            box-shadow: 0 8px 24px #00ffe0cc, 0 2px 8px #fff2 inset, 0 0 16px #00ffe0a0;
        }
        .btn:active {
            animation: btn-press 0.18s cubic-bezier(.68,-0.55,.27,1.55);
            transform: scale(0.95) translateY(2px);
            box-shadow: 0 2px 6px #0008, 0 1px 2px #fff3 inset;
            background: rgba(80, 120, 255, 0.28);
        }
        @keyframes btn-press {
            0% { transform: scale(1) translateY(0); }
            50% { transform: scale(0.92) translateY(3px); }
            100% { transform: scale(0.97) translateY(2px); }
        }
        @keyframes btn-hover-glow {
            0% { box-shadow: 0 6px 18px #0007, 0 2px 8px #fff2 inset, 0 0 8px #00ffe0a0; }
            100% { box-shadow: 0 8px 24px #00ffe0cc, 0 2px 8px #fff2 inset, 0 0 16px #00ffe0a0; }
        }
        .btn.operator {
            background: linear-gradient(145deg, #ff512f 60%, #dd2476 100%);
            box-shadow: 0 6px 18px #ff512f55, 0 2px 8px #fff2 inset, 0 0 8px #ff512f80;
        }
        .btn.equal {
            background: linear-gradient(145deg, #11998e 60%, #38ef7d 100%);
            box-shadow: 0 6px 18px #38ef7d55, 0 2px 8px #fff2 inset, 0 0 8px #38ef7d80;
            grid-row: 6 / span 1;
            grid-column: 4 / span 1;
        }
        .btn.clear {
            background: linear-gradient(145deg, #f7971e 60%, #ffd200 100%);
            color: #222;
            box-shadow: 0 6px 18px #ffd20055, 0 2px 8px #fff2 inset, 0 0 8px #ffd20080;
        }
        .btn.adv {
            background: linear-gradient(145deg, #00c3ff 60%, #ffff1c 100%);
            color: #222;
            box-shadow: 0 6px 18px #00c3ff55, 0 2px 8px #fff2 inset, 0 0 8px #00c3ff80;
        }
        .btn.zero {
            grid-column: 1 / span 2;
        }
        .btn.parenthesis {
            background: linear-gradient(145deg, #f953c6 60%, #b91d73 100%);
            box-shadow: 0 6px 18px #f953c655, 0 2px 8px #fff2 inset, 0 0 8px #f953c680;
        }
        .btn:focus {
            outline: 2px solid #fff8;
        }
        @media (max-width: 450px) {
            .calculator {
                width: 99vw;
                padding: 10px 2px 10px 2px;
            }
            .display {
                font-size: 1.1rem;
                padding: 8px 4px;
            }
        }
    </style>
</head>
<body>
    <div class="calculator">
        <div class="display" id="display">0</div>
        <div class="buttons">
            <button class="btn clear" onclick="clearDisplay()">C</button>
            <button class="btn adv" onclick="sqrt()">√</button>
            <button class="btn adv" onclick="square()">x²</button>
            <button class="btn operator" onclick="inputOperator('÷')">÷</button>

            <button class="btn adv" onclick="reciprocal()">1/x</button>
            <button class="btn adv" onclick="toggleSign()">±</button>
            <button class="btn adv" onclick="inputOperator('%')">%</button>
            <button class="btn operator" onclick="inputOperator('×')">×</button>

            <button class="btn parenthesis" onclick="inputParenthesis('(')">(</button>
            <button class="btn parenthesis" onclick="inputParenthesis(')')">)</button>
            <button class="btn" onclick="backspace()">⌫</button>
            <button class="btn operator" onclick="inputOperator('-')">-</button>

            <button class="btn" onclick="inputNumber('7')">7</button>
            <button class="btn" onclick="inputNumber('8')">8</button>
            <button class="btn" onclick="inputNumber('9')">9</button>
            <button class="btn operator" onclick="inputOperator('+')">+</button>

            <button class="btn" onclick="inputNumber('4')">4</button>
            <button class="btn" onclick="inputNumber('5')">5</button>
            <button class="btn" onclick="inputNumber('6')">6</button>
            <button class="btn equal" onclick="calculate()">=</button>

            <button class="btn" onclick="inputNumber('1')">1</button>
            <button class="btn" onclick="inputNumber('2')">2</button>
            <button class="btn" onclick="inputNumber('3')">3</button>
            <button class="btn" style="visibility:hidden"></button>

            <button class="btn zero" onclick="inputNumber('0')">0</button>
            <button class="btn" onclick="inputDot()">.</button>
            <button class="btn" style="visibility:hidden"></button>
            <button class="btn" style="visibility:hidden"></button>
        </div>
    </div>
    <script>
        let display = document.getElementById('display');
        let currentInput = '0';
        let operator = null;
        let operand1 = null;
        let operand2 = null;
        let resultDisplayed = false;
        let parenthesisCount = 0;

        function updateDisplay() {
            display.textContent = currentInput;
        }

        function inputNumber(num) {
            if (resultDisplayed) {
                currentInput = num;
                resultDisplayed = false;
            } else if (currentInput === '0') {
                currentInput = num;
            } else {
                currentInput += num;
            }
            updateDisplay();
        }

        function inputDot() {
            if (resultDisplayed) {
                currentInput = '0.';
                resultDisplayed = false;
            } else {
                // Only allow one dot in the last number
                let parts = currentInput.split(/[^0-9.]/);
                let last = parts[parts.length - 1];
                if (!last.includes('.')) {
                    currentInput += '.';
                }
            }
            updateDisplay();
        }

        function clearDisplay() {
            currentInput = '0';
            operator = null;
            operand1 = null;
            operand2 = null;
            resultDisplayed = false;
            parenthesisCount = 0;
            updateDisplay();
        }

        function inputOperator(op) {
            if (resultDisplayed) {
                resultDisplayed = false;
            }
            if (/[-+×÷%]$/.test(currentInput)) {
                currentInput = currentInput.slice(0, -1) + op;
            } else {
                currentInput += op;
            }
            updateDisplay();
        }

        function inputParenthesis(par) {
            if (par === '(') {
                if (currentInput === '0' || resultDisplayed) {
                    currentInput = '(';
                } else if (/\d$/.test(currentInput)) {
                    currentInput += '×(';
                } else {
                    currentInput += '(';
                }
                parenthesisCount++;
            } else if (par === ')' && parenthesisCount > 0) {
                currentInput += ')';
                parenthesisCount--;
            }
            resultDisplayed = false;
            updateDisplay();
        }

        function calculate() {
            let expr = currentInput
                .replace(/×/g, '*')
                .replace(/÷/g, '/')
                .replace(/%/g, '%')
                .replace(/--/g, '+');
            // Balance parentheses
            for (let i = 0; i < parenthesisCount; i++) expr += ')';
            try {
                let result = Function('return ' + expr)();
                if (typeof result === 'number' && !isNaN(result) && isFinite(result)) {
                    currentInput = result.toString();
                    if (currentInput.length > 14) {
                        currentInput = parseFloat(currentInput).toExponential(8);
                    }
                } else {
                    currentInput = 'Error';
                }
            } catch {
                currentInput = 'Error';
            }
            updateDisplay();
            resultDisplayed = true;
            parenthesisCount = 0;
        }

        function backspace() {
            if (resultDisplayed || currentInput.length === 1) {
                currentInput = '0';
                resultDisplayed = false;
            } else {
                let last = currentInput.slice(-1);
                if (last === '(') parenthesisCount--;
                if (last === ')') parenthesisCount++;
                currentInput = currentInput.slice(0, -1);
            }
            updateDisplay();
        }

        function sqrt() {
            if (resultDisplayed) resultDisplayed = false;
            let match = currentInput.match(/(\d+\.?\d*)$/);
            if (match) {
                let num = parseFloat(match[1]);
                if (num >= 0) {
                    let sqrtVal = Math.sqrt(num);
                    currentInput = currentInput.replace(/(\d+\.?\d*)$/, sqrtVal.toString());
                } else {
                    currentInput = 'Error';
                    resultDisplayed = true;
                }
            } else if (/\)$/.test(currentInput)) {
                currentInput += '**0.5';
            }
            updateDisplay();
        }

        function square() {
            if (resultDisplayed) resultDisplayed = false;
            let match = currentInput.match(/(\d+\.?\d*)$/);
            if (match) {
                let num = parseFloat(match[1]);
                let sqVal = num * num;
                currentInput = currentInput.replace(/(\d+\.?\d*)$/, sqVal.toString());
            } else if (/\)$/.test(currentInput)) {
                currentInput += '**2';
            }
            updateDisplay();
        }

        function reciprocal() {
            if (resultDisplayed) resultDisplayed = false;
            let match = currentInput.match(/(\d+\.?\d*)$/);
            if (match) {
                let num = parseFloat(match[1]);
                if (num !== 0) {
                    let recVal = 1 / num;
                    currentInput = currentInput.replace(/(\d+\.?\d*)$/, recVal.toString());
                } else {
                    currentInput = 'Error';
                    resultDisplayed = true;
                }
            }
            updateDisplay();
        }

        function toggleSign() {
            if (resultDisplayed) resultDisplayed = false;
            let match = currentInput.match(/(\d+\.?\d*)$/);
            if (match) {
                let num = parseFloat(match[1]);
                let toggled = -num;
                currentInput = currentInput.replace(/(\d+\.?\d*)$/, toggled.toString());
            }
            updateDisplay();
        }

        // Keyboard support
        document.addEventListener('keydown', function(e) {
            if (e.key >= '0' && e.key <= '9') inputNumber(e.key);
            if (e.key === '.') inputDot();
            if (e.key === '+' || e.key === '-') inputOperator(e.key);
            if (e.key === '*' || e.key === 'x' || e.key === 'X') inputOperator('×');
            if (e.key === '/') inputOperator('÷');
            if (e.key === '%') inputOperator('%');
            if (e.key === 'Enter' || e.key === '=') calculate();
            if (e.key === 'Backspace') backspace();
            if (e.key === 'Escape') clearDisplay();
            if (e.key === '(') inputParenthesis('(');
            if (e.key === ')') inputParenthesis(')');
        });
    </script>
</body>
</html>
