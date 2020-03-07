<template>
    <div class="relative pr-10">
      <button class="delete_btn delete_btn_small" v-show="delete_item_true" @click="$emit('delete_scale_item',id)">x</button>
      <div class="wrapper">
        <input v-model="measurement" placeholder="Measurement" class="input_field" />
        <span class="mx-2">{{ measured_unit || '' }}</span>
        <span class="mx-2">=</span>
        <span class="output">{{ output || '' }}</span>
        <span>{{ actual_unit || '' }}</span>
      </div>
  </div>
</template>

<script>
var sig_figs = 3;

export default {
    props: [ 'scale_item_id', 'scale', 'measured_unit', 'actual_unit', 'delete_item_true' ],
    
    data(){
      return {
        measurement: null,
        id: null
      }
    },

    created(){
      this.id = this.scale_item_id;
    },
    
    computed: {
        output() {
          let value = this.measurement*this.scale;
          if (String(value).replace('.','').length > sig_figs){
            return Number(value).toPrecision(sig_figs);
          }
            return value;
        }
    }
};
</script>

<style>
.wrapper {
  display: grid;
  grid-template-columns: 3fr 0.2fr 0.2fr 1.5fr 0.2fr;
  align-items: center;
}
.delete_btn_small {
  position: absolute;
  right: 5px;
  top: 6px;
}
</style>