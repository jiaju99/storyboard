<template>
    <div class="control-container">
        <div class="title"><span>control</span></div>
        <div class="control-container-box">
            <el-row v-for="(row, rowKey) in showRow" :key="rowKey" >
                <el-col v-for="(col, colKey) in showCol" :key="colKey" :span='24 / showCol'>
                    <div v-if="rowKey * showCol + colKey < controls.length">
                        <draggable :options="{group:{ name:'control',  pull:'clone', put:false }}" 
                        @start="onStart" @end="onEnd" @choose="onChoose">
                            <component
                                :initial="true"
                                :id="rowKey * showCol + colKey"
                                :control="controls[rowKey * showCol + colKey]"
                                :attribute="controls[rowKey * showCol + colKey].attribute" 
                                :is="controls[rowKey * showCol + colKey].name">
                            </component>
                        </draggable>
                    </div>
                </el-col>
            </el-row>
        </div>
    </div>
</template>

<script>
  
    import draggable from 'vuedraggable'
    import components from '@/components/template/elementUI/control/index'
    import uiControls from '@/components/template/elementUI/attribute/index.js'

    export default {
        data() {
            return {
            
            }
        },
        computed: {
            controls () {
                var arr = []
                for (let key in uiControls) {
                    arr.push(uiControls[key]); 
                }
                return arr
            },
            showRow: function() {
                let length = this.controls.length / 3
                return Math.ceil(length)
            },
            showCol: function() {
                return 3
            },

        },
        components: {
            draggable,

        },
        watch: {
          
        },
        updated() {
            console.log('updated')
      
        },
        methods: {
            onEnd (evt,ee) {
                
                if (this.$store.state.currentAddControl) {
                    let control = JSON.parse(JSON.stringify(this.controls[evt.item.id]))
                    let array = this.$store.state.controls[this.$store.state.currentController.id] || []
                    let showCount = array.length
                    control.index = showCount
                    control.show = true

                    this.$store.state.currentControl = control
                    this.$store.state.currentIndex = showCount

                    array.push(this.$store.state.currentControl)
                    this.$set(this.$store.state.controls, this.$store.state.currentController.id, array)

                    let sceneTreeNode = {
                        pid: 0,
                        id: 0,
                        parent: true,
                        rename: false,
                        label: 'View 1',
                        children: []
                    } 
                    this.$set(this.$store.state.sceneTree, sceneTreeNode.id, sceneTreeNode)
                }
                this.$store.state.currentAddControl = false
            },
            onStart (evt) {
            //    console.log('onStart')

            },
            onChoose (evt) {
                // console.log('onChoose')
            },
        }
    }
</script>

<style scoped>
    .control-container {
        background-color: #eeeeee;
        height: 50%;
    }
    .control-container .title {
        color: black;
        font-size: 18px;
        border-top: 1px solid #cccccc;
        border-bottom: 1px solid #dddddd;
        height: 5%;
    }
    .control-container-box {
        background-color:#eeeeee;
        height: 95%;
        text-align: center;
    }
</style>