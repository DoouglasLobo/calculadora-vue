<template>
  <div class="calculator">
    <div class="display">
      <div class="expression">{{ expression || '0' }}</div>
      <div class="result">{{ result }}</div>
    </div>
    <div class="keys">
      <button @click="clearAll" class="key key--span-2">C</button>
      <button @click="inputOperator('%')">%</button>
      <button @click="inputOperator('/')">÷</button>

      <button @click="inputDigit('7')">7</button>
      <button @click="inputDigit('8')">8</button>
      <button @click="inputDigit('9')">9</button>
      <button @click="inputOperator('*')">×</button>

      <button @click="inputDigit('4')">4</button>
      <button @click="inputDigit('5')">5</button>
      <button @click="inputDigit('6')">6</button>
      <button @click="inputOperator('-')">−</button>

      <button @click="inputDigit('1')">1</button>
      <button @click="inputDigit('2')">2</button>
      <button @click="inputDigit('3')">3</button>
      <button @click="inputOperator('+')">+</button>

      <button @click="inputDigit('0')" class="key--span-2">0</button>
      <button @click="inputDigit('.')">.</button>
      <button @click="calculate" class="key--equals">=</button>
    </div>
  </div>
</template>

<script>
import { ref, computed } from 'vue'

export default {
  setup() {
    const expression = ref('')
    const result = computed(() => {
      try {
        // Evita eval em string vazia ou término em operador
        if (!expression.value || /[+\-*/.%]$/.test(expression.value)) {
          return ''
        }
        // Avalia expressão e formata
        const val = Function(`"use strict";return (${expression.value})`)()
        return Number.isFinite(val) ? val : 'Erro'
      } catch {
        return 'Erro'
      }
    })

    function inputDigit(d) {
      // Prevê dois pontos seguidos
      if (d === '.' && expression.value.slice(-1) === '.') return
      expression.value += d
    }

    function inputOperator(op) {
      // Substitui operador repetido ou no início
      if (!expression.value && op !== '-') return
      if (/[+\-*/.%]$/.test(expression.value)) {
        expression.value = expression.value.slice(0, -1) + op
      } else {
        expression.value += op
      }
    }

    function clearAll() {
      expression.value = ''
    }

    function calculate() {
      if (result.value !== '' && result.value !== 'Erro') {
        expression.value = String(result.value)
      }
    }

    return {
      expression,
      result,
      inputDigit,
      inputOperator,
      clearAll,
      calculate,
    }
  },
}
</script>

<style scoped>
.calculator {
  width: 260px;
  margin: 2rem auto;
  border-radius: 10px;
  overflow: hidden;
  box-shadow: 0 4px 10px rgba(0,0,0,0.3);
  background: #222;
}
.display {
  background: #444;
  color: #fff;
  text-align: right;
  padding: 1rem;
}
.expression {
  font-size: 1.2rem;
  opacity: 0.7;
}
.result {
  font-size: 2rem;
  margin-top: 0.2rem;
}
.keys {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
}
button {
  font-size: 1.4rem;
  padding: 1rem;
  border: 1px solid #333;
  background: #333;
  color: #fff;
  cursor: pointer;
}
button:active {
  background: #555;
}
.key--span-2 {
  grid-column: span 2;
}
.key--equals {
  background: #f57c00;
  color: #fff;
}
.key--equals:active {
  background: #fb8c00;
}
</style>
