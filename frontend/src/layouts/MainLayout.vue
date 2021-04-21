<template>
  <q-layout view="lHh Lpr lFf" >
    <q-header elevated>
      <q-toolbar>
        <q-btn
          flat
          dense
          round
          icon="menu"
          aria-label="Menu"
          @click="leftDrawerOpen = !leftDrawerOpen"
        />

        <q-toolbar-title>
          Quasar App
        </q-toolbar-title>

        <div>Quasar v{{ $q.version }}</div>
      </q-toolbar>
    </q-header>

    <q-drawer
      v-model="leftDrawerOpen"
      show-if-above
      bordered
      content-class="bg-grey-1"
    >
      <q-list>
        <q-item-label
          header
          class="text-grey-8"
        >
          Menu Interativo
        </q-item-label>
        <EssentialLink
          v-for="link in essentialLinks"
          :key="link.title"
          v-bind="link"
        />
      </q-list>
    </q-drawer>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
import EssentialLink from 'components/EssentialLink.vue'

const linksData = [
  {
    title: 'Home',
    caption: 'Página Principal',
    icon: 'home',
    link: ''
  },
  {
    title: 'Gráfico',
    caption: 'Legenda do Gráfico',
    icon: 'school',
    link: '/grafico'
  },
  {
    title: 'Tabela',
    caption: 'Legenda da Tabela',
    icon: 'code',
    link: '/tabela'
  },
  {
    title: 'Linkedin',
    caption: '@QuasarFramework',
    icon: 'public',
    target: '_blank',
    link: 'https://facebook.quasar.dev'
  }
]

export default {
  name: 'MainLayout',
  components: { EssentialLink },
  data () {
    return {
      leftDrawerOpen: false,
      essentialLinks: linksData
    }
  }
}
</script>
