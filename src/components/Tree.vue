<template>
  <div class="container">
    <tree
      :data="computedTree"
      node-text="name"
      layoutType="euclidean"
      type="tree"
      :zoomable="true"
      :radius="4"
    ></tree>
  </div>
</template>

<script>
import { tree } from 'vued3tree'
import { mapState } from 'vuex'
export default {
  name: 'Tree',
  components: {
    tree
  },
  computed: {
    ...mapState(['componentMap']),
    computedTree () {
      return this.buildTree()
    }
  },
  data () {
    return {
      tree: null
    }
  },
  methods: {
    formatComponentMap (compMap) {
      console.log('compMap', compMap)
      let result = []
      Object.values(compMap).forEach(compData => {
        result.push({
          name: compData.componentName,
          children: compData.children
        })
      })
      return result
    },
    transformToTree (data) {
      let result = {}
      const nodes = {}
      const formattedData = this.formatComponentMap(data)

      formattedData.forEach(component => {
        if (!nodes[component.name]) {
          nodes[component.name] = { name: component.name, children: [] }
          result = nodes
        }
        component.children.forEach(child => {
          nodes[child] = { name: child, children: [] }
          nodes[component.name].children.push(nodes[child])
        })
      })
      return result
    },
    buildTree () {
      let build = this.transformToTree(this.componentMap)
      return build['App']
    }
  }
}
</script>

<style lang="stylus">
.container {
  height: 100%;
  width: 100%;
}

.treeclass .nodetree text {
  font-size: 19px !important;
  cursor: pointer;
  text-shadow: none !important;
  font-weight: bold;
  fill: #FFF;
  /* none of these classes work
  color: white !important;
  background: white;
  margin: 1rem; */
}
  /* changes the circle node color */
.treeclass .nodetree circle {
  fill: #A2D9FF;
}
/* changes the stroke color */
.treeclass .linktree {
  stroke: $secondary !important;
  stroke-opacity: 0.4;
  stroke-width: 8px;
}
</style>
