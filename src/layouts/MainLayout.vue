<template>
  <q-layout view="lHh Lpr lFf">
    <q-header class="bg-white text-grey-10" bordered>
      <q-toolbar class="text-center constrain">
        <q-btn
          class="larg-screen-only q-mr-sm"
          to="/camera"
          icon="eva-camera-outline"
          flat
          round
          size="20px"
          dense
        />
        <q-separator vertical inset spaced class="larg-screen-only" />
        <q-toolbar-title class="text-grand-hotel text-bold">
          Quasagram
        </q-toolbar-title>
        <q-separator vertical inset spaced class="larg-screen-only" />
        <q-btn
          class="larg-screen-only"
          to="/"
          icon="eva-home-outline"
          flat
          round
          size="20px"
          dense
        />
        <!-- <div>Quasar v{{ $q.version }}</div> -->
      </q-toolbar>
    </q-header>

    <q-footer class="bg-white small-screen-only" bordered>
      <!-- <q-toolbar-title></q-toolbar-title> -->
      <q-tabs
        class="text-grey-10"
        active-color="primary"
        indicator-color="transparent"
      >
        <q-route-tab to="/" name="Home" icon="eva-home-outline" />
        <q-route-tab to="/camera" name="Camira" icon="eva-camera-outline" />
      </q-tabs>
    </q-footer>

    <q-page-container class="bg-grey-1">
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
import { defineComponent, ref } from "vue";

export default defineComponent({
  name: "MainLayout",

  setup() {
    const leftDrawerOpen = ref(false);

    return {
      leftDrawerOpen,
      toggleLeftDrawer() {
        leftDrawerOpen.value = !leftDrawerOpen.value;
      },
    };
  },
});
</script>

<style lang="scss">
.q-toolbar {
  @media (min-width: $breakpoint-sm-min) {
    height: 77px;
  }
}
.q-toolbar__title {
  font-size: 30px;
  text-align: left;

  @media (max-width: $breakpoint-xs-max) {
    text-align: center;
  }
}
.q-footer {
  .q-tab__icon {
    font-size: 30px;
  }
}
</style>
