<template>
  <!-- eslint-disable -->
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png" />
    <Menu :dishList="dishes" />
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import HelloWorld from "@/components/HelloWorld.vue"; // @ is an alias to /src
import Menu from "@/components/Menu/Menu.vue";

@Component({
  components: {
    HelloWorld,
    Menu
  }
})
export default class Home extends Vue {
  
}
/* eslint-disable */

</script>
