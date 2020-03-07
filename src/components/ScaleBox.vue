<template>
  <div class="card px-4 py-2 mx-auto my-4 rounded shadow-sm border-2 border-gray-300">
    <div class="relative">
      <p class="text-2xl text-center">Scale Set {{ num }}</p>
      <button class="absolute right-0 top-0 delete_btn" v-show="delete_box_true" @click="$emit('delete_me',id)">X</button>
    </div>

    <div class="px-1">
      <input class="input_description" v-model="description" placeholder="Description" />
    </div>

    <div class="flex flex-wrap">
      <div class="w-1/2 px-1">
        <input v-model.number="measured" placeholder="Measured value" class="input_field" />
      </div>
      <div class="w-1/2 px-1">
        <input v-model="measured_unit" placeholder="Measured unit" class="input_field" />
      </div>
    </div>

    <div class="flex flex-wrap">
      <div class="w-1/2 px-1">
        <input v-model.number="actual" placeholder="Actual value" class="input_field" />
      </div>
      <div class="w-1/2 px-1">
        <input v-model="actual_unit" placeholder="Actual unit" class="input_field" />
      </div>
    </div>

    <div class="flex justify-center mb-6 mt-4 text-xl">
        <div class="px-4 text-center bg-gray-200 py-1 rounded-lg">
          <span>Scale:</span>
          <span class="mx-6">{{ scale }}</span>
          &nbsp;
          <span>{{ scale_unit }}</span>
      </div>
    </div>

    <hr class="mb-8" />

    <ScaleItem
        v-for="(item,index) in scale_items"
        :key="scale_item_ids[index]"
        :scale_item_id="scale_item_ids[index]"
        :scale="scale"
        :measured_unit="measured_unit"
        :actual_unit="actual_unit"
        :delete_item_true="delete_item_true"
        @delete_scale_item="delete_scale_item"
    ></ScaleItem>

    <button
      class="text-l block bg-white hover:bg-gray-200 active:bg-gray-300 text-gray-800 font-semibold px-4 border border-gray-400 rounded shadow mx-auto mt-2"
      @click="add_scale_item"
    >+</button>
  </div>
</template>

<script>
import ScaleItem from "@/components/ScaleItem.vue";

var sig_figs = 4;

export default {
  name: "ScaleBox",
  
  components: { ScaleItem },

  props: [ 'id', 'count', 'delete_box_true'],
  
  data() {
    return {
      num: null,
      description: '',
      measured: null,
      actual: null,
      measured_unit: '',
      actual_unit: '',
      scale_items: [],
      scale_item_ids: [],
      local_count: 0
    };
  },

  created(){
    this.num = this.count;
  },

  mounted(){
    this.add_scale_item();
  },

  computed: {
    scale() {
      if (typeof(this.actual) == 'number' && typeof(this.measured) == 'number') {
        let value = this.actual/this.measured;
        if (String(value).replace('.','').length > sig_figs) {
          return Number(value).toPrecision(sig_figs);
        } else {
          return this.actual/this.measured;
        }
      } else {
        return '--'
      }
    },

    scale_unit() {
      if (this.actual_unit && this.measured_unit) {
        return this.actual_unit + '/' + this.measured_unit;
      } else {
        return '';
      }
    },

    delete_item_true() { return this.scale_items.length > 1 }
  },
  
  methods: {
      add_scale_item() {
        this.local_count += 1;
        this.scale_item_ids.push(this.local_count);
        this.scale_items.push(ScaleItem);
      },

      delete_scale_item(id) {
        let index = this.scale_item_ids.findIndex(e => e == id);
        this.scale_items.splice(index,1);
        this.scale_item_ids.splice(index,1);
      }
  }
}
</script>

<style>
.card {
  width: 350px;
}
</style>
