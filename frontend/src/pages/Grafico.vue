<template>
  <q-page>
    <div class="row q-mt-md q-col-gutter-md">
      <div class="col">
        <q-card>
          <q-card-section>
            <span class="text-h5 text-weight-bold">Gráfico Populacional</span>
          </q-card-section>

          <q-card-section v-if="isReady">
            <!-- Pegamos essa linha abaixo lá na documentação e colocamos aqui pra importar esse tipo de gráfico -->
            <apexchart type="bar" height="350" :options="chartOptions" :series="series" :key="chart-items-`${this.dados.length}`"></apexchart>
            <!-- A :key é adicionada ali para sempre que mudar o array de dados se o tamanho dele alterar, o componente vai sofrer re-render -->
          </q-card-section>
        </q-card>
      </div>
    </div>
  </q-page>
</template>

<script>
import axios from 'axios' // Importando Axios

export default {
  name: 'Grafico',
  data () {
    return {
      dados: [],
      series: [
        {
          name: 'Country',
          data: []
        }
      ],
      chartOptions: {
        chart: {
          type: 'bar',
          height: 350
        },
        plotOptions: {
          bar: {
            horizontal: false,
            columnWidth: '55%',
            endingShape: 'rounded',
            dataLabels: {
              position: 'top' // top, center, bottom
            }
          }
        },
        dataLabels: {
          enabled: true,
          formatter: (val) => {
            return val.toLocaleString('pt-BR')
          }
        },
        stroke: {
          show: true,
          width: 2,
          colors: ['transparent']
        },
        xaxis: {
          categories: []
        },
        yaxis: {
          title: {
            text: 'Population'
          }
        },
        fill: {
          opacity: 1
        },
        tooltip: {
          y: {
            formatter: function (val) {
              return val.toLocaleString('pt-BR') + ' population'
            }
          }
        }
      }
    }
  },
  computed: {
    isReady () {
      return this.dados.length > 0
    }
  },
  methods: {
    pegarInformacoes () {
      this.$q.loading.show({ // Chamando o loading
        delay: 400
      })

      axios.get('http://127.0.0.1:8000/api/countries') // Fazendo a requisição no API
        .then((resposta) => {
          this.dados = resposta.data
          this.montarGraficos(resposta.data)
        })
        .catch((erro) => {
          // Chamando o plugin de notificação
          this.$q.notify({
            position: 'center', // Posição que a mensagem aparecerá
            timeout: 3000, // Tempo de exposição da mensagem no browser (3000 = 3 segundos)
            color: 'negative', // Cor do background da mensagem
            textColor: 'white', // Cor do texto da mensagem
            actions: [{ icon: 'check', color: 'white' }], // Ícone e cor do ícone
            message: 'Erro na requisição. Erro: ' + erro // Texto da mensagem
          })

          this.$q.loading.hide() // Encerrando o loading
        })
    },
    montarGraficos (dados) {
      dados.map(this.organizarDados)
      // console.log(this.series[0].data)
      // console.log(this.series[0].name)
      // console.log(this.chartOptions.xaxis.categories)
      this.$q.loading.hide() // Encerrando o loading
    },
    organizarDados (item) {
      this.series[0].data.push(item.population)
      this.chartOptions.xaxis.categories.push(`${item.name} | ${item.continent}`)
    }
  },
  mounted () {
    this.pegarInformacoes()
  }
}
</script>
