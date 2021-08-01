<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated>
      <q-toolbar>
        <q-btn
          flat
          dense
          round
          icon="menu"
          aria-label="Menu"
          @click="toggleLeftDrawer"
        />
        <q-toolbar-title> Todo </q-toolbar-title>
        <div>{{ todaysDate }}</div>
      </q-toolbar>
    </q-header>

    <q-drawer v-model="leftDrawerOpen" show-if-above bordered class="bg-grey-1">
      <q-list>
        <q-item-label header class="text-grey-8">
          Essential Links
        </q-item-label>

        <Sidebar v-for="link in Sidebars" :key="link.title" v-bind="link" />
      </q-list>
    </q-drawer>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
import Sidebar from "components/Sidebar.vue";
import { date } from "quasar";

const linksList = [
  {
    title: "Todo",
    // caption: "quasar.dev",
    icon: "list",
    link: "/",
  },
  {
    title: "Github",
    caption: "github.com/quasarframework",
    icon: "code",
    link: "https://github.com/quasarframework",
  },
];

import { defineComponent, ref } from "vue";

export default defineComponent({
  name: "MainLayout",

  components: {
    Sidebar,
  },
  computed: {
    todaysDate() {
      const timeStamp = Date.now();
      return date.formatDate(timeStamp, "ddd, DD-MMM-YYYY HH:mm");
    },
  },
  setup() {
    const leftDrawerOpen = ref(false);

    return {
      Sidebars: linksList,
      leftDrawerOpen,
      toggleLeftDrawer() {
        leftDrawerOpen.value = !leftDrawerOpen.value;
      },
    };
  },
});
</script>
