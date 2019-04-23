<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png">
    <cascader :list = "list" @change="refresh" v-model="testOne"></cascader>
  </div>
</template>

<script>
import cascader from '@/views/cascader.vue'

export default {
  name: 'home',
  data () {
    return {
      testOne: "",
      list: [
        {
          name: 'child_1',
          nodes:[]
        },
        {
          name: 'child_2',
          nodes: [
            {
              name: 'child_2_1',
              nodes: []
            },
            {
              name: 'child_2_2',
              nodes: []
            }
          ]
        },
        {
          name: 'child_3',
          nodes: [
            {
              name: 'child_3_1',
              nodes: [
                {
                  name: 'child_3_1_1',
                  nodes: []
                },
                {
                  name: 'child_3_1_2',
                  nodes: [
                    {
                      name: 'child_3_1_2_1',
                      nodes: []
                    },
                    {
                      name: 'child_3_1_2_2',
                      nodes: []            
                    }
                  ]
                }
              ]
            },
            {
              name: 'child_3_2',
              nodes: []
            }
          ]
        }
      ]
    }
  },
  components: {
    cascader
  },
  methods: {
    refresh: function(){
      console.log("refresh");
    },
    show: function(){
      console.log(this.testOne);
    }
  }
}
</script>
