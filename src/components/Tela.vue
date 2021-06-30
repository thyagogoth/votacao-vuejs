<template>
  <div class="urna-tela">
    <div v-if="tela != 'fim'" class="urna-tela-voto">
      <div class="urna-tela-voto-textos">
        <div class="urna-tela-voto-titulo">Seu voto para:</div>
        <div class="urna-tela-voto-tipo">{{ tela }}</div>

        <div class="urna-tela-voto-numeros">
          Números

          <div
            class="urna-tela-voto-numero"
            v-for="(value, key) in numeroVoto.padEnd(quantidadeNumeros, ' ')"
            :key="key"
          >
            {{ value }}
          </div>
        </div>
        <div class="urna-tela-voto-descricao">
          Nome:
          <strong>{{ candidato.nome ? candidato.nome : '--' }}</strong>
        </div>
        <div class="urna-tela-voto-descricao">
          Partido:
          <strong>{{ candidato.partido ? candidato.partido : '--' }}</strong>
        </div>
      </div>

      <div v-if="candidato.imagem" class="urna-tela-voto-imagem">
        <img :src="candidato.imagem" />
      </div>
      <div class="urna-tela-voto-instrucoes">
        <p>Aperte a tela:</p>
        <p>BRANCO para VOTAM EM <strong>BRANCO</strong></p>
        <p>LARANJA para <strong>CORRIGIR</strong></p>
        <p>VERDE para <strong>CONFIRMAR</strong></p>
      </div>
    </div>

    <div v-if="tela == 'fim'" class="urna-tela-fim">FIM</div>
  </div>
</template>

<script>
export default {
  name: 'Tela',
  props: {
    tela: String,
    numeroVoto: String,
    quantidadeNumeros: Number,
    candidato: Object,
  },
};
</script>

<style scoped>
.urna-tela {
  width: 56.5%;
  height: 100%;

  background-color: var(--ballot-box-screen-color);

  border-radius: 5px;
  border: 2px solid var(--light-border-color);
  padding: 20px;

  color: var(--dark-text-color);
}

.urna-tela-voto {
  display: flex;
  flex-wrap: wrap;
  width: 100%;
  height: 100%;
}

.urna-tela-voto-textos {
  flex: 1;
}

.urna-tela-voto-titulo {
  font-size: 20px;
}
.urna-tela-voto-tipo {
  font-size: 30px;
  font-weight: bold;
  margin: 20px 0;
  text-transform: uppercase;
}

.urna-tela-voto-numeros {
  display: flex;
  align-items: center;
}
.urna-tela-voto-numero {
  width: 45px;
  height: 55px;
  border: 1px solid var(--dark-border-color);

  margin-left: 8px;

  display: flex;
  align-items: center;
  justify-content: center;

  font-size: 30px;
  font-weight: 500;
}

.urna-tela-voto-descricao {
  margin: 20px 0 0 0;
}
.urna-tela-voto-imagem img {
  width: 130px;
  height: 156px;
  border: 1px solid var(--dark-border-color);
}

.urna-tela-voto-instrucoes {
  width: 100%;
  border-top: 3px solid var(--dark-border-color);
  font-size: 13px;
  margin-top: 20px;
}

.urna-tela-fim {
  display: flex;
  align-items: center;
  justify-content: center;

  font-size: 120px;
  width: 100%;
  height: 100%;
}
</style>
