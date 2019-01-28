<template>
  <div class="page">
    <div class="all" v-if="page">
      <label for="title">Title</label>
      <div>
        <input type="text" v-model="title" class="title" name="title" placeholder="Enter a title">
      </div>
      <label for="content">Content</label>
      <div>
        <div class="editor">
          <vue-editor v-model="v" placeholder="Enter some content"></vue-editor>
        </div>
        <button class="delete" @click="deletePage()">Delete Page</button>
        <button class="save" @click="savePage()">Save Page</button>
      </div>
    </div>
    <div v-else>
      <h1>&larr; To start, create a new page!</h1>
    </div>
  </div>
</template>

<script>
import { VueEditor } from "vue2-editor";
export default {
  name: "Page",
  components: { VueEditor },
  props: ["page", "id"],
  data: () => ({
    v: "",
    title: ""
  }),
  watch: {
    page: {
      immediate: true,
      handler(v) {
        if (v) {
          this.v = v.content;
          this.title = v.title;
        }
      }
    }
  },
  methods: {
    deletePage() {
      this.$emit("delete-page");
    },
    savePage() {
      this.$emit("save-page", {
        title: this.title,
        content: this.v,
        id: this.id
      });
    },
    setContent(c) {
      this.v = c;
    }
  }
};
</script>

<style lang="scss" scoped>
.page {
  width: 100%;
  padding: 2rem;
  background-image: url("../../src/image/doodles.png");
  border-top-right-radius: 20px;
  border-bottom-right-radius: 20px;
  opacity: 0.9;
  @media screen and (max-width: 400px) {
    max-width: 400px;
    width: 100%;
    margin-left: -10px;
    border-radius: 0 20px 0 0;
    margin-bottom: -15px;
  }
}

.content,
.title {
  border-style: none;
  border-radius: 0.25rem;
  border: solid 1px lightgray;
  width: 100%;
  box-sizing: border-box;
  margin-bottom: 1.25rem;
}

.content:focus,
.title:focus {
  outline: 0;
}

.content {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  resize: vertical;
  font-size: 1.5rem;
  padding: 0.5rem;
  height: 20rem;
}

.title {
  font-size: 1.5rem;
  padding: 0.5rem 1rem;
}

label {
  margin-bottom: 0.5rem;
  display: inline-block;
  font-weight: bold;
  color: rgba(58, 44, 38, 0.993);
}

button {
  border-style: none;
  margin-top: 5px;
  padding: 0.5rem 0.75rem;
  background-color: #e6c275;
  margin-right: 1rem;
  border-radius: 0.25rem;
  color: white;
  font-size: 1rem;
  cursor: pointer;
  @media screen and (max-width: 400px) {
    padding: 1rem 0.75rem;
    margin-right: 5px;
    margin-top: 10px;
    font-size: 1.1rem;
  }
}

button:hover {
  background-color: #d66a21;
}

h1 {
  color: rgba(94, 72, 62, 0.993);
}

.editor {
  background-color: #fff;
  max-height: 283px;
  overflow-y: auto;
  @media screen and (max-width: 400px) {
    width: 100%;
    max-width: 400px;
  }
}
</style>