<template>
  <q-page class="container">
    <div class="row justify-center q-gutter-xl" style="margin-top: 20vh">

      <div class="col-4">
          <div class="text-center">
            <span class="text-h5 text-bold text-grey-8">Grafico Populacional</span>
          </div>
          <q-item class="full-width justify-center grafico" style="height: 30vh" clickable to="grafico">
            <span class="text-h4 text-grey-8">Clique</span>
          </q-item>
      </div>

      <div class="col-4">
        <div class="text-center">
          <span class="text-h5 text-bold text-grey-8">Tabela com Lista de Usuários</span>
        </div>
        <q-item class="full-width justify-center tabela" style="height: 30vh" clickable to="tabela">
            <span class="text-h4 text-grey-8">Clique</span>
        </q-item>
      </div>

    </div>
  </q-page>
</template>

<script>
export default {
  name: 'PageIndex'
}
</script>

<style scoped>
  .grafico {
    background-image: url('./../assets/grafico.jpg');
    background-repeat: no-repeat;
    background-size: cover;
  }

  .tabela {
    background-image: url('./../assets/lista2.jpg');
    background-repeat: no-repeat;
    background-size: cover;
  }
</style>
