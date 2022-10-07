<template>
  <div
    v-for="(item, idx) in items"
    :class="['header', `header-${idx + 1}`]"
    :style="{ gridArea: `header-${idx + 1}` }"
  >
    {{ item }}
  </div>
</template>

<script>
export default {
  props: {
    items: Array,
  },
};
</script>
