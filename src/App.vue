<script setup>
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://vuejs.org/api/sfc-script-setup.html#script-setup
// import HelloWorld from "./components/HelloWorld.vue";
import TableJson from "./components/TableJson.vue";

import json from "./json/data.json";

const headers = [
  "Класс документации",
  "Тип документации",
  "Язык",
  "Вид документации",
  "Документы",
];
</script>

<template>
  <!-- <HelloWorld msg="Vite + Vue" /> -->
  <TableJson :obj="json" :headers="headers" />
</template>
