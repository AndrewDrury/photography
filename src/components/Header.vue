<template>
  <div class="header row hidden" align="left">
    <div class="col">
      <a href="https://www.instagram.com/andrew_drury" target="_blank">
        <i class="fa fa-instagram"></i>
        @andrew_drury
      </a>
    </div>
    <div class="col">
      <a href="mailto:hello@andrew-drury.com">
        <i class="fa fa-envelope"></i>
        hello@andrew-drury.com
      </a>
    </div>
  </div>
</template>

<script>
export default {
  name: "Header",
};
</script>

<style lang="scss">
@import url("https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css");

.col {
  margin-block: 5px;
}

.header {
  padding: 0 5rem;
  margin-top: 38px;
}

a {
  text-decoration: none;
}

a:hover {
  text-decoration: underline;
}

@media (max-width: 1200px) {
  .header {
    padding: 0 6vw;
  }
}

@media (max-width: 576px) {
  .header {
    -moz-column-count: 1;
    -webkit-column-count: 1;
    column-count: 1;
    padding: 0 15px;
  }
}
</style>
