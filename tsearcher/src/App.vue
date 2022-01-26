<template>
  <div id="app">
    <section class="search-section">
      <h1 class="search-section__title">Search</h1>
      <p class="search-section__paragraph">all what you need</p>
      <label class="search-section__search-lable">
        <input
          @keydown.enter="serarchRequest()"
          type="text"
          v-model="searchInput"
          class="search-section__input"
          placeholder="Enter your request"
        />
        <button
          v-on:click.stop="serarchRequest()"
          class="search-section__search-btn"
        >
          Search
        </button>
        {{ searchInput }}
      </label>
    </section>
    <ul v-if="searchResponse != undefined">
      <p v-if="searchResponse.objects.length === 0">нічого не знайдено</p>
      <li
        v-for="item in searchResponse.objects"
        :key="item.id"
        @click="(selectedItem = item ), OpenPopUp()"
       
      >
        {{ item.searchScore }}
        <p v-if="item.package.author" style="display: inline-block">
          {{ item.package.author.name }}
        </p>
                <p v-if="item.package.author" style="display: inline-block"> 
         name: {{ item.package.name }}
        </p>
      </li>
      <PopupWindow
      :isOpen="isOpen"
      :selectedItem="selectedItem"
       @close="ClosePopUp()"
    />
    </ul>

  </div>
</template>

<script>
import PopupWindow from "./components/PopupWindow.vue";

const apiUrl = "https://registry.npmjs.com/-/v1/";

export default {
  name: "App",
  components: {
    PopupWindow,
  },
  data() {
    return {
      searchInput: "",
      selectedItem: undefined,
      searchResponse: undefined,
      isOpen: false,
    };
    
  },
  computed: {},
  methods: {
    async serarchRequest() {
      const qResultSize = 10;
      this.searchResponse = await (
        await fetch(
          `${apiUrl}search?text=${this.searchInput}&size=${qResultSize}`
        )
      ).json();
      console.log(this.searchResponse);
    },
    OpenPopUp () {
      this.isOpen = true; 
    },
    ClosePopUp(){
      this.isOpen = false; 
    },

    log(itm) {
      console.log(itm);
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
