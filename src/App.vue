<template>
  <div class="calculator">
    <h1>Calculadora Aritmética</h1>

    <input type="number" v-model.number="num1" placeholder="Digite o primeiro número" />
    
    <select v-model="operacao">
      <option value="+">+</option>
      <option value="-">−</option>
      <option value="*">×</option>
      <option value="/">÷</option>
    </select>
    
    <input type="number" v-model.number="num2" placeholder="Digite o segundo número" />

    <div class="resultado">
      Resultado: <strong>{{ resultado }}</strong>
    </div>
  </div>
</template>

<script>
import { ref, computed } from 'vue'

export default {
  setup() {
    const num1 = ref(0)
    const num2 = ref(0)
    const operacao = ref('+')

    const resultado = computed(() => {
      switch (operacao.value) {
        case '+': return num1.value + num2.value
        case '-': return num1.value - num2.value
        case '*': return num1.value * num2.value
        case '/': return num2.value !== 0 ? (num1.value / num2.value).toFixed(2) : 'Divisão por zero'
        default: return 0
      }
    })

    return { num1, num2, operacao, resultado }
  }
}
</script>

<style scoped>
.calculator {
  max-width: 320px;
  margin: 2rem auto;
  padding: 2rem;
  border: 1px solid #ccc;
  border-radius: 10px;
  font-family: Arial, sans-serif;
  text-align: center;
}
input, select {
  width: 100%;
  padding: 0.6rem;
  margin: 0.5rem 0;
  font-size: 1rem;
}
.resultado {
  margin-top: 1rem;
  font-size: 1.2rem;
}
</style>
