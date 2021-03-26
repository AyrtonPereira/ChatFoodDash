<template>
  <!-- eslint-disable -->
  <div class="home">
    <Menu />
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import HelloWorld from "@/components/HelloWorld.vue"; // @ is an alias to /src
import Menu from "@/components/Menu/Menu.vue";

@Component({
  components: {
    Menu
  }
})
export default class Home extends Vue {
  
}
/* eslint-disable */

</script>
