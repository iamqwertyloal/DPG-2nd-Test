<script>
  let firstValue = 0;
  let operatorValue = '';
  let awaitingNextValue = false;
  let displayValue = '0';

  function calculate (operator, firstNumber, secondNumber) {
    switch (operator) {
      case "/":
        return firstNumber / secondNumber
        break;
      case "*":
        return firstNumber * secondNumber
        break;
      case "%":
        return firstNumber % secondNumber
        break;
      case "+":
        return firstNumber + secondNumber
        break;
      case "-":
        return firstNumber - secondNumber
        break;
      case "=":
        return secondNumber
        break;
    }
  }
  
  const sendNumberValue = (number) => {
    if (awaitingNextValue) {
      displayValue = number;
      awaitingNextValue = false;
    } else {
      displayValue = displayValue === '0' ? number : displayValue + number;
    }
  };

  const addDecimal = () => {
    if (typeof(displayValue) === "number") {
      displayValue = displayValue.toString();
    }
    if (!displayValue.includes('.')) {
      displayValue += '.';
      awaitingNextValue = false;
    }
  };

  const useOperator = (operator) => {
    const currentValue = Number(displayValue);
    if (operatorValue && awaitingNextValue) {
      operatorValue = operator;
      return;
    }
    if (!firstValue) {
      firstValue = currentValue;
    } else {
      const calculation = calculate(operatorValue, firstValue, currentValue);
      displayValue = calculation;
      firstValue = calculation;
    }
    awaitingNextValue = true;
    operatorValue = operator;
  };

  const clearRightmost = () => {
    const newValue = Array.from(String(displayValue));
    if (newValue.length > 1) {
      newValue.pop();
      displayValue = newValue.join("");
      firstValue = Number(displayValue)
    } else {
      displayValue = "0";
      firstValue = 0;
    }
  }

  const swapSign = () => {
    if (typeof(displayValue) === "number") {
      displayValue = -(displayValue);
      firstValue = displayValue;
      displayValue.toString();
      return
    }

    const newValue = Array.from(String(displayValue));

    if (displayValue.includes('-')) {
      newValue.shift();
      displayValue = newValue.join("");
    } else {
      newValue.unshift("-");
      displayValue = newValue.join("");
    }
  }

  const resetAll = () => {
    firstValue = 0;
    operatorValue = '';
    awaitingNextValue = false;
    displayValue = '0';
  };
</script>

<div class="calculator">
  <div class="calculator-display">
    <h1>{displayValue}</h1>
  </div>
  <div class="calculator-buttons">
    <button on:click={resetAll}>AC</button>
    <button on:click={swapSign}>+/-</button>
    <button on:click={() => useOperator('%')} class="operator" value="%">%</button>
    <button on:click={clearRightmost} class="backspace orange">&#9003;</button>
    <button on:click={() => sendNumberValue('7')} value="7">7</button>
    <button on:click={() => sendNumberValue('8')}  value="8">8</button>
    <button on:click={() => sendNumberValue('9')}  value="9">9</button>
    <button on:click={() => useOperator('/')} class="orange operator" value="/">÷</button>
    <button on:click={() => sendNumberValue('4')}  value="4">4</button>
    <button on:click={() => sendNumberValue('5')} value="5">5</button>
    <button on:click={() => sendNumberValue('6')} value="6">6</button>
    <button on:click={() => useOperator('*')} class="orange operator" value="*">×</button>
    <button on:click={() => sendNumberValue('1')} value="1">1</button>
    <button on:click={() => sendNumberValue('2')}  value="2">2</button>
    <button on:click={() => sendNumberValue('3')} value="3">3</button>
    <button on:click={() => useOperator('-')} class="orange operator" value="-">-</button>
    <button on:click={() => addDecimal()} class="decimal" value=".">,</button>
    <button on:click={() => sendNumberValue('0')} value="0">0</button>
    <button on:click={() => useOperator('=')} class="operator" value="=">=</button>
    <button on:click={() => useOperator('+')} class="orange operator" value="+">+</button>
  </div>
</div>