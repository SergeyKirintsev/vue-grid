<script setup>
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://vuejs.org/api/sfc-script-setup.html#script-setup
// import HelloWorld from "./components/HelloWorld.vue";
import TableJson from "./components/TableJson.vue";

import { arr } from './json/data';

function test(arr, obj, level = 1, path = []) {
  if (!Array.isArray(arr)) {
    return;
  }
  arr.forEach((el) => {
    const { id, name } = el;
    const elObj = {
      id,
      name,
    }

    const newPath = [...path, elObj]

    if (obj[level]) {
      obj[level].push({ ...elObj, newPath })
    } else {
      obj[level] = [{ ...elObj, newPath }];
    }

    const children = el.children;
    const nextLevel = level + 1;

    test(children, obj, nextLevel, newPath);
  });
}

const obj = {}
test(arr, obj);

const level_5 = obj[5].map(el => el.newPath)
console.log(level_5);

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
  <TableJson 
    :headers="headers"
    :levelAll="level_5"
   />
</template>
