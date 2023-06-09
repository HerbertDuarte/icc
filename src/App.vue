<template>
  <section>
    <h1> <span class="icon">+</span> <span class="title">Soma de números binários</span></h1>
    <form v-on:submit="handleSubmit">
      <input required placeholder="Digite o primeiro número da soma" type="number" v-model="number1" name="number1"
        id="number1">
      <input required placeholder="Digite o segundo número da soma" type="number" v-model="number2" name="number2"
        id="number2">
      <button type="submit">Somar</button>
    </form>
    <p v-if="!error">{{ result }}</p>
    <p class="error" v-if="error">{{ error }}</p>
  </section>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      result: 'O resultado aparecerá aqui!',
      number1: '',
      number2: '',
      error: ''
    }
  },
  methods: {
    handleSubmit(event) {
      event.preventDefault()

      const n1 = this.number1.toString()
      const n2 = this.number2.toString()

      const regex = /^[01]+$/;
      if (regex.test(n1) && regex.test(n2)) {
        this.result = 'resultado: ' + this.somaBinaria(n1, n2)
      } else {
        this.error = "Digite apenas números binários! Ex.: 10101 + 110"
        this.result = ''
      }
    },

    somaBinaria(binario1, binario2) {
      // Adiciona zeros à esquerda para alinhar os números binários
      const maiorComprimento = Math.max(binario1.length, binario2.length);
      binario1 = this.adicionarZerosEsquerda(binario1, maiorComprimento);
      binario2 = this.adicionarZerosEsquerda(binario2, maiorComprimento);

      console.log(binario1, binario2)

      let resultado = '';
      let carry = 0;

      // Realiza a soma bit a bit
      for (let i = binario1.length - 1; i >= 0; i--) {
        let bit1 = parseInt(binario1[i]);
        let bit2 = parseInt(binario2[i]);

        let soma = bit1 + bit2 + carry;

        if (soma === 0 || soma === 1) {
          resultado = soma.toString() + resultado;
          carry = 0;
        } else if (soma === 2) {
          resultado = '0' + resultado;
          carry = 1;
        } else if (soma === 3) {
          resultado = '1' + resultado;
          carry = 1;
        }
      }

      // Verifica se há carry restante
      if (carry === 1) {
        resultado = '1' + resultado;
      }

      return resultado
    },

    adicionarZerosEsquerda(binario, comprimento) {
      while (binario.length < comprimento) {
        binario = '0' + binario;
      }
      return binario;
    }
  }
}
</script>
