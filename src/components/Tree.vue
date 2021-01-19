<template>
  <div class="tree-browser">
    <!-- Tree의 마지막 노드일때 -->
    <div v-if="!hasChildren">
      <div :style="{ margin: `0 ${depth * 10}px 0 ${depth * 10}px` }">
        <label :for="node.label" :class="getClass">
          <input
            type="checkbox"
            class="node"
            :name="node.label"
            :id="node.label"
            :value="node.label"
            @change="e => handleChange(e, node.key)"
          />
          {{ node.label }}
        </label>
      </div>
    </div>
    <!-- Tree의 마지막 노드가 아닐때 -->
    <div v-else :class="expanded ? 'expanded' : 'fold'">
      <div :style="{ 'margin-left': `${depth * 20}px` }" @click="nodeClicked">
        <div>
          <span class="type">
            {{ expanded ? "&#9660;" : "&#9658;" }}
          </span>
          <span>{{ node.label }}</span>
        </div>
      </div>

      <div
        :class="!child.children ? 'leaf-node' : ''"
        v-for="child in node.children"
        :key="child.name"
      >
        <Tree
          v-if="expanded"
          :node="child"
          :depth="depth + 1"
          :selected="selected"
          @change="node => $emit('change', node)"
        />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Tree",
  props: {
    node: Object,
    depth: {
      type: Number,
      default: 0
    },
    selected: {
      type: Array
    }
  },
  data() {
    return {
      expanded: false
    };
  },
  methods: {
    nodeClicked() {
      this.expanded = !this.expanded;
      if (!this.hasChildren) {
        this.$emit("onClick", this.node);
      }
    },
    handleChange(event, key) {
      this.$emit("change", { event, key });
    }
  },
  computed: {
    getClass() {
      return this.selected.includes(this.node.key) ? "selected" : "unselected";
    },
    hasChildren() {
      const { children } = this.node;
      return children && children.length > 0;
    }
  }
};
</script>

<style scoped>
.tree-browser label {
  cursor: pointer;
  padding: 3px 10px;
}
.tree-browser input[type="checkbox"] {
  display: none;
  box-shadow: 0 1px 2px rgba(0, 0, 0, 0.05),
    inset 0px 1px 3px rgba(0, 0, 0, 0.1);
}
.tree-browser .selected {
  /* border: solid 1px blue; */
  border-radius: 10px;
  background-color: aqua;
}
.tree-browser .selected:after {
  content: "x";
  margin-left: 10px;
}

.node {
  text-align: left;
  font-size: 18px;
}
.expanded > .leaf-node {
  display: inline-flex;
}
</style>
