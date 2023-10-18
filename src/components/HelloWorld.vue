<script setup>
import { h, nextTick } from 'vue';
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
  const treeNode = event.target.closest('.tree-node');
  // if (!treeNode.classList.contains('extend')) {
  //   const childElements = treeNode.querySelectorAll('.child');
  //   childElements.forEach((childElement) => {
  //     childElement.style.height = 50 + "px";
  //   });
  // }else {
  //   const childElements = treeNode.querySelectorAll('.child');
  //   childElements.forEach((childElement) => {
  //     childElement.style.height = 0;
  //   });
  // }
  treeNode.classList.toggle('extend');
  

}

const getNodeTree = (node, level) => {
  if (!node.children || node.children?.length <= 0) {
    return h('div', { class: 'tree-node ' + (level === 0 ? 'node-root' : ''), 'data-key': node?.key }, [
      h('span', { class: 'tree-title' }, node?.title)
    ]);
  } else {
    let children = h('div', { class: 'tree-node haschild ' + (level === 0 ? 'node-root' : ''), 'data-key': node?.key }, [
      h('span', { class: 'btn' , onClick: handleToggleExtend  }, '+'),
      h('span', { class: 'tree-title' }, node?.title)
    ]);
    
    for (let i = 0; i < node.children.length; i++) {
      children.children.push(
        h('div', { class: 'child' }, [
          h('div', { class: 'tree-space' }),
          getNodeTree(node.children[i])
        ])
      );
    }
    
    return children;
  }
};

</script>

<template>
  <div>
    <div class="tree-root">
      <div v-for="node in treeData" >
        <component :is="getNodeTree(node)" />
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
  width: 0.5px;
  background: #181818;
}
.tree-node > .child{
  height: 0;
  transition: all 0.2s ease-out;
  overflow-y: hidden;
}
.tree-node.extend > .child{
  transition: all 0.2s ease-in;
  min-height: 20px;
}
.btn {
  cursor: pointer;
}

</style>
