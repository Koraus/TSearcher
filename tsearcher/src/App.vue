<template>
  <div id="app">
    <b-container>
      <section class="search-section">
        <h1 class="search-section__title">Search</h1>
        <p class="search-section__paragraph">all what you need</p>

        <label class="search-section__search-lable">
          <b-form-input
            v-model="searchInput"
            placeholder="Enter your request"
            @keydown.enter="serarchRequest()"
            class="m-1"
          >
          </b-form-input>
          <b-button
            v-on:click.stop="serarchRequest()"
            variant="outline-primary"
            class="m-1"
            >Search
          </b-button>
        </label>
      </section>

      <div v-if="searchResponse" class="search-section__search-result">
        <div
          class="card search-section__card"
          style="width: 100%; max-width: 70rem"
          v-for="item in searchResponse.objects"
          :key="item.id"
          @click="selectedItem = item"
        >
          <div class="card-header">
            <h4>{{ item.package.name }}</h4>
          </div>
          <ul class="list-group list-group-flush">
            <li class="list-group-item">
              <span v-if="item.package.author">
                <span class="card__list-title">author name:</span>
                {{ item.package.author.name }}
              </span>
              <span v-else> no author name </span>
            </li>
            <li class="list-group-item">
              <span class="card__list-title">description:</span>
              <span v-if="item.package.description">
                {{ item.package.description }}
              </span>
            </li>
            <li class="list-group-item">
              <span class="card__list-title">date:</span>
              <span v-if="item.package.date"> {{ item.package.date }} </span>
            </li>
          </ul>

        </div>
        <div v-if="searchResponse.total === 0">Нічого не знайдено</div>

          <PopupWindow
            v-if="selectedItem"
            :selectedItem="selectedItem"
            @close="selectedItem = undefined"
            @click="log(this.selectedItem)"
          />

      </div>
    </b-container>
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
      totalPages: undefined,
      currentPage: 1,
    
    };
  },
  computed: {
    
  },
  methods: {
    async serarchRequest() {
      const qResultSize = 10;
      let startFrom = 100;
      this.searchResponse = await (
        await fetch(
          `${apiUrl}search?text=${this.searchInput}&size=${qResultSize}&from=${startFrom}`
        )
      ).json();
      this.totalPages = Math.ceil(this.searchResponse.total / qResultSize);
      this.currentPage = Math.ceil(startFrom / qResultSize);
      console.log(this.searchResponse);
      console.log(this.totalPages);

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

.search-section__search-lable {
  display: flex;
  width: 70%;
  margin-bottom: 20px;
}
.search-section {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  margin-bottom: 20px;
}

.search-section__item {
  max-width: 800px;
  min-width: 200px;
  padding: 20px 0 20px 0;
  margin: 10px;
  border-top-width: auto;
}
.search-section__search-result {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  flex-wrap: wrap;
}
.search-section__card {
  margin: 10px;
}

.card__list-title {
  font-size: 14px;
  color: #2c3e50;
}
</style>
