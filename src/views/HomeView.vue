<template>
  <div class="container">
    <div>
      <h3>Quill Editor</h3>
      <QuillEditor
        theme="snow"
        toolbar="full"
        v-model:content="content"
        contentType="html"
      />
      <button style="margin-top: 12px" @click="save">Save</button>
    </div>
  </div>
  <div class="container" style="margin-top: 12px">
    <input type="text" v-model="id" />
    <button @click="show">Show</button>
  </div>
  <div v-html="showContent"></div>
</template>

<script>
import { QuillEditor } from "@vueup/vue-quill";
import "@vueup/vue-quill/dist/vue-quill.snow.css";
import axios from "axios";

export default {
  data() {
    return {
      content: "",
      showContent: "",
      id: "",
    };
  },
  components: {
    QuillEditor,
  },
  methods: {
    save() {
      axios.post("https://text-editor-api.herokuapp.com/create", {
        content: this.content,
      });
    },
    async show() {
      const response = await axios.post(
        "https://text-editor-api.herokuapp.com/read",
        {
          id: this.id,
        }
      );
      if (response.data.data.length === 0) {
        this.showContent = "";
        return;
      }
      this.showContent = response.data.data[0].content;
    },
  },
};
</script>

<style scoped>
.container {
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
