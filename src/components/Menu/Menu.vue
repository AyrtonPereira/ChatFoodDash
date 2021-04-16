<template>
  <div class="menu">
    <div class="return">
      <img
        class="return-arrow"
        src="../../assets/arrow.png"
        v-on:click="limpaCache()"
      />
    </div>
    <div class="search-box">
      <h2>Search</h2>
      <input
        class="input-search"
        type="text"
        v-model="search"
        placeholder="Search for dishes..."
      />
    </div>
    <Category
      v-for="category in menu.categories"
      :key="category.id"
      :categories="category"
      v-on:updateCart="updateCart($event)"
    />
  </div>
</template>

<script lang="ts">
import { Component, Vue, Watch } from "vue-property-decorator";
import axios from "axios";
import Category from "@/components/Menu/Category.vue";
import Vuex from "vuex";

@Component({
  components: {
    Category,
  },
})
export default class Menu extends Vue {
  /*eslint-disable */
  menu: object = {};
  search = "";
  listRecharge = false;

  async getDishes(): Promise<void> {
    const result = await axios.get(
      "https://chatfood-cdn.s3.eu-central-1.amazonaws.com/fe-code-challenge-1/menu.json"
    );
    let menu = result.data;
    let list: any = menu.items;
    menu.categories.forEach(function(v: object, i: number, a: any, b: any) {
      console.log(v, a, b);
      a[i].items = list.filter(function(item) {
        if (item.category_id == i) {
          item.selected = 0;
          return true;
        } else {
          return false;
        }
      });
    });
    this.store.commit("setMenu", menu);
  }

  async loadCart(): Promise<void> {
    await this.getDishes();
    localStorage.dishList = JSON.stringify(this.store.getters.getMenu);
  }

  updateCart(dish: any): void {
    const dishId: number = dish.id;
    const categoryId: number = dish.category;
    const list = this.store.getters.getMenu;
    list.categories[categoryId].items.find(function(
      v: any,
      i: number,
      a: any
    ) {
      if (v.id == dishId) {
        if (v.stock.availability > 0) {
          a[i].selected++;
          v.stock.availability--;
          return true;
        } else {
          alert("Quantidade máxima para o produto selecionada");
        }
      }
    });
    if (this.menu.categories[0].id == 0) {
      this.menu.categories[0].items.find(function(v: JSON, i: number, a: any) {
        if (v.id == dishId) {
          a[i].selected++;
        }
      });
    }
    this.store.commit("updateMenu", list);
  }

  @Watch("search")
  filterMenu(): void {
    let text = new RegExp(this.search, "i");
    if (this.search.length > 2) {
      let filter = {
        categories: [{ name: "Search Results", items: [], id: 0 }],
      };
      const dishMenu = JSON.parse(localStorage.dishList);
      filter.categories[0].items = dishMenu.items.filter(function(v: object) {
        return v.name.match(text);
      });
      this.menu = filter;
      this.listRecharge = true;
    } else if (this.search.length <= 2 && this.listRecharge == true) {
      this.menu = this.store.getters.getMenu;
      this.listRecharge = false;
    }
  }

  async limpaCache(): Promise<void> {
    delete localStorage.dishList;
    await this.loadCart();
    this.menu = {};
    this.search = "";
    this.menu = this.store.getters.getMenu;
  }

  store = new Vuex.Store({
    state: {
      dishList: {},
    },
    mutations: {
      setMenu(state, payload) {
        state.dishList = payload;
      },
      updateMenu(state, payload) {
        state.dishList = payload;
        localStorage.dishList = JSON.stringify(payload);
      },
    },
    getters: {
      getMenu: (state) => {
        return state.dishList;
      },
    },
  });

  mounted() {
    if (!localStorage.dishList) {
      this.loadCart();
    } else {
      this.store.commit("setMenu", JSON.parse(localStorage.dishList));
    }
    this.menu = this.store.getters.getMenu;
  }
}
</script>

<style lang="scss">
.menu {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  max-width: 375px;
  background-color: #ffffff;

  .return {
    align-self: flex-start;
    margin-left: 14px;

    .return-arrow {
      cursor: pointer;
    }
  }

  .search-box {
    text-align: left;

    h2 { 
      color: #050505;
    }

    .input-search {
      text-align: left;
      height: 50px;
      border: 2px solid #ebeff4;
      min-width: 333px;
      border-radius: 7px;
      font-size: 14px;
      color: #838da6;
    }
  }
}
</style>
