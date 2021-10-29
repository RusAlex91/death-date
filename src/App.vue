<template>
  <div class="">
    <Landing />
  </div>
</template>

<script>
import Landing from './components/Landing.vue'

export default {
  name: 'App',
  components: {
    Landing
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Bad+Script&family=Roboto:ital,wght@0,100;0,300;0,400;0,500;0,700;0,900;1,100;1,300;1,400;1,500;1,700;1,900&display=swap');

* {
  margin: 0;
  padding: 0;
}

#app {
  /* font-family: 'Bad Script', cursive; */
  font-family: 'Roboto', sans-serif;
}
body {
  background: #202024;
}

@media only screen and (max-width: 320px) {
}
</style>
