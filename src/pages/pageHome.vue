<template>
  <q-page class="constrain q-pa-md">
    <div class="row q-col-gutter-lg">
      <div class="col-12 col-sm-8"><HomePosts /></div>
      <div class="col-4 larg-screen-only">
        <!-- defult width is 12 and after small width will be 8
      it is throw from minmum size to upper size    -->
        <q-item class="fixed">
          <q-item-section avatar>
            <q-avatar size="48px">
              <img src="https://cdn.quasar.dev/img/boy-avatar.png" />
            </q-avatar>
          </q-item-section>

          <q-item-section>
            <q-item-label class="text-bold">danny__connel</q-item-label>
            <q-item-label caption>san Francisco ,United State</q-item-label>
          </q-item-section>
        </q-item>
      </div>
    </div>
    <!-- <h2>Page Home</h2> -->
  </q-page>
</template>

<script>
import { defineComponent } from "vue";
import HomePosts from "./homePosts.vue";

export default defineComponent({
  name: "pageHome",
  components: { HomePosts },
});
</script>
