<template >
  <div id="app" >
    <b-container  >
      <section class="search-section">
        <h1 class="search-section__title">Search</h1>
        <p class="search-section__paragraph">all what you need</p>

        <label class="search-section__search-lable">
          <b-form-input
            v-model="searchInput"
            placeholder="Enter your request"
            @keydown.enter="serarchRequest(1)"
            class="m-1"
          >
          </b-form-input>
          <b-button
            v-on:click.stop="serarchRequest(1)"
            variant="outline-primary"
            class="m-1"
            >Search
          </b-button>
        </label>
      </section>

      <div v-if="searchResponse" class="search-section__search-result">
        <div
          class="card search-section__card"
          style="width: 95%; max-width: 70rem"
          v-for="item in searchResponse.objects"
          :key="item.id"
          @click="selectedItem = item, scrollHid()"
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
            <li class="list-group-item" >
              <span class="card__list-title">date:</span>
              <span v-if="item.package.date"> {{ item.package.date }} </span>
            </li>
            <li  class="list-group-item" style="color: royalblue">  click to read more  </li>
          </ul>
        </div>
        <div v-if="searchResponse.total === 0">Нічого не знайдено</div>

        <PopupWindow
          v-if="selectedItem"
          :selectedItem="selectedItem"
          @close="selectedItem = undefined"
          @click="log(this.selectedItem)"
        />
        <b-pagination
          v-model="currentPage"
          :total-rows="searchResponse.total"
          :per-page="searchResultsPerPage"
          first-number
          last-number
          @page-click="(_0, page) => serarchRequest(page)"
        ></b-pagination>
      </div>
    </b-container>
    <FooterSection/>
  </div>
    
</template>

<script>
import PopupWindow from "./components/PopupWindow.vue";
import FooterSection from "./components/FooterSection.vue";
const apiUrl = "https://registry.npmjs.com/-/v1/";

export default {
  name: "App",
  components: {
    PopupWindow,
    FooterSection,
  },
  data() {
    return {
      searchInput: "",
      selectedItem: undefined,
      searchResponse: undefined,
      perPage: 1,
      currentPage: undefined,
      searchResultsPerPage: 10,
    };
  },
  computed: {
  },
  methods: {
    async serarchRequest(page) {
      const startFrom = (page - 1) * this.searchResultsPerPage;
      const searchParams = new URLSearchParams({
        text: this.searchInput,
        size: this.searchResultsPerPage,
        from: startFrom,
      });
      this.searchResponse = await (
        await fetch(
          `${apiUrl}search?${searchParams.toString()}`
        )
      ).json();
    },
    log(...itm) {
      console.log(...itm);
    },
    scrollHid(){
      document.body.classList.add('overflowHidden')
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
  padding-bottom: 5vh;

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
.overflowHidden {overflow: hidden;}
</style>
