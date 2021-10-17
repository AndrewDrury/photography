<template>
  <Header />
  <Gallery />
</template>

<script>
import Gallery from "@/components/Gallery.vue";
import Header from "@/components/Header.vue";

export default {
  name: "home",
  components: {
    Gallery,
    Header,
  },
};
</script>
