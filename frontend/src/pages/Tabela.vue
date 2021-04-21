<template>
  <q-page>
    <div class="row">
      <div class="col">
        <q-table title="Lista de Usuários" :data="info" :columns="columns" row-key="id">
            <template v-slot:body="props">
              <q-tr :props="props">
                <q-td key="avatar" :props="props">
                  <q-img style="height: 80px; max-width:80px; border-radius: 50px" :src="props.row.avatar"/>
                </q-td>
                <q-td key="first_name" :props="props">
                  {{ props.row.first_name }}
                </q-td>
                <q-td key="last_name" :props="props">
                  {{ props.row.last_name }}
                </q-td>
                <q-td key="email" :props="props">
                  {{ props.row.email }}
                </q-td>
              </q-tr>
            </template>
        </q-table>
      </div>
    </div>
  </q-page>
</template>

<script>
import axios from 'axios' // Importando Axios

export default {
  name: 'Tabela',
  data () {
    return {
      columns: [
        {
          name: 'avatar', // Campo da coluna
          required: true,
          label: 'Foto do Usuário', // Label do campo
          align: 'left', // Alinhamento
          sortable: true // Ordenação
        },
        {
          name: 'first_name', // Campo da coluna
          label: 'Nome', // Label do campo
          field: row => row.first_name,
          format: val => `${val}`, // Faz o vínculo do nome do campo virtual com o dado que vem da requisição
          align: 'left', // Alinhamento
          sortable: true // Ordenação
        },
        {
          name: 'last_name', // Campo da coluna
          label: 'Sobrenome', // Label do campo
          field: row => row.last_name,
          format: val => `${val}`, // Faz o vínculo do nome do campo virtual com o dado que vem da requisição
          align: 'left', // Alinhamento
          sortable: true // Ordenação
        },
        {
          name: 'email', // Campo da coluna
          label: 'E-mail', // Label do campo
          field: row => row.email,
          format: val => `${val}`, // Faz o vínculo do nome do campo virtual com o dado que vem da requisição
          align: 'left', // Alinhamento
          sortable: true // Ordenação
        }
      ],
      info: [] // Armazenará as informações pega na requisição
    }
  },
  methods: {
    getInfo () {
      this.$q.loading.show({ // Chamando o plugin de loading
        delay: 400
      })

      const options = {
        method: 'GET',
        url: 'https://reqres.in/api/users'
      }

      axios.request(options) // Fazendo a requisição na WebAPI (Poderia ser em um banco de dados)
        .then((resposta) => {
          this.info = resposta.data.data
          this.$q.loading.hide() // Encerrando o loading
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
    }
  },
  mounted () {
    this.getInfo()
  }
}
</script>
