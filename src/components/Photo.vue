<template>
  <div class="lightbox" @click.self="closeLightbox">
    <img :src="getImgUrl(photo)" />
  </div>
</template>

<script>
const photos = require.context(
  "../assets/images/thumbnails",
  true,
  /^.*\.jpg$/
);

console.log(photos.keys());

export default {
  name: "Photo",
  data() {
    return {
      photos,
    };
  },
  computed: {
    photo() {
      return this.photos.keys().find((photo) => {
        return Number(photo.slice(2, -4)) === Number(this.$route.params.id);
      });
    },
  },
  methods: {
    getImgUrl(filename) {
      return require("../assets/images" + filename.substring(1));
    },
    closeLightbox() {
      this.$router.push("/");
    },
  },
};
</script>

<style>
.lightbox {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.8);
  display: grid;
  grid-template-columns: repeat(3, 1fr);
}

.lightbox img {
  margin: auto;
  width: 100%;
  grid-column-start: 2;
}
</style>
