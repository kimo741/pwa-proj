<template>
  <q-page class="constrain-more q-pa-md">
    <div class="camera-frame q-pa-md">
      <video v-show="!imageCapture" autoplay ref="video" class="full-width" />
      <canvas
        v-show="imageCapture"
        ref="canvas"
        height="200"
        class="full-width"
      />
    </div>
    <div class="text-center q-pa-md">
      <q-btn
        v-if="cameraSupport"
        @click="captureImage"
        round
        color="grey-10"
        icon="eva-camera"
        size="lg"
      />
      <q-file
        outlined
        v-else
        @input="captureImagefallback"
        label="Choose an image"
        accept="image/*"
      >
        <template v-slot:prepend>
          <q-icon name="eva-attach-outline" />
        </template>
      </q-file>
    </div>
    <div class="row justify-center q-ma-md">
      <q-input
        v-model="post.caption"
        class="col col-sm-8"
        label="Caption"
        dense
      />
    </div>
    <div class="row justify-center q-ma-md">
      <q-input
        v-model="post.location"
        class="col col-sm-8"
        label="Location"
        dense
      >
        <template v-slot:append>
          <q-btn
            round
            @click="getLocation"
            dense
            flat
            icon="eva-navigation-2-outline"
          />
        </template>
      </q-input>
    </div>
    <div class="row justify-center q-mt-lg">
      <q-btn unelevated rounded color="primary" label="Post Image" />
    </div>
  </q-page>
</template>

<script>
import axios from "axios";
import { uid } from "quasar";
import { timeouts } from "retry";
import { defineComponent } from "vue";
import { useQuasar } from "quasar";

require("md-gum-polyfill"); // prifexes //

export default defineComponent({
  name: "PageCamira",
  data() {
    return {
      post: {
        id: uid(),
        caption: "",
        location: "",
        photo: "",
        date: Date.now(),
      },
      imageCapture: false,
      cameraSupport: true,
      imageUpload: "",
    };
  },
  methods: {
    initcamera() {
      // display media scrren in video src
      navigator.mediaDevices
        .getDisplayMedia({
          video: true,
        })
        .then((stream) => {
          this.$refs.video.srcObject = stream;
        })
        .catch((err) => {
          this.cameraSupport = false;
        });
    },
    captureImage() {
      let video = this.$refs.video;
      let canvas = this.$refs.canvas;
      canvas.width = video.getBoundingClientRect().width;
      canvas.height = video.getBoundingClientRect().height;
      let context = canvas.getContext("2d");
      context.drawImage(video, 0, 0, canvas.width, canvas.height);
      this.imageCapture = true;
      this.post.photo = this.dataURItoBlob(canvas.toDataURL());
      this.disableCamera();
    },
    captureImagefallback(file) {
      let canvas = this.$refs.canvas;
      let context = canvas.getContext("2d");
      // this.post.photo = file;
      let reader = new FileReader();
      var img = new Image();
      reader.onload = (event) => {
        img.onload = () => {
          canvas.width = img.width;
          canvas.height = img.height;
          this.imageCapture = true;
          context.drawImage(img, 0, 0);
        };
        img.src = event.target.result;
      };
      reader.readAsDataURL(file.target.files[0]);
    },

    dataURItoBlob(dataURI) {
      // convert base64 to raw binary data held in a string
      // doesn't handle URLEncoded DataURIs - see SO answer #6850276 for code that does this
      var byteString = atob(dataURI.split(",")[1]);

      // separate out the mime component
      var mimeString = dataURI.split(",")[0].split(":")[1].split(";")[0];

      // write the bytes of the string to an ArrayBuffer
      var ab = new ArrayBuffer(byteString.length);

      // create a view into the buffer
      var ia = new Uint8Array(ab);

      // set the bytes of the buffer to the correct values
      for (var i = 0; i < byteString.length; i++) {
        ia[i] = byteString.charCodeAt(i);
      }

      // write the ArrayBuffer to a blob, and you're done
      var blob = new Blob([ab], { type: mimeString });
      return blob;
    },
    disableCamera() {
      this.$refs.video.srcObject.getVideoTracks().forEach((track) => {
        track.stop();
      });
    },
    getLocation() {
      navigator.geolocation.getCurrentPosition(
        (position) => {
          this.getCityAndCuntry(position);
        },
        (err) => {
          this.locationError();
          console.log("catch 1 run");
        },
        { timeouts: 500 }
      );
    },
    getCityAndCuntry(possetion) {
      let apiUrl = `https://geocode.xyz/${possetion.coords.latitude},${possetion.coords.longitude}?json=1`;
      this.$axios
        .get(apiUrl)
        .then((res) => {
          return console.log(res);
          // console.log(res);
          // this.locationSuccess(res);
        })
        .catch((err) => {
          this.locationError(err);
          console.log(err);
        });
    },
    locationSuccess(resp) {
      this.post.location = resp.data.city;
      if (resp.data.country) {
        this.post.location += `, ${resp.data.country}`;
      }
    },
    locationError() {
      this.$q.dialog({
        title: "Error",
        message: "Cloud not find location",
      });
      console.log(this.$q);
    },
  },
  beforeUnmount() {
    if (this.cameraSupport) {
      this.disableCamera();
    }
  },
  mounted() {
    this.initcamera();
  },
});
</script>
<style lang="scss">
.camera-frame {
  border: 2px solid $grey-10;
  border-radius: 10px;
}
</style>
