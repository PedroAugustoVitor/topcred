<script>
import 'bootstrap'

export default {
  data: function () {
    return {
      rates: {
        receivable: [0, 1.15, 1.16, 1.17, 1.18, 1.19, 1.20, 1.21, 1.22, 1.23, 1.24, 1.25, 1.26, 1.27, 1.28, 1.29, 1.30, 1.31, 1.32],
        limit: [0, 0.130434782608696, 0.137931034482759, 0.145299145299145, 0.15254237288135597, 0.15966386554621803, 0.16666666666666696, 0.173553719008264, 0.180327868852459, 0.18699186991869898, 0.19354838709677402, 0.19999999999999996, 0.20634920634920606, 0.21259842519685002, 0.21875, 0.224806201550388, 0.23076923076923106, 0.23664122137404597, 0.242424242424242]

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
