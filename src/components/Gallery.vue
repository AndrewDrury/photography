<template>
  <div class="gallery">
    <div
      class="gallery-panel"
      v-for="photo in photos.keys()"
      :key="photo.slice(2, -4)"
    >
      <div class="desktop">
        <router-link :to="`/${photo.slice(2, -4)}`">
          <img class="hidden" :src="getImgUrl(photo)" v-on:load="onLoaded" v-show="loaded" />
        </router-link>
      </div>
      <div class="mobile">
        <img class="hidden" :src="getImgUrl(photo)" v-on:load="onLoaded" v-show="loaded" />
      </div>
    </div>
  </div>
</template>

<script>
const photos = require.context(
  "../assets/images/thumbnails",
  true,
  /^.*\.jpg$/
);

export default {
  name: "Gallery",
  mounted() {
    this.animateElements();
  },
  data() {
    return {
      photos,
      photosLoaded: 0,
      loaded: false,
    };
  },
  methods: {
    onLoaded() {
      this.photosLoaded += 1
      console.log(this.photosLoaded)
      if (this.photosLoaded == this.photos.keys().length * 2) {
        this.loaded = true;
        console.log('DONE')
      }
    },
    getImgUrl(filename) {
      return require("../assets/images/thumbnails" + filename.substring(1));
    },
    animateElements: function () {
      var elements;
      var windowHeight;
      function init() {
        elements = document.querySelectorAll(".hidden");
        windowHeight = window.innerHeight;
        addEventHandlers();
        checkPosition();
      }
      function addEventHandlers() {
        window.addEventListener("scroll", checkPosition);
        window.addEventListener("resize", init);
      }
      function checkPosition() {
        for (var i = 0; i < elements.length; i++) {
          var element = elements[i];
          var positionFromTop = elements[i].getBoundingClientRect().top;
          if (positionFromTop - windowHeight <= 0) {
            element.classList.add("fade-in-element");
            element.classList.remove("hidden");
          }
          if (positionFromTop - windowHeight > 1) {
            //UNCOMMENT to reanimate
            // element.classList.add('hidden');
            // element.classList.remove('fade-in-element');
          }
        }
      }
      window.addEventListener("scroll", checkPosition);
      window.addEventListener("resize", init);
      init();
      checkPosition();
    },
  },
};
</script>

<style>
.desktop {
  display: inline-block;
  overflow: hidden;
}

.mobile {
  display: none;
  margin-bottom: 0.5rem;
}

.gallery {
  line-height: 0;
  -webkit-column-count: 3;
  -webkit-column-gap: 0.5rem;
  -moz-column-count: 3;
  -moz-column-gap: 0.5rem;
  column-count: 3;
  column-gap: 0.5rem;
  padding: 0 5rem;
  margin: 5rem auto;
}

.gallery-panel img {
  width: 100% !important;
  height: auto !important;
  /* margin-bottom: 0.5rem; */
  transition: transform 0.3s linear;
}

.gallery-panel:hover img {
  transform: scale(1.1);
}

@media (max-width: 1200px) {
  .gallery {
    -moz-column-count: 2;
    -webkit-column-count: 2;
    column-count: 2;
    padding: 0 6vw;
  }
}

@media (max-width: 576px) {
  .gallery {
    -moz-column-count: 1;
    -webkit-column-count: 1;
    column-count: 1;
    padding: 0 15px;
  }

  .gallery-panel img {
    margin-bottom: 1rem;
  }

  .gallery-panel:hover img {
    transform: scale(1);
  }

  .desktop {
    display: none;
  }

  .mobile {
    display: inline;
  }
}
</style>
