<template>
    <div id="app">
        <h1 style="text-align: center">Vue Grid Layout</h1>
        <!--<pre>{{ layout | json }}</pre>-->
        <div>
            <div class="layoutJSON">
                Displayed as <code>[x, y, w, h]</code>:
                <div class="columns">
                    <div class="layoutItem" v-for="item in layout" :key="item.i">
                        <b>{{item.i}}</b>: [{{item.x}}, {{item.y}}, {{item.w}}, {{item.h}}]
                    </div>
                </div>
            </div>
            <!-- 
            <div class="layoutJSON">
                Displayed as <code>[x, y, w, h]</code>:
                <div class="columns">
                    <div class="layoutItem" v-for="item in layout2" :key="item.i">
                        <b>{{item.i}}</b>: [{{item.x}}, {{item.y}}, {{item.w}}, {{item.h}}]
                    </div>
                </div>
            </div>
            -->
        </div>
        <slider-table></slider-table>
        <div id="content">
            <button @click="decreaseWidth">Decrease Width</button>
            <button @click="increaseWidth">Increase Width</button>
            <button @click="addItem">Add an item</button>
            <button @click="removeItem">Remove an item</button>
            <!-- Add to show rtl support -->
            <button @click="changeDirection">Change Direction</button>
            <input type="checkbox" v-model="draggable"/> Draggable
            <input type="checkbox" v-model="resizable"/> Resizable
            <input type="checkbox" v-model="mirrored"/> Mirrored
            <input type="checkbox" v-model="responsive"/> Responsive
            <input type="checkbox" v-model="preventCollision"/> Prevent Collision
            <div style="margin-top: 10px;margin-bottom: 10px;">
                Row Height: <input type="number" v-model="rowHeight"/> Col nums: <input type="number" v-model="colNum"/>
            </div>
            <!-- 
            <grid-layout
                    :layout.sync="layout"
                    :col-num="parseInt(colNum)"
                    :row-height="rowHeight"
                    :is-draggable="draggable"
                    :is-resizable="resizable"
                    :is-mirrored="mirrored"
                    :prevent-collision="preventCollision"
                    :vertical-compact="true"
                    :use-css-transforms="true"
                    :responsive="responsive"
                    @layout-created="layoutCreatedEvent"
                    @layout-before-mount="layoutBeforeMountEvent"
                    @layout-mounted="layoutMountedEvent"
                    @layout-ready="layoutReadyEvent"
                    @layout-updated="layoutUpdatedEvent"
            >
                <grid-item v-for="item in layout" :key="item.i"
                           :static="item.static"
                           :x="item.x"
                           :y="item.y"
                           :w="item.w"
                           :h="item.h"
                           :i="item.i"
                           @resize="resize"
                           @move="move"
                           @resized="resized"
                           @container-resized="containerResized"
                           @moved="moved"
                >
                -->
                    <!--<custom-drag-element :text="item.i"></custom-drag-element>
                    <test-element :text="item.i"></test-element>
                    <button @click="clicked">CLICK ME!</button>-->
                <!--
                </grid-item>
            </grid-layout>
            <hr/>-->
            <grid-layout
                    :layout="layout"
                    :col-num="22"
                    :row-height="rowHeight"
                    :is-draggable="draggable"
                    :is-resizable="resizable"
                    :vertical-compact="true"
                    :use-css-transforms="true"
            >
                <grid-item v-for="item in layout" 
                           :key="item.i"
                           :static="item.static"
                           :isHeader="item.resizable"
                           :x="item.x"
                           :y="item.y"
                           :w="item.w"
                           :h="item.h"
                           :min-w="2"
                           :min-h="2"
                           :i="item.i"
                           :is-draggable="false"
                           :is-resizable="item.resizable"
                           @click.native="clicked($event, item.x, item.y)"
                >
                    <test-element :text="item.i"></test-element>
                </grid-item>
            </grid-layout>
            
        </div>

    </div>
</template>

