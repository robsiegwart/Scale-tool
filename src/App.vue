<template>
  <div id="app">
    <div class="box_container">
      <ScaleBox
        v-for="(item,index) in scale_boxes"
        :key="ids[index]"
        :id="ids[index]"
        :count="count"
        :delete_box_true="delete_box_true"
        @delete_me="delete_"
      ></ScaleBox>
    </div>

    <button class="
        block
        text-xl
        text-white
        font-semibold
        bg-blue-700
        hover:bg-blue-600
        active:bg-blue-800
        py-2
        px-8
        mx-auto
        mt-2
        border
        border-gray-400
        rounded
        shadow
    "
      @click="add"
    >+</button>
  </div>
</template>

<script>
import ScaleBox from '@/components/ScaleBox.vue';

export default {
  name: 'App',

  components: {
    ScaleBox
  },

  data(){
    return {
      scale_boxes: [ ],
      ids: [],
      count: 0
    }
  },

  mounted(){
    this.add();
  },

  computed: {
    delete_box_true(){ return this.scale_boxes.length > 1; }
  },

  methods: {
    add(){
      this.count += 1;
      this.ids.push(this.count);
      this.scale_boxes.push(ScaleBox);
    },

    delete_(id){
      let index = this.ids.findIndex(e => e == id);
      this.ids.splice(index,1);
      this.scale_boxes.splice(index,1);
    },
  }
}
</script>


<style>
.box_container { display: flex; flex-flow: wrap; }
</style>