<template>
  <div class="container">
    <h2>
      <span>Pomodoro</span>
      <button :disabled="estado==='comecar'"@click="iniciar">Iniciar
        <i class="glyphicon glyphicon-play"></i>
      </button>
      <button :disabled="estado!=='comecar'"@click="pausar">Pausar
        <i class="glyphicon glyphicon-play"></i>
      </button>
      <button :disabled="estado!=='comecar' && estado !== 'pausar'"@click="terminar">Resetar
        <i class="glyphicon glyphicon-play"></i>
      </button>
    </h2>
    <h3>{{ funcao }}</h3>
    <a v-if="estado === 'comecar' && this.pomodoroEstado === 'trabalho'">Deve se manter concentrado durante 25 minutos</a>
    <div class="well">
      <div class="pomodoro-timer">
        <span>{{ min }}</span>:<span>{{ sec }}</span>
      </div>
    </div>
  </div>
</template>
<script>
const POMODORO_ESTADO = {
  TRABALHO: "trabalho",
  DESCANSO: "descanso"
};
const ESTADO = {
  COMECAR: "comecar",
  TERMINAR: "terminar",
  PAUSAR: "pausar"
};
const TEMPO_PARA_TRABALHAR = 1;
const TEMPO_PARA_DESCANSO = 5;
export default {
  data: () => ({
    estado: ESTADO.TERMINAR,
    minuto: TEMPO_PARA_TRABALHAR,
    segundo: 0,
    pomodoroEstado: POMODORO_ESTADO.TRABALHO,
    timestamp: 0
  }),
  //criando titulo trabalhar ou descansar?
  computed: {
    funcao() {
      return this.pomodoroEstado === POMODORO_ESTADO.TRABALHO
        ? "Trabalhar!"
        : "Descansar!";
    },
    //acrescentando o 0 antes do numero quando chegar no valor menor q 10.
    min() {
      if (this.minuto < 10) {
        return "0" + this.minuto;
      }
      return this.minuto;
    },
    sec() {
      if (this.segundo < 10) {
        return "0" + this.segundo;
      }
      return this.segundo;
    }
  },
  methods: {
    iniciar() {
      this.estado = ESTADO.COMECAR;
      this._tick();
      this.intervalo = setInterval(this._tick, 1000);
    },
    pausar() {
      this.estado = ESTADO.PAUSAR;
      clearInterval(this.intervalo);
    },
    terminar() {
      alert("Tempo Reiniciado");
      this.estado = ESTADO.TERMINAR;
      clearInterval(this.intervalo);
      this.pomodoroEstado = POMODORO_ESTADO.TRABALHO;
      this.minuto = TEMPO_PARA_TRABALHAR;
      this.segundo = 0;
    },
    _tick() {
      if (this.segundo !== 0) {
        this.segundo--;
        return;
      }
      if (this.minuto !== 0) {
        this.minuto--;
        this.segundo = 59;
        return;
      }
      this.pomodoroEstado =
        this.pomodoroEstado === POMODORO_ESTADO.TRABALHO
          ? POMODORO_ESTADO.DESCANSO
          : POMODORO_ESTADO.TRABALHO;
      if (this.pomodoroEstado === POMODORO_ESTADO.TRABALHO) {
        this.minuto = TEMPO_PARA_TRABALHAR;
      } else {
        this.minuto = TEMPO_PARA_DESCANSO;
      }
    }
  }
};
</script>