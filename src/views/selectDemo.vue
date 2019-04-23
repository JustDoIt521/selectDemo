<template>
    <div id="selectDemo">
        <ul>
            <li v-for="(item,index) in nodes" :key="index">
                <div
                    class="listName"
                    @click="changeList(item, index)"
                >
                    <span :class="{'clicked':item.isShowChild}" 
                    >
                        {{item.name}}
                    </span>
                    <div v-show="item.nodes &&item.nodes.length > 0" style="position:absolute;right:1rem;height:100%;">
                        <img src="@/images/right.png" v-show="!item.isShowChild"/>
                        <img src="@/images/down.png" v-show="item.isShowChild"/>
                    </div>
                </div>
                <div class="children" 
                 v-show="item.isShowChild && item.nodes && item.nodes.length > 0">
                    <selectDemo :nodes = "item.nodes"></selectDemo>
                </div>
            </li>
        </ul>
    </div>
</template>
<script>
import selectDemo from "@/views/selectDemo"
import BUS from "@/views/BUS.js"
export default {
    name: "selectDemo",
    data () {
        return {
            isShowList: false
        }
    },
    components:{
        selectDemo
    },
    props:[
        "nodes"
    ],
    mounted(){
    },
    methods: {
        changeList: function(item,index) {
            if(item.hasOwnProperty("isShowChild")){
                item.isShowChild = !item.isShowChild;
            } else {
                this.$set(item, 'isShowChild', true);
            }
            if(!item.isShowChild){
                this.childStatus(item);
                console.warn(item)
            }
            if(item.isShowChild){
                for(let i = 0; i < this.nodes.length; i++){
                    if(this.nodes[i].hasOwnProperty("isShowChild") && this.nodes[i].isShowChild && index !== i){
                        this.nodes[i].isShowChild = false;
                        this.childStatus(this.nodes[i]);
                        break;
                    }
                }
            }
            BUS.$emit("getValue", item);
        },
        //如果子列表的子列表被展开，点击父列表关闭时，子列表的子列表也应该被关闭 
        childStatus: function(item){
            if(!item.nodes || item.nodes.length <1){
                return;
            }
            for(let i = 0; i < item.nodes.length; i++){
                // 因为每一级列表只会有一个展开项，因此只需要找到展开项 递归即可
                if(item.nodes[i].isShowChild){
                    item.nodes[i].isShowChild = false;
                    this.childStatus(item.nodes[i]);
                    break;
                }
            }
        }
    },
}
</script>
<style lang="less">
    #selectDemo{
        position: relative;
        ul{
            width: 10rem;
            margin: 0;
            padding:0;
            position:absolute;
            box-shadow: 0 2px 12px 0 rgba(0,0,0,.1);
            border:1px solid #DCDCDC;
            border-radius: 0.1rem;
        }
        ul li{
            position: relative;
            width:10rem;
            list-style: none;
            text-align: center;
            line-height:2rem;
            margin: 0;
            box-sizing: border-box;
            padding: 0 0 0 2rem;
            display: flex;
            flex-direction: row;
            img{
                width:1rem;
                height:1rem;
            }
        }
        ul li:hover{
            background: #eeeeee;
        }
        .clicked{
            color:#1E90FF;
        }
        .listName{
            display:flex;
            width:100%;
            cursor: pointer;
        }
        .children{
            position: absolute;
            right:0px;
            height:0px;
            padding:0px;
        }
        
    }
</style>


