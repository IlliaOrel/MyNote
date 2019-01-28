<template>
  <div class="notebook">
    <ul>
      <template v-for="(page, index) of renderList">
        <li
          @click="changePage(index)"
          :key="page.id"
          :class="{ 'active': index === activePage }"
          class="page"
          v-if="page.title.length || page.content.length"
        >
          <h1 class="main">
            {{page.title}}
            <p id="txt">{{page.content}}</p>
          </h1>
        </li>
      </template>
      <li class="new-page" @click="newPage()">
        <font-awesome-icon icon="plus-circle"/>
      </li>
    </ul>
  </div>
</template>
<script>
export default {
  name: "Notebook",
  props: ["pages", "activePage"],

  methods: {
    changePage(index) {
      this.$emit("change-page", index);
    },
    newPage() {
      this.$emit("new-page");
    }
  },

  computed: {
    renderList() {
      return this.pages.map(p => {
        let c = document.createElement("div");
        c.innerHTML = p.content;
        const content = c.textContent;
        c = null;
        return {
          title: p.title,
          content
        };
      });
    }
  }
};
</script>

<style lang="scss" scoped>
.notebook {
  max-width: 20rem;
  width: 30rem;
  background: #ebd472;
  border-right: 2px solid rgba(94, 72, 62, 0.993);
  overflow-y: auto;
  max-height: 600px;
}
@media screen and (max-width: 400px) {
  .notebook {
    width: 100%;
    max-width: 400px;
    border: none;
    padding-right: 50px;
    border-radius: 20px;
  }
}

ul {
  list-style-type: none;
  padding: 0;
  margin: 0;
  height: 100%;
  position: relative;
  @media screen and (max-width: 400px) {
    width: 100%;
    max-width: 400px;
  }
}

li {
  padding: 1rem;
  font-size: 1.5rem;
  min-height: 1.5rem;
  white-space: pre-wrap;
  overflow: hidden;
  text-overflow: ellipsis;
  margin-left: 0;
  @media screen and (max-width: 400px) {
    width: 100%;
    max-width: 400px;
    padding-right: 32px;
  }
}

p {
  font-size: 1rem;
  min-height: 1rem;
  color: rgb(165, 124, 91);
  white-space: normal;
  line-height: 15px;
}

li:hover {
  cursor: pointer;
  background-color: #e0c180;
  border: 1px solid rgb(180, 178, 178);
}

.active {
  background-color: #e0c180;
}

.active:hover {
  background-color: #d66a21;
}

.new-page {
  background-color: #e0bd71;
  color: white;
  position: relative;
  text-align: center;
  bottom: 0;
  width: 100%;
  box-sizing: border-box;
  font-size: 2rem;
  margin-left: 0;
  @media screen and (max-width: 400px) {
    width: 100%;
    max-width: 200px;
    border-radius: 20px;
    margin-left: 70px;
  }
}

.new-page:hover {
  background-color: #d66a21;
}
.new-page:active {
  background-color: #944918;
}

.main {
  word-wrap: break-word;
  max-height: 150px;
  font-size: 1.5rem;
  line-height: 20px;
  color: rgba(94, 72, 62, 0.993);
  margin-bottom: 0;
  margin-top: -25px;
}
</style>
