<template>
  <div class="gallery">
    <div
      class="gallery-panel"
      v-for="photo in photos.keys()"
      :key="photo.slice(2, -4)"
    >
      <div class="desktop hidden">
        <router-link :to="`/${photo.slice(2, -4)}`">
          <img :src="getImgUrl(photo)" />
        </router-link>
      </div>
      <div class="mobile hidden">
        <img :src="getImgUrl(photo)" />
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
    };
  },
  methods: {
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
  margin-bottom: 0.5rem;
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

.fade-in-element {
  opacity: 0;
  -webkit-animation: reveal 1s cubic-bezier(0, 1, 0.5, 1) 1 normal forwards;
  -moz-animation: reveal 1s ease-in 1 normal forwards;
  -o-animation: reveal 1s cubic-bezier(0, 1, 0.5, 1) 1 normal forwards;
  animation: reveal 1s cubic-bezier(0, 1, 0.5, 1) 1 normal forwards;
}

.hidden {
  opacity: 0;
}

@-webkit-keyframes reveal {
  0% {
    opacity: 0;
    -webkit-transform: translateY(100%);
    -moz-transform: translateY(100%);
    -o-transform: translateY(100%);
    transform: translateY(100%);
  }
  100% {
    opacity: 1;
    -webkit-transform: translateY(0);
    -moz-transform: translateY(0);
    -o-transform: translateY(0);
    transform: translateY(0);
  }
}

@-moz-keyframes reveal {
  0% {
    opacity: 0;
    -webkit-transform: translateY(100%);
    -moz-transform: translateY(100%);
    -o-transform: translateY(100%);
    transform: translateY(100%);
  }
  100% {
    opacity: 1;
    -webkit-transform: translateY(0);
    -moz-transform: translateY(0);
    -o-transform: translateY(0);
    transform: translateY(0);
  }
}

@-o-keyframes reveal {
  0% {
    opacity: 0;
    -webkit-transform: translateY(100%);
    -moz-transform: translateY(100%);
    -o-transform: translateY(100%);
    transform: translateY(100%);
  }
  100% {
    opacity: 1;
    -webkit-transform: translateY(0);
    -moz-transform: translateY(0);
    -o-transform: translateY(0);
    transform: translateY(0);
  }
}

@keyframes reveal {
  0% {
    opacity: 0;
    -webkit-transform: translateY(100%);
    -moz-transform: translateY(100%);
    -o-transform: translateY(100%);
    transform: translateY(100%);
  }
  100% {
    opacity: 1;
    -webkit-transform: translateY(0);
    -moz-transform: translateY(0);
    -o-transform: translateY(0);
    transform: translateY(0);
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

  .desktop {
    display: none;
  }

  .mobile {
    display: inline;
  }
}
</style>