<script>
    import GridItem from './components/GridItem.vue';
    import GridLayout from './components/GridLayout.vue';
    // import ResponsiveGridLayout from './components/ResponsiveGridLayout.vue';
    import TestElement from './components/TestElement.vue';
    import CustomDragElement from './components/CustomDragElement.vue';
    import {getDocumentDir, setDocumentDir} from "./helpers/DOM";
    import SliderTable from './components/SliderTable.vue';
    //var eventBus = require('./eventBus');

    let testLayout = [
        {"x":0,"y":0,"w":2,"h":2,"i":"", resizable: true, draggable: false, static: true},
        {"x":2,"y":0,"w":2,"h":1,"i":"1", resizable: true, draggable: false, static: true},
        {"x":4,"y":0,"w":2,"h":1,"i":"2", resizable: true, draggable: false, static: true},
        {"x":6,"y":0,"w":2,"h":1,"i":"3", resizable: true, draggable: false, static: true},
        {"x":8,"y":0,"w":2,"h":1,"i":"4", resizable: true, draggable: false, static: true},
        {"x":10,"y":0,"w":2,"h":1,"i":"5", resizable: true, draggable: false, static: true},
        {"x":12,"y":0,"w":2,"h":1,"i":"6", resizable: true, draggable: false, static: true},
        {"x":14,"y":0,"w":2,"h":1,"i":"7", resizable: true, draggable: false, static: true},
        {"x":16,"y":0,"w":2,"h":1,"i":"8", resizable: true, draggable: false, static: true},
        {"x":18,"y":0,"w":2,"h":1,"i":"9", resizable: true, draggable: false, static: true},
        {"x":20,"y":0,"w":2,"h":1,"i":"10", resizable: true, draggable: false, static: true},

        {"x":2,"y":1,"w":2,"h":1,"i":"1", resizable: true, draggable: false, static: true},
        {"x":4,"y":1,"w":2,"h":1,"i":"2", resizable: true, draggable: false, static: true},
        {"x":6,"y":1,"w":2,"h":1,"i":"3", resizable: true, draggable: false, static: true},
        {"x":8,"y":1,"w":2,"h":1,"i":"4", resizable: true, draggable: false, static: true},
        {"x":10,"y":1,"w":2,"h":1,"i":"5", resizable: true, draggable: false, static: true},
        {"x":12,"y":1,"w":2,"h":1,"i":"6", resizable: true, draggable: false, static: true},
        {"x":14,"y":1,"w":2,"h":1,"i":"7", resizable: true, draggable: false, static: true},
        {"x":16,"y":1,"w":2,"h":1,"i":"8", resizable: true, draggable: false, static: true},
        {"x":18,"y":1,"w":2,"h":1,"i":"9", resizable: true, draggable: false, static: true},
        {"x":20,"y":1,"w":2,"h":1,"i":"10", resizable: true, draggable: false, static: true},

        {"x":0,"y":2,"w":2,"h":3,"i":"9:00", resizable: false, draggable: false, static: false},
        {"x":2,"y":2,"w":2,"h":3,"i":"", resizable: false, draggable: false, static: false},
        {"x":4,"y":2,"w":2,"h":3,"i":"", resizable: false, draggable: false, static: false},
        {"x":6,"y":2,"w":2,"h":3,"i":"", resizable: false, draggable: false, static: false},
        {"x":8,"y":2,"w":2,"h":3,"i":"", resizable: false, draggable: false, static: false},
        {"x":10,"y":2,"w":2,"h":3,"i":"", resizable: false, draggable: false, static: false},
        {"x":12,"y":2,"w":2,"h":3,"i":"", resizable: false, draggable: false, static: false},
        {"x":14,"y":2,"w":2,"h":3,"i":"", resizable: false, draggable: false, static: false},
        {"x":16,"y":2,"w":2,"h":3,"i":"", resizable: false, draggable: false, static: false},
        {"x":18,"y":2,"w":2,"h":3,"i":"", resizable: false, draggable: false, static: false},
        {"x":20,"y":2,"w":2,"h":2,"i":"", resizable: false, draggable: false, static: false},
        {"x":20,"y":4,"w":2,"h":1,"i":"再", resizable: false, draggable: false, static: true},


        {"x":0,"y":5,"w":2,"h":3,"i":"10:00", resizable: false, draggable: false, static: false},
        {"x":2,"y":5,"w":2,"h":3,"i":"1", resizable: false, draggable: false, static: false},
        {"x":4,"y":5,"w":2,"h":6,"i":"再", resizable: false, draggable: false, static: true},
        {"x":6,"y":5,"w":2,"h":3,"i":"3", resizable: false, draggable: false, static: false},
        {"x":8,"y":5,"w":2,"h":3,"i":"4", resizable: false, draggable: false, static: false},
        {"x":10,"y":5,"w":2,"h":3,"i":"5", resizable: false, draggable: false, static: false},
        {"x":12,"y":5,"w":2,"h":3,"i":"6", resizable: false, draggable: false, static: false},
        {"x":14,"y":5,"w":2,"h":3,"i":"7", resizable: false, draggable: false, static: false},
        {"x":16,"y":5,"w":2,"h":3,"i":"8", resizable: false, draggable: false, static: false},
        {"x":18,"y":5,"w":2,"h":3,"i":"9", resizable: false, draggable: false, static: false},
        {"x":20,"y":5,"w":2,"h":3,"i":"10", resizable: false, draggable: false, static: false},

        {"x":0,"y":8,"w":2,"h":3,"i":"11:00", resizable: false, draggable: false, static: false},
        {"x":2,"y":8,"w":2,"h":3,"i":"1", resizable: false, draggable: false, static: false},
        //{"x":4,"y":8,"w":2,"h":3,"i":"2", resizable: false, draggable: false, static: false},
        {"x":6,"y":8,"w":2,"h":3,"i":"3", resizable: false, draggable: false, static: false},
        {"x":8,"y":8,"w":2,"h":3,"i":"4", resizable: false, draggable: false, static: false},
        {"x":10,"y":8,"w":2,"h":3,"i":"5", resizable: false, draggable: false, static: false},
        {"x":12,"y":8,"w":2,"h":3,"i":"6", resizable: false, draggable: false, static: false},
        {"x":14,"y":8,"w":2,"h":3,"i":"7", resizable: false, draggable: false, static: false},
        {"x":16,"y":8,"w":2,"h":3,"i":"8", resizable: false, draggable: false, static: false},
        {"x":18,"y":8,"w":2,"h":3,"i":"9", resizable: false, draggable: false, static: false},
        {"x":20,"y":8,"w":2,"h":3,"i":"10", resizable: false, draggable: false, static: false},

        {"x":0,"y":11,"w":2,"h":3,"i":"12:00", resizable: false, draggable: false, static: false},
        {"x":2,"y":11,"w":2,"h":3,"i":"1", resizable: false, draggable: false, static: false},
        {"x":4,"y":11,"w":2,"h":3,"i":"2", resizable: false, draggable: false, static: false},
        {"x":6,"y":11,"w":2,"h":3,"i":"3", resizable: false, draggable: false, static: false},
        {"x":8,"y":11,"w":2,"h":3,"i":"4", resizable: false, draggable: false, static: false},
        {"x":10,"y":11,"w":2,"h":3,"i":"5", resizable: false, draggable: false, static: false},
        {"x":12,"y":11,"w":2,"h":3,"i":"6", resizable: false, draggable: false, static: false},
        {"x":14,"y":11,"w":2,"h":3,"i":"7", resizable: false, draggable: false, static: false},
        {"x":16,"y":11,"w":2,"h":5,"i":"再", resizable: false, draggable: false, static: true},
        {"x":18,"y":11,"w":2,"h":3,"i":"9", resizable: false, draggable: false, static: false},
        {"x":20,"y":11,"w":2,"h":3,"i":"10", resizable: false, draggable: false, static: false},

        {"x":0,"y":14,"w":2,"h":3,"i":"13:00", resizable: false, draggable: false, static: false},
        {"x":2,"y":14,"w":2,"h":3,"i":"1", resizable: false, draggable: false, static: false},
        {"x":4,"y":14,"w":2,"h":3,"i":"2", resizable: false, draggable: false, static: false},
        {"x":6,"y":14,"w":2,"h":3,"i":"3", resizable: false, draggable: false, static: false},
        {"x":8,"y":14,"w":2,"h":3,"i":"4", resizable: false, draggable: false, static: false},
        {"x":10,"y":14,"w":2,"h":3,"i":"5", resizable: false, draggable: false, static: false},
        {"x":12,"y":14,"w":2,"h":3,"i":"6", resizable: false, draggable: false, static: false},
        {"x":14,"y":14,"w":2,"h":3,"i":"7", resizable: false, draggable: false, static: false},
        {"x":16,"y":14,"w":2,"h":1,"i":"8", resizable: false, draggable: false, static: false},
        {"x":18,"y":14,"w":2,"h":3,"i":"9", resizable: false, draggable: false, static: false},
        {"x":20,"y":14,"w":2,"h":3,"i":"10", resizable: false, draggable: false, static: false},

        {"x":0,"y":17,"w":2,"h":3,"i":"14:00", resizable: false, draggable: false, static: false},
        {"x":2,"y":17,"w":2,"h":3,"i":"1", resizable: false, draggable: false, static: false},
        {"x":4,"y":17,"w":2,"h":3,"i":"2", resizable: false, draggable: false, static: false},
        {"x":6,"y":17,"w":2,"h":3,"i":"3", resizable: false, draggable: false, static: false},
        {"x":8,"y":17,"w":2,"h":3,"i":"4", resizable: false, draggable: false, static: false},
        {"x":10,"y":17,"w":2,"h":3,"i":"5", resizable: false, draggable: false, static: false},
        {"x":12,"y":17,"w":2,"h":3,"i":"6", resizable: false, draggable: false, static: false},
        {"x":14,"y":17,"w":2,"h":3,"i":"7", resizable: false, draggable: false, static: false},
        {"x":16,"y":17,"w":2,"h":3,"i":"8", resizable: false, draggable: false, static: false},
        {"x":18,"y":17,"w":2,"h":3,"i":"9", resizable: false, draggable: false, static: false},
        {"x":20,"y":17,"w":2,"h":3,"i":"10", resizable: false, draggable: false, static: false},

    ];

    export default {
        name: 'app',
        components: {
            // ResponsiveGridLayout,
            GridLayout,
            GridItem,
            TestElement,
            CustomDragElement,
            SliderTable,
        },
        data () {
            return {
                //layout: JSON.parse(JSON.stringify(testLayout)),
                layout: JSON.parse(JSON.stringify(testLayout)),
                draggable: true,
                resizable: true,
                mirrored: false,
                responsive: true,
                preventCollision: false,
                rowHeight: 30,
                colNum: 32,
                index: 0
            }
        },
        mounted: function () {
            this.index = this.layout.length;
        },
        methods: {

            clicked: function(event, x, y) {
                console.log("click i=" + event);
                //var targetId = event.currentTarget.id;
                window.alert("CLICK!" + "(" + x + "," + y + ")" );
            },

            increaseWidth: function() {
                let width = document.getElementById("content").offsetWidth;
                width += 20;
                document.getElementById("content").style.width = width+"px";
            },
            decreaseWidth: function() {
                let width = document.getElementById("content").offsetWidth;
                width -= 20;
                document.getElementById("content").style.width = width+"px";
            },
            removeItem: function(item) {
                //console.log("### REMOVE " + item.i);
                this.layout.splice(this.layout.indexOf(item), 2);
            },
            addItem: function() {
                // let self = this;
                //console.log("### LENGTH: " + this.layout.length);
                //newX = 0;
                //newY = 0;
                let item = {"x":3,"y":2,"w":2,"h":2,"i":this.index+"", whatever: "bbb"};
                this.index++;
                this.layout.push(item);
            },
            move: function(i, newX, newY){
                console.log("MOVE i=" + i + ", X=" + newX + ", Y=" + newY);
            },
            resize: function(i, newH, newW, newHPx, newWPx){
                console.log("RESIZE i=" + i + ", H=" + newH + ", W=" + newW + ", H(px)=" + newHPx + ", W(px)=" + newWPx);
            },
            moved: function(i, newX, newY){
                console.log("### MOVED i=" + i + ", X=" + newX + ", Y=" + newY);
            },
            resized: function(i, newH, newW, newHPx, newWPx){
                console.log("### RESIZED i=" + i + ", H=" + newH + ", W=" + newW + ", H(px)=" + newHPx + ", W(px)=" + newWPx);
            },
            containerResized: function(i, newH, newW, newHPx, newWPx){
                console.log("### CONTAINER RESIZED i=" + i + ", H=" + newH + ", W=" + newW + ", H(px)=" + newHPx + ", W(px)=" + newWPx);
            },
            /**
             * Add change direction button
             */
            changeDirection() {
                let documentDirection = getDocumentDir();
                let toggle = "";
                if (documentDirection === "rtl") {
                    toggle = "ltr"
                } else {
                    toggle = "rtl"
                }
                setDocumentDir(toggle);
                //eventBus.$emit('directionchange');
            },

            layoutCreatedEvent: function(newLayout){
                console.log("Created layout: ", newLayout)
            },
            layoutBeforeMountEvent: function(newLayout){
                console.log("beforeMount layout: ", newLayout)
            },
            layoutMountedEvent: function(newLayout){
                console.log("Mounted layout: ", newLayout)
            },
            layoutReadyEvent: function(newLayout){
                console.log("Ready layout: ", newLayout)
            },
            layoutUpdatedEvent: function(newLayout){
                console.log("Updated layout: ", newLayout)
            },

        },
    }
</script>

<style>
    /*    #app {
            font-family: 'Avenir', Helvetica, Arial, sans-serif;
            -webkit-font-smoothing: antialiased;
            -moz-osx-font-smoothing: grayscale;
            text-align: center;
            color: #2c3e50;
            margin-top: 60px;
        }

        h1, h2 {
            font-weight: normal;
        }

        ul {
            list-style-type: none;
            padding: 0;
        }

        li {
            display: inline-block;
            margin: 0 10px;
        }

        a {
            color: #42b983;
        }*/
</style>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /*text-align: center;*/
  color: #2c3e50;
  /*margin-top: 60px;*/
}
</style>
