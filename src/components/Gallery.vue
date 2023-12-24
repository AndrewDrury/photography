<template>
  <div>
    <div class="loading" v-show="loading">
      <svg width="100" height="100" viewBox="0 0 100 100">
        <polyline
          class="line-cornered stroke-still"
          points="0,0 100,0 100,100"
          stroke-width="10"
          fill="none"
        ></polyline>
        <polyline
          class="line-cornered stroke-still"
          points="0,0 0,100 100,100"
          stroke-width="10"
          fill="none"
        ></polyline>
        <polyline
          class="line-cornered stroke-animation"
          points="0,0 100,0 100,100"
          stroke-width="10"
          fill="none"
        ></polyline>
        <polyline
          class="line-cornered stroke-animation"
          points="0,0 0,100 100,100"
          stroke-width="10"
          fill="none"
        ></polyline>
      </svg>
    </div>
    <div class="gallery">
      <div
        class="gallery-panel"
        v-for="photo in photos.keys().reverse()"
        :key="photo.slice(2, -4)"
      >
        <div class="desktop">
          <!-- <router-link :to="`/${photo.slice(2, -4)}`"> -->
          <img
            class="hidden"
            :src="getImgUrl(photo)"
            v-on:load="onLoaded"
            v-show="loaded"
          />
          <!-- </router-link> -->
        </div>
        <div class="mobile">
          <img
            class="hidden"
            :src="getImgUrl(photo)"
            v-on:load="onLoaded"
            v-show="loaded"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
const photos = require.context("../assets/images", true, /^.*\.jpg$/);

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
      loading: true,
    };
  },
  methods: {
    onLoaded() {
      this.photosLoaded += 1;
      console.log(this.photosLoaded);
      if (this.photosLoaded == this.photos.keys().length * 2) {
        this.loaded = true;
        this.loading = false;
      }
    },
    getImgUrl(filename) {
      return require("../assets/images" + filename.substring(1));
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
  margin: 115px auto 5rem auto;
}

.gallery-panel div img {
  width: 100% !important;
  height: auto !important;
  /* margin-bottom: 0.5rem; */
  transition: transform 0.3s linear;
}

.gallery-panel:hover img {
  transform: scale(1.1);
}

.loading {
  display: flex;
  justify-content: center;
  padding-top: 60px;
}

svg {
  -webkit-transform: rotate(45deg) scale(1);
  transform: rotate(45deg) scale(1);
}

.stroke-still {
  stroke: #232323;
}

.stroke-animation {
  -webkit-animation: stroke-spacing 1.2s ease-in, stroke-color 4.8s linear;
  animation: stroke-spacing 1.2s ease-in, stroke-color 4.8s linear;
  -webkit-animation-iteration-count: infinite;
  animation-iteration-count: infinite;
  -webkit-animation-delay: 0;
  animation-delay: 0;
  -webkit-animation-direction: normal;
  animation-direction: normal;
  -webkit-animation-fill-mode: forwards;
  animation-fill-mode: forwards;
  -webkit-animation-play-state: running;
  animation-play-state: running;
  -webkit-transform-origin: center center;
  transform-origin: center center;
}

@-webkit-keyframes stroke-spacing {
  0% {
    stroke-dasharray: 0 200;
  }
  45% {
    stroke-dashoffset: 0;
    stroke-dasharray: 200 200;
  }
  90% {
    stroke-dashoffset: -200;
    stroke-dasharray: 200 200;
  }
  100% {
    stroke-dashoffset: -200;
    stroke-dasharray: 200 200;
  }
}

@keyframes stroke-spacing {
  0% {
    stroke-dasharray: 0 200;
  }
  45% {
    stroke-dashoffset: 0;
    stroke-dasharray: 200 200;
  }
  90% {
    stroke-dashoffset: -200;
    stroke-dasharray: 200 200;
  }
  100% {
    stroke-dashoffset: -200;
    stroke-dasharray: 200 200;
  }
}

@-webkit-keyframes stroke-color {
  0% {
    stroke: #3498db;
  }
  24% {
    stroke: #643232;
  }
  25% {
    stroke: #327864;
  }
  49% {
    stroke: #327864;
  }
  50% {
    stroke: #32326e;
  }
  74% {
    stroke: #32326e;
  }
  75% {
    stroke: #78325a;
  }
  99% {
    stroke: #78325a;
  }
}

@keyframes stroke-color {
  0% {
    stroke: #3498db;
  }
  24% {
    stroke: #643232;
  }
  25% {
    stroke: #327864;
  }
  49% {
    stroke: #327864;
  }
  50% {
    stroke: #32326e;
  }
  74% {
    stroke: #32326e;
  }
  75% {
    stroke: #78325a;
  }
  99% {
    stroke: #78325a;
  }
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
