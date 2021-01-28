<template>
  <div>
    <h1>Vue Tree Browser</h1>
    <h3>version: 0.0.2</h3>
    <Tree :node="root" :selected="selected" @change="handleNodeClick" />
  </div>
</template>

<script>
import Tree from "./components/Tree";
import mocksInterests from "./mocks-interests";

export default {
  components: {
    Tree
  },
  data() {
    return {
      root: mocksInterests,
      selected: []
    };
  },
  methods: {
    handleNodeClick({ key }) {
      let newSelected = this.selected;
      if (newSelected.includes(key)) {
        newSelected.splice(newSelected.indexOf(key), 1);
      } else {
        newSelected.push(key);
      }
      this.selected = newSelected;
    }
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  text-align: center;
  margin: 0 auto;
}
</style>
