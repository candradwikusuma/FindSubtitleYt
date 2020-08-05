<template>
  <div :is="tag" :name="name" class="wrapper">
    <slot></slot>
  </div>
</template>
<script>
export default {
  props: {
    tag: {
      type: String,
      default: "div",
    },
    name: {
      type: String,
      default: "",
    },
  },
};
</script>