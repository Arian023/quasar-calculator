<template>
  <q-layout view="hHh LpR fFf">
    <q-header elevated>
      <q-toolbar>
        <q-btn
          flat
          dense
          round
          icon="menu"
          aria-label="Menu"
          @click="toggleLeftDrawer" />

        <q-toolbar-title>
          <q-avatar>
            <img src="/icons/favicon.png" />
          </q-avatar>
          Calculadora
        </q-toolbar-title>

        <div><!-- Right side text - empty --></div>
      </q-toolbar>
    </q-header>

    <q-drawer v-model="leftDrawerOpen" side="left" behavior="mobile" elevated>
      <q-list>
        <q-item-label header> Más herramientas </q-item-label>

        <EssentialLink
          v-for="link in essentialLinks"
          :key="link.title"
          v-bind="link" />
      </q-list>
    </q-drawer>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
import { defineComponent, ref } from 'vue';
import EssentialLink from 'components/EssentialLink.vue';

const linksList = [
  {
    title: 'Calculadora',
    icon: 'calculate',
    link: '/',
  },
  {
    title: 'Acerca',
    icon: 'info',
    link: '/about',
  },
  {
    title: 'Github',
    icon: 'code',
    link: 'https://github.com/Arian023/quasar-calculator',
  },
  {
    title: 'Mi portafolio',
    icon: 'next_week',
    link: 'https://arian-acevedo.netlify.app/',
  },
];

export default defineComponent({
  name: 'MainLayout',

  components: {
    EssentialLink,
  },

  setup() {
    const leftDrawerOpen = ref(false);

    return {
      essentialLinks: linksList,
      leftDrawerOpen,
      toggleLeftDrawer() {
        leftDrawerOpen.value = !leftDrawerOpen.value;
      },
    };
  },
});
</script>
