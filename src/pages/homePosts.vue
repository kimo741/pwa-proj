<template>
  <div>
    <q-card
      class="card-posts"
      flat
      bordered
      v-for="post in posts"
      :key="post.id"
    >
      <q-item>
        <q-item-section avatar>
          <q-avatar>
            <img src="https://cdn.quasar.dev/img/boy-avatar.png" />
          </q-avatar>
        </q-item-section>

        <q-item-section>
          <q-item-label class="text-bold">danny__connel</q-item-label>
          <q-item-label caption> {{ post.location }}</q-item-label>
        </q-item-section>
      </q-item>

      <q-separator />
      <q-img class="col" :src="post.imgURL" />
      <q-card-section>
        <div>{{ post.caption }}</div>
        <div class="text-caption text-grey">
          {{ Date(post.dateC) }}
        </div>
      </q-card-section>
    </q-card>
  </div>
</template>
<script>
import { date } from "quasar";
export default {
  name: "homePosts",
  data() {
    return {
      posts: [
        {
          id: 1,
          caption: "Golden Gate Bridge",
          dateC: 1651606693866,
          location: "san Francisco ,United State",
          imgURL: "https://cdn.quasar.dev/img/parallax2.jpg",
        },
        {
          id: 2,
          caption: "Golden Gate Bridge",
          dateC: 1651606693866,
          location: "san Francisco ,United State",
          imgURL: "https://cdn.quasar.dev/img/parallax2.jpg",
        },
        {
          id: 3,
          caption: "Golden Gate Bridge",
          dateC: 1651606693866,
          location: "san Francisco ,United State",
          imgURL: "https://cdn.quasar.dev/img/parallax2.jpg",
        },
        {
          id: 3,
          caption: "Golden Gate Bridge",
          dateC: 1651606693866,
          location: "san Francisco ,United State",
          imgURL: "https://cdn.quasar.dev/img/parallax2.jpg",
        },
      ],
    };
  },
  filters: {
    niceDate(value) {
      // return date.formatDate(value, "YYYY-MM-DDTHH:mm:ss.SSSZ");
      return date.formatDate(value, "YYYY-MM-DDTHH:mm:ss.SSSZ");
    },
  },
};
</script>
<style lang="scss">
.card-posts {
  .q-img {
    min-height: 200px;
  }
}
</style>
