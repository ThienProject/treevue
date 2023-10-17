<script setup>
const treeData = ([
  {
    title: 'parent 1',
    key: '0-0',
    children: [
      {
        title: 'parent 1-0',
        key: '0-0-0',
        children: [
          {title: '0-0-0-0', key: '0-0-0-0'},
          {
            title: '0-0-0-1', key: '0-0-0-1',
          },
          {title: '0-0-0-2', key: '0-0-0-2'},
        ],
      },
      {
        title: 'parent 1-1',
        key: '0-0-1',
        children: [{title: 'leaf', key: '0-0-1-0'}],
      },
      {
        title: 'parent 1-2',
        key: '0-0-2',
        children: [
          {title: 'leaf 1', key: '0-0-2-0'},
          {
            title: 'leaf 2',
            key: '0-0-2-1',
          },
        ],
      },
    ],
  },
  {
    title: 'parent 2',
    key: '0-1',
    children: [
      {
        title: 'parent 2-0',
        key: '0-1-0',
        children: [
          {title: 'leaf', key: '0-1-0-0'},
          {title: 'leaf', key: '0-1-0-1'},
        ],
      },
    ],
  },
]);

const handleToggleExtend =  (event)=>{
  console.log(event);
  event.target.closest().classList.toggle('extend');
}

const getNodeTree = (node, level) => {
  if (!node.children || node.children?.length <= 0) {
    return `<div class="tree-node ${level == 0 ?'node-root':''}" data-key="${node?.key}"><span class="tree-title">node?.title</span></div>`;
  } else {
    let children = `<div class="tree-node haschild ${level == 0 ?'node-root':''}" :data-key="${node?.key}">
                        <span class="btn" @click.native.capture="handleToggleExtend">
                            +
                        </span>
                    <span class="tree-title">${node?.title}</span>`;
    for (let i = 0; i < node.children.length; i++) {
      children += `
                    <div class="child">
                        <div class="tree-space"></div>
                        <span class="btn" @click.native.capture="handleToggleExtend">
                            +
                        </span>
                        ${getNodeTree(node.children[i])}
                    </div>
             `
    }
    children += ` </div>`;
    return children;
  }
}

</script>

<template>
  <div>
    <div class="tree-root">
      <div v-for="node in treeData" v-html="getNodeTree(node)" >
      </div>
    </div>
  </div>
</template>

<style>
.tree-space {
  display: inline-block;
  padding: 10px;
}
.tree-node {
  display: inline-block;
  position: relative;
}
.tree-node.extend.haschild::before {
  position: absolute;
  content: "";
  height: 90%;
  top: 10%;
  left: -2px;
  width: 2px;
  background: #181818;
}
.tree-node .child{
  height :0;
  overflow-y: hidden;
}
.tree-node.extend .child{
  height: 100%;
}
.btn {
  cursor: pointer;
}

</style>
