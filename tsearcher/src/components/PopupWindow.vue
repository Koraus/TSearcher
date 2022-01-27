<template  >
  <div class="bg-drop" @click.stop="close()">
    <div
      class="card text-white bg-light  base-info"
      style="max-width: 60%"
      v-if="selectedItem"
    >
      <div class="card-header"><h3 class="card__title">About</h3></div>
      <div class="card-body">
        <p style="color: red">Name: {{ selectedItem.package.name }}</p>
        <p style="color: red">
          Description: {{ selectedItem.package.description }}
        </p>
        <ul class="card__links-list">
          <li
            v-for="(value, name) in selectedItem.package.links"
            :key="name.id"
          >
            {{ name }}: <a target="_blank" :href="value"> {{ value }} </a>
          </li>
        </ul>
      </div>
    </div>
    <div class="card text-white bg-light base-info" style="max-width: 60%">
      <div class="card-header"><h3 class="card__title"> More info about versions <span  v-if="packageInfo"> {{this.packageInfo.versions[0]}}  </span>  </h3></div>
      <div class="card-body text-success">
        <p class="card-text" v-if="packageInfo"> </p>
        <ul class="card__list">
          File list:
        <li v-for="item in this.packageVersionInfo.files" :key="item.id"> {{item.name}} </li>
        </ul>
        <h5 v-if="!packageInfo" class="card-title"> loading...</h5>
        
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "PopupWindow",

  created() {
    this.getInfo();
  },

  data() {
    return {
      packageInfo: "",
      packageVersionInfo: "",
    };
  },
  props: {
    item: Object,
    selectedItem: Object,
  },
  emits: {
    close: null,
  },

  methods: {
    close() {
      this.$emit("close");
    },
    async getInfo() {
      this.packageInfo = await (
        await fetch(
          `https://data.jsdelivr.com/v1/package/npm/${this.selectedItem.package.name}`
        )
      ).json();
      this.packageVersionInfo = await (
        await fetch(
          `https://data.jsdelivr.com/v1/package/npm/${this.selectedItem.package.name}@${this.packageInfo.versions[0]}`
        )
      ).json();
      
      
    },
    
  },
};
</script>

<style >
.bg-drop {
  background: rgba(0, 0, 0, 0.452);
  min-width: 100%;
  min-height: 100%;
  position: fixed;
  top: 0;
  left: 0;
  color: blue;
  font-size: 18px;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 40px;
}
.base-info {
  background: white;
  width: 70%;
  height: 60%;
  color: blue;
}
.jsdelivr-info {
  background: rgb(48, 197, 140);
}
.card__links-list {
  color: blue;
 
}
.card__list{
  
}
.card__list > li{
 text-align: left;
}


.card__links-list > li{
 text-align: left;
}
.card__title{
  color: royalblue
}
</style>