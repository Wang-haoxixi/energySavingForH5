<template>
  <div class="jodit_wysiwyg-wrapper" v-html="value" />
  <!-- <jodit-vue class="jodit_wysiwyg-wrapper" :value="value" :config="config" /> -->
</template>

<script>
// import JoditVue from "./JoditVue";

export default {
  name: "IepJoditRead",

  // components: { JoditVue },
  props: {
    value: {
      type: String,
      required: false,
    },
  },
  // data() {
  //   return {
  //     config: {
  //       readonly: true,
  //       preset: "inline",
  //     },
  //   };
  // },
};
</script>
<style lang="scss" scoped>
.jodit_wysiwyg-wrapper ::v-deep img {
  max-width: 100% !important;
}
</style>
