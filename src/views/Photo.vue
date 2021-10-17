<template>
  <Header />
  <Gallery />
  <Photo />
</template>

<script>
import Photo from "@/components/Photo.vue";
import Gallery from "@/components/Gallery.vue";
import Header from "@/components/Header.vue";

export default {
  name: "photo",
  components: {
    Photo,
    Gallery,
    Header,
  },
};
</script>
