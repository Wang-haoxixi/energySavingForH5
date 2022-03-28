<template>
  <div></div>
</template>

<script>
// import store from "@/store";
import { zh_cn } from "./zh_cn";
import Jodit from "jodit"

export default {
  name: "JoditVue",

  props: {
    value: { type: String, required: true },
    config: { type: Object, default: () => ({}) },
  },

  computed: {
    defaultConfig() {
      return {
        i18n: {
          zh_cn,
        },
        disablePlugins: "media,symbols,sticky",
        language: "zh_cn",
        buttons:
          "source,|,undo,redo,bold,strikethrough,underline,italic,|,align,,,outdent,indent,|,font,fontsize,brush,paragraph,|,image,file,table,link,\n,cut,hr,eraser,copyformat,fullsize,print",
        buttonsMD:
          "source,|,undo,redo,bold,strikethrough,underline,italic,|,align,,,outdent,indent,|,font,fontsize,brush,paragraph,|,image,file,table,link,|,cut,hr,eraser,copyformat,fullsize,print",
        buttonsSM:
          "source,|,undo,redo,bold,strikethrough,underline,italic,|,align,,,outdent,indent,|,font,fontsize,brush,paragraph,|,image,file,table,link,|,cut,hr,eraser,copyformat,fullsize,print",
        buttonsXS:
          "source,|,undo,redo,bold,strikethrough,underline,italic,|,align,,,outdent,indent,|,font,fontsize,brush,paragraph,|,image,file,table,link",
        // uploader: {
        //   url: "/api/admin/file/app/sftp/upload",
        //   headers: {
        //     Authorization: "Bearer " + store.getters.accessToken,
        //   },
        //   isSuccess(resp) {
        //     return !resp.code;
        //   },
        //   process(resp) {
        //     return {
        //       files: resp.data || [],
        //       path: "",
        //       baseurl: "",
        //       error: resp.msg,
        //       msg: resp.msg,
        //     };
        //   },
        // },
        ...this.config,
      };
    },
  },

  watch: {
    value: {
      handler: function(newValue) {
        if (this.editor.value !== newValue) {
          this.initEditor(newValue);
        }
      },
    },
  },

  mounted() {
    // Code that will run only after the
    // entire view has been rendered
    // eslint-disable-next-line no-undef
    this.editor = new Jodit(this.$el, this.defaultConfig);
    this.editor.events.on("change", newValue => this.$emit("input", newValue));
    this.initEditor(this.value);
  },

  beforeDestroy() {
    this.editor.destruct();
  },
  methods: {
    initEditor(newValue) {
      this.$nextTick(() => {
        this.$set(this.editor, "value", newValue);
      });
    },
  },
};
</script>
