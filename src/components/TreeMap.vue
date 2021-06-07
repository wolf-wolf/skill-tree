<template>
    <div class="container">
        <div class="op-panel">
            <div class="op-btn" @click="controlScale('bigger')">+</div>
            <div class="op-btn" @click="controlScale('smaller')">-</div>
            <div class="op-btn" @click="controlScale('restore')">1:1</div>
        </div>
        <vue-tree
            style="width: 100vw; height: 100vh;"
            :dataset='richMediaData'
            :config='treeConfig'
            ref='scaleTree'
            linkStyle='straight'
        >
            <template v-slot:node='{ node, collapsed }'>
                <div class='rich-media-node'>
                    <div class="inner-node" :style="getNodeBgColor(node.familiar, collapsed)">{{ node.name }}</div>
                </div>
            </template>
        </vue-tree>
    </div>
</template>

<script>
import VueTree from '../vue-tree/VueTree.vue'
import TreeData from '../assets/js/tree.json'

export default {
    name: 'treemap',
    components: {'vue-tree': VueTree},
    data() {
        return {
            richMediaData: TreeData,
            treeConfig: {nodeWidth: 200, nodeHeight: 30, levelHeight: 60}
        }
    },
    methods: {
        getNodeBgColor(value) {
            let opacity = value === undefined ? 0 : value;

            console.log(opacity)

            return {
                background: `rgba(0,0,255,${opacity})`,8
                borderColor: `rgba(0,0,255,${opacity})`,
                border: `1px solid blue`,
                color: opacity <= 0.3 ? '#333' : '#fff'
            }
        },
        controlScale(command) {
            switch (command) {
                case 'bigger':
                    this.$refs.scaleTree.zoomIn()
                    break
                case 'smaller':
                    this.$refs.scaleTree.zoomOut()
                    break
                case 'restore':
                    this.$refs.scaleTree.restoreScale()
                    break
            }
        }
    }
}
</script>

<style scoped lang='scss'>
.container {
    display: flex;
    flex-direction: column;
    align-items: center;

    .op-panel {
        display: flex;
        justify-content: flex-start;
        align-items: center;

        .op-btn {
            padding: 4px 8px;
            border-radius: 4px;
            font-family: "Roboto", sans-serif;
            font-size: 14px;
            border: 1px solid #0084ff;
            background: #0084ff;
            color: #fff;
            min-width: 80px;
            height: 30px;
            line-height: 22px;
            cursor: pointer;
            box-sizing: border-box;
            margin-right: 16px;
        }
    }
}

.tree-node {
    display: inline-block;
    width: 28px;
    height: 28px;
    border-radius: 50%;
    background-color: antiquewhite;
    text-align: center;
    line-height: 28px;
}

.rich-media-node {
    color: #333;
    background-color: #fff;
    font-size: 12px;
    overflow: hidden;

    .inner-node {
        padding: 8px 12px;
        user-select: none;
        border-radius: 50%;
    }
}

h3 {
    margin-top: 32px;
    margin-bottom: 16px;
}
</style>
