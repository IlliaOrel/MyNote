<template>
  <div id="app">
    <Notebook @change-page="changePage" @new-page="newPage" :pages="pages" :activePage="index"/>
    <Page
      @save-page="savePage"
      @delete-page="deletePage"
      @get-content="getContent"
      :page="currentPage"
      :id="index"
    />
  </div>
</template>
<script>
import Notebook from "./components/Notebook";
import Page from "./components/Page";

export default {
  name: "app",
  components: {
    Notebook,
    Page
  },
  data: () => ({
    pages: [],
    index: 0
  }),
  mounted() {
    if (localStorage.getItem("pages")) {
      try {
        this.pages = JSON.parse(localStorage.getItem("pages"));
      } catch (e) {
        localStorage.removeItem("pages");
      }
    }
  },
  computed: {
    currentPage() {
      return this.pages[this.index];
    }
  },
  methods: {
    newPage() {
      this.pages.push({
        title: "",
        content: ""
      });
      this.index = this.pages.length - 1;
    },

    changePage(index) {
      this.index = index;
    },

    updateLocalStorage() {
      const parsed = JSON.stringify(this.pages);
      localStorage.setItem("pages", parsed);
    },

    savePage(c) {
      this.pages = [...this.pages];

      this.pages[c.id] = {
        title: c.title,
        content: c.content
      };

      this.updateLocalStorage();
    },
    /* isEmpty(c) {
      const isEmpty = c.title.length || c.content.length === 0;
      if (!isEmpty) {
        savePage(c);
      }

      if (isEmpty) {
        const p = document.createElement("p");
        p.textContent = "You are have not tasks.";
      }
    }, */

    deletePage() {
      this.pages.splice(this.index, 1);
      this.index = Math.max(this.index - 1, 0);
      this.updateLocalStorage();
    }
  }
};
</script>

<style lang="scss" scoped>
html,
body,
#app {
  height: 100%;
  width: 100%;
}

body {
  margin: 0;
}

#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  display: flex;
  flex-direction: row;
  box-shadow: 7rem 0 5rem 3rem #f7ffad;
  border: 2px solid rgba(94, 72, 62, 0.993);
  border-top-right-radius: 20px;
  border-bottom-right-radius: 20px;
}
@media screen and (max-width: 400px) {
  #app {
    width: 100%;
    display: flex;
    flex-direction: column;
    border: none;
    box-shadow: none;
  }
}
</style>
