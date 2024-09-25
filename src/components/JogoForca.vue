<template>
  <div class="forca-container">
    <h1>Jogo da Forca</h1>

    <!-- Escolha de categorias -->
    <div v-if="!jogoIniciado" class="categorias">
      <h2>Escolha uma categoria:</h2>
      <button v-for="(palavras, categoria) in categorias" :key="categoria" @click="escolherCategoria(categoria)">
        {{ categoria }}
      </button>
    </div>

    <!-- Exibição do jogo após a escolha da categoria -->
    <div v-else>
      <!-- Desenho da forca -->
      <div class="forca-desenho">
        <img :src="imagensForca[erros]" alt="Forca" />
      </div>

      <!-- Palavra oculta -->
      <div class="palavra">
        <span v-for="(letra, index) in palavraExibida" :key="index">{{ letra }}</span>
      </div>

      <!-- Letras do alfabeto -->
      <div class="letras">
        <button v-for="letra in alfabeto" :key="letra" @click="adivinharLetra(letra)" :disabled="letrasAdivinhadas.includes(letra)">
          {{ letra }}
        </button>
      </div>

      <!-- Exibição do resultado -->
      <div v-if="jogoAcabou" class="resultado">
        <h2>{{ resultadoMensagem }}</h2>
        <button @click="reiniciarJogo">Jogar Novamente</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      categorias: {
        Animais: ['ELEFANTE', 'GIRAFA', 'LEAO', 'CACHORRO'],
        Tecnologia: ['COMPUTADOR', 'TELEFONE', 'INTERNET', 'PROGRAMACAO'],
        Frutas: ['BANANA', 'MORANGO', 'ABACAXI', 'LARANJA']
      },
      palavraSecreta: '',
      palavraExibida: [],
      alfabeto: 'ABCDEFGHIJKLMNOPQRSTUVWXYZ'.split(''),
      letrasAdivinhadas: [],
      erros: 0,
      maxErros: 6,
      jogoIniciado: false,
      jogoAcabou: false,
      resultadoMensagem: '',
      imagensForca: [
        '/imagens/forca0.png',
        '/imagens/forca1.png',
        '/imagens/forca2.png',
        '/imagens/forca3.png',
        '/imagens/forca4.png',
        '/imagens/forca5.png',
        '/imagens/forca6.png'
      ]
    };
  },
  methods: {
    escolherCategoria(categoria) {
      const palavras = this.categorias[categoria];
      this.palavraSecreta = palavras[Math.floor(Math.random() * palavras.length)];
      this.palavraExibida = this.palavraSecreta.split('').map(() => '_');
      this.jogoIniciado = true;
    },
    adivinharLetra(letra) {
      if (!this.letrasAdivinhadas.includes(letra)) {
        this.letrasAdivinhadas.push(letra);

        if (this.palavraSecreta.includes(letra)) {
          this.palavraSecreta.split('').forEach((char, index) => {
            if (char === letra) {
              this.palavraExibida[index] = letra;
            }
          });
        } else {
          this.erros++;
        }

        this.verificarFimDeJogo();
      }
    },
    verificarFimDeJogo() {
      if (this.erros >= this.maxErros) {
        this.jogoAcabou = true;
        this.resultadoMensagem = `Você perdeu! A palavra era "${this.palavraSecreta}".`;
      } else if (this.palavraExibida.join('') === this.palavraSecreta) {
        this.jogoAcabou = true;
        this.resultadoMensagem = 'Parabéns, você ganhou!';
      }
    },
    reiniciarJogo() {
      this.palavraSecreta = '';
      this.palavraExibida = [];
      this.letrasAdivinhadas = [];
      this.erros = 0;
      this.jogoIniciado = false;
      this.jogoAcabou = false;
      this.resultadoMensagem = '';
    }
  }
};
</script>

<style scoped>
.forca-container {
  text-align: center;
  font-family: 'Arial', sans-serif;
  margin-top: 30px;
}

.categorias {
  display: flex;
  justify-content: center;
  gap: 20px;
}

.palavra {
  font-size: 2.5em;
  letter-spacing: 10px;
  margin: 20px 0;
}

.letras {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  margin-top: 20px;
}

.letras button {
  margin: 5px;
  padding: 10px;
  font-size: 1.2em;
  width: 50px;
}

.forca-desenho {
  margin: 20px 0;
}

.forca-desenho img {
  width: 200px;
}

.resultado {
  margin-top: 20px;
}

.resultado h2 {
  font-size: 2em;
  color: #d9534f;
}

.resultado button {
  padding: 10px 20px;
  font-size: 1.2em;
  background-color: #5cb85c;
  color: white;
  border: none;
  cursor: pointer;
  border-radius: 5px;
}

.resultado button:hover {
  background-color: #4cae4c;
}
</style>
