<template>
  <div id="app">
    <div class="urna">
      <Tela
        :tela="tela"
        :numeroVoto="numeroVoto"
        :quantidadeNumeros="quantidadeNumeros"
        :candidato="candidato"
      />
      <Teclado
        :adicionarNumero="adicionarNumero"
        :corrigir="corrigir"
        :confirmar="confirmar"
        :votarEmBranco="votarEmBranco"
      />
    </div>
  </div>
</template>

<script>
import '@/assets/css/style.css';
import Tela from '@/components/Tela';
import Teclado from '@/components/Teclado';

import confirmAudio from '@/assets/audios/confirm.wav';
import keyAudio from '@/assets/audios/key.wav';

export default {
  name: 'App',
  components: {
    Tela,
    Teclado,
  },
  methods: {
    adicionarNumero(numero) {
      this.executarSom(keyAudio);
      // Verifica limite de digitos por tipo de voto
      if (this.numeroVoto.length == this.quantidadeNumeros) {
        return false;
      }
      // adiciona o número selecionado
      this.numeroVoto += `${numero}`;

      this.verificarCandidato();
    },

    verificarCandidato() {
      //voto incompleto
      if (this.numeroVoto.length < this.quantidadeNumeros) {
        return false;
      }
      // verifica se existe candidato
      if (this.candidatos[this.tela][this.numeroVoto]) {
        this.candidato = this.candidatos[this.tela][this.numeroVoto];
        return true;
      }
      //voto nulo
      this.candidato = {
        nome: 'VOTO NULO',
        partido: 'VOTO NULO',
        imagem: '',
      };
    },

    corrigir() {
      this.executarSom(keyAudio);
      this.limpar();
    },
    limpar() {
      this.candidato = {};
      this.numeroVoto = '';
    },

    confirmar() {
      if (this.numeroVoto.length < this.quantidadeNumeros) {
        return false;
      }
      return this.avancarTela();
    },
    avancarTela() {
      this.executarSom(keyAudio);
      if (this.tela == 'prefeito') {
        this.tela = 'vereador';
        this.quantidadeNumeros = 5;
        return this.limpar();
      }

      //FINALIZA  A VOTAÇÃO
      this.tela = 'fim';
      this.executarSom(confirmAudio);

      // instancia atual
      let instancia = this;

      //voltar ao inicio
      setTimeout(function() {
        instancia.tela = 'prefeito';
        instancia.quantidadeNumeros = 2;
        return instancia.limpar();
      }, 3000);
    },

    votarEmBranco() {
      this.executarSom(keyAudio);
      if (this.tela == 'fim') {
        return false;
      }
      this.limpar();
      this.avancarTela();
    },

    executarSom(arquivoSom) {
      if (arquivoSom) {
        let audio = new Audio(arquivoSom);
        audio.play();
      }
    },
  },
  data() {
    return {
      tela: 'prefeito',
      numeroVoto: '',
      quantidadeNumeros: 2,
      candidato: {},
      candidatos: {
        prefeito: {
          '17': {
            nome: 'Felipe Arcaro',
            partido: 'IM PAU',
            imagem: './images/arcaro.jpg',
          },
          '24': {
            nome: 'Gabriel Lehmen',
            partido: 'TI',
            imagem: './images/gabriel.jpg',
          },
        },
        vereador: {
          '01234': {
            nome: 'Thiago Fernando',
            partido: 'TI',
            imagem: './images/135-hover.png',
          },
          '00420': {
            nome: 'Mateus Patel',
            partido: 'SUP',
            imagem: './images/patel.jpg',
          },
          '00333': {
            nome: 'Tarça',
            partido: 'SUP',
            imagem: './images/tarcicio.jpg',
          },
          '02468': {
            nome: 'Teteu',
            partido: 'IM PAU',
            imagem: './images/freitas.jpg',
          },
          '03690': {
            nome: 'Borges',
            partido: 'IM PAU',
            imagem: './images/borges.jpg',
          },
        },
      },
    };
  },
};
</script>

<style>
#app {
  background-color: var(--background-color);

  width: 100%;
  height: 100%;

  display: flex;
  justify-content: center;
  align-items: center;
}

.urna {
  width: 1000px;
  height: 500px;

  background-color: var(--ballot-box-background-color);
  padding: 30px;
  border-radius: 5px;

  display: flex;
  justify-content: space-between;
}
</style>
