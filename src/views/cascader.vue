<template>
    <div id="cascader">
        <input v-bind:value="value" 
        v-on:input ="handleInput" readonly/>
        <selectDemo :nodes="list" ></selectDemo>
    </div>
</template>
<script>
import selectDemo from '@/views/selectDemo.vue'
import BUS from "@/views/BUS.js"
export default {
    name: "cascader",
    components: {
        selectDemo
    },
    data(){
        return {
            path: ""
        }
    },
    props: [
        "value",
        "list"
    ],
    mounted() {
        BUS.$on("getValue", val => {
            this.path = val.name;
            this.$emit("change", val);
            this.$emit('input', val.name);
        })
    },
    methods: {
        handleInput: function(event){
            var value = event.target.value;
            this.$emit('input',value);
        }
    }
}
</script>
<style lang="less">
    #cascader{
        display: flex;
        flex-direction: column;
        width:10rem;
        margin-left: auto;
        margin-right: auto;
        input{
            width: 10rem;
            line-height:1.8rem;
            border-radius: 0.2rem;
            border: 1px solid #DCDCDC;
            box-sizing: border-box;
            padding: 0 0 0 5px;
            margin-bottom: 5px;
            font-size: 15px;
        }
    }
</style>


