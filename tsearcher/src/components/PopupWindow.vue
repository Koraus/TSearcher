<template  >
  <div v-if="isOpen" class="bg-drop" @click.stop="close()">
    <div v-if="selectedItem" class="base-info">
      <p style="color: red">Name: {{ selectedItem.package.name }}</p>
      <p style="color: red">Name: {{ selectedItem.package.description }}</p>
      <ul>
        <li v-for="(value, name) in selectedItem.package.links" :key="name.id">
          {{ name }}: {{ value }}
        </li>
      </ul>
    </div>
    <div v-if="!packageInfo" class="jsdelivr-info">loading</div>
    <div v-if="packageInfo" class="jsdelivr-info">
      <p v-if="isOpen">Last V: {{this.packageInfo.versions[0]}}</p>
      <p v-if="isOpen"> {{this.packageVersionInfo}} </p>
    </div>
  </div>
</template>

<script>
export default {
  name: "PopupWindow",

  beforeUpdate() {
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
    isOpen: {
      type: Boolean,
      required: true,
    },
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
  position: absolute;
  top: 0;
  color: blue;
  font-size: 18px;
}
.base-info {
  background: white;
  width: 70%;
  height: 60%;
}
.jsdelivr-info {
  background: rgb(48, 197, 140);
}
</style>