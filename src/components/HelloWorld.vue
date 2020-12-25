<template>
  <el-tree
    :data="data"
    show-checkbox
    node-key="id"
    draggable
    default-expand-all
    :expand-on-click-node="false"
  >
    <template #default="{ node, data }">
      <span class="custom-tree-node">
        <span>{{ node.label }}</span>
        <span>
          <a @click="append(data)"> Append </a>
          <a @click="remove(node, data)"> Delete </a>
        </span>
      </span>
    </template>
  </el-tree>
</template>

<script>
import { ref, watch } from "vue";
let id = 1000;
export default {
  name: "Config",
  setup() {
    const json = [
      {
        id: 1,
        label: "一级 1",
        children: [
          {
            id: 4,
            label: "二级 1-1",
            children: [
              {
                id: 9,
                label: "三级 1-1-1",
              },
              {
                id: 10,
                label: "三级 1-1-2",
              },
            ],
          },
        ],
      },
      {
        id: 2,
        label: "一级 2",
        children: [
          {
            id: 5,
            label: "二级 2-1",
          },
          {
            id: 6,
            label: "二级 2-2",
          },
        ],
      },
      {
        id: 3,
        label: "一级 3",
        children: [
          {
            id: 7,
            label: "二级 3-1",
          },
          {
            id: 8,
            label: "二级 3-2",
          },
        ],
      },
    ];
    const treeData = ref(JSON.parse(JSON.stringify(json)));
    watch(treeData, () => {
      console.log("change", treeData);
    });
    function append(data) {
      const newChild = { id: id++, label: "test", children: [] };
      if (!data.children) {
        data.children = [];
      }
      data.children.push(newChild);
      treeData.value = JSON.parse(JSON.stringify(treeData.value));
    }
    function remove(node, data) {
      const parent = node.parent;
      const children = parent.data.children || parent.data;
      const index = children.findIndex((d) => d.id === data.id);
      children.splice(index, 1);
      treeData.value = JSON.parse(JSON.stringify(treeData.value));
    }
    return {
      data: treeData,
      append,
      remove,
    };
  },
};
</script>
<style lang="less" scoped>
</style>