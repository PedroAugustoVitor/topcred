<script>
import 'bootstrap'

export default {
  data: function () {
    return {
      rates: {
        receivable: [0, 1.1764705882352942, 1.1904761904761905, 1.2048192771084338, 1.2195121951219512, 1.2345679012345678, 1.25, 1.2658227848101264, 1.282051282051282, 1.2987012987012987, 1.3157894736842106, 1.3333333333333333, 1.3513513513513513, 1.36986301369863, 1.3888888888888888, 1.4084507042253522, 1.4285714285714286, 1.4492753623188408, 1.4705882352941178],
        limit: [0, 0.15, 0.16, 0.17, 0.18, 0.19, 0.20, 0.21, 0.22, 0.23, 0.24, 0.25, 0.26, 0.27, 0.28, 0.29, 0.30, 0.31, 0.32]

      },
      value: 1000,
      term: 1,
      limit: false,
      result: {
        willCharge: 0,
        willReceive: 0,
        portion: 0
      }
    }
  },
  mounted() {
    this.process()
  },
  methods: {
    process: function () {
      this.limit ? this.result = {
        willCharge: this.formatter(this.value),
        willReceive: this.formatter(this.value - (this.value * this.rates.limit[this.term])),
        portion: this.formatter(this.value / this.term)
      } : this.result = {
        willCharge: this.formatter(this.value * this.rates.receivable[this.term]),
        willReceive: this.formatter(this.value),
        portion: this.formatter(this.value * this.rates.receivable[this.term] / this.term)
      }
    },
    formatter: function (e) {
      return e.toLocaleString("pt-BR", {
        style: "currency",
        currency: "BRL"
      })
    },
    copy: function () {
      let e = `Simulador TopCred\nSerá cobrado no cartão: ${this.result.willCharge}.\nVocê receberá: ${this.result.willReceive}.\nSer${this.term > 1 ? "ão" : "á"} ${this.term} parcela${this.term > 1 ? "s" : ""} de ${this.result.portion}`;
      navigator.clipboard.writeText(e)
    }
  }
}
</script>

<template>
  <div class="text-bg-dark">
    <div class="col-6 offset-3">
      <div class="col-4 offset-4 pt-5">
        <img alt="Logo" class="img-fluid" src="../img/logo.png">
      </div>
      <div class="mb-3">
        <label for="exampleFormControlInput1" class="form-label">Valor: </label>
        <input type="number" class="form-control" v-model="value" @change="process">
      </div>
      <div class="col-12">
        <label class="form-label">Número de parcelas: </label>
        <input type="range" class="form-range" min="1" max="18" v-model="term" @change="process">
        <h2 class="text-center">{{ term }}</h2>
      </div>
      <div class="form-check form-switch">
        <input class="form-check-input" type="checkbox" role="switch" v-model="limit" @change="process">
        <label class="form-check-label">Calcular {{limit?"a partir do limite":"valor à receber"}}</label>
      </div>
      <div class="jumbotron">
        <h1 class="display-6">Detalhes da simulação</h1>
        <p class="lead">Será cobrado no cartão: R$: {{ result.willCharge }}</p>
        <p class="lead">Você receberá: R$: {{ result.willReceive }}</p>
        <hr class="my-4">
        <p>Ser{{ parseInt(term) === 1 ? "á" : "ão" }} {{ term }} parcela{{ parseInt(term) === 1 ? "" : "s" }} de R$:
          {{ result.portion }}</p>
      </div>
      <p class="lead">
        <a class="btn btn-outline-primary btn-lg" role="button" @click="copy">Copiar</a>
      </p>
    </div>
  </div>
</template>

<style scoped>

</style>
