<template >
<footer class="footer">
<span>{{ name }} &nbsp; </span>  <a :href="gitLink">{{ gitLink }}</a>
</footer>
</template>

<script>
export default {
  name: "FooterSection",
  data() {
    return {
      gitLink: "https://github.com/Koraus",
      name: "Dmytro Kashchynets",
    };
  },
};
</script>

<style >
.footer {
    display: flex;
   justify-content: center;
   align-items: center;
width: 100%;
min-height: 5vh;
background: rgb(215, 215, 245);
position: fixed;
bottom: 0;
}
</style>