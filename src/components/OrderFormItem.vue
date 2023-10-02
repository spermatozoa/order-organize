<script setup>
import { ref, reactive, watch } from 'vue';

const props = defineProps({
  id: String
})

const order = ref("")
const name = ref("")
const price = ref(0)
const note = ref("")

const order_list = reactive(['沙茶牛肉炒飯', '鮭魚炒飯', '起司紅醬炒飯', '起司炒飯', '泰式塔香炒飯', '韓式泡菜炒飯', '麻辣炒飯'])
const name_list = reactive(['人名1', '人名2','人名3'])
const note_list = reactive(['加蛋', '加飯','加麵'])

const emit = defineEmits(['order_change', 'delete_item'])
watch(order, (new_order)=>{
  emit('order_change', props.id, {order:new_order})
})
watch(name, (_new)=>{
  emit('order_change', props.id, {name:_new})
})
watch(price, (_new)=>{
  emit('order_change', props.id, {price:_new})
})
watch(note, (_new)=>{
  emit('order_change', props.id, {note:_new})
})

function deleteItem(){
  emit('delete_item', props.id)
}
</script>
<template>
  <v-divider
    :thickness="2"
    class="border-opacity-50 mt-1 mb-2"
  ></v-divider>
  <v-row class="mt-1 mb-n5">
    <v-col>
      <v-combobox
        label="菜名"
        :items="order_list"
        variant="outlined"
        v-model.lazy="order"
      ></v-combobox>
    </v-col>

    <v-col>
      <v-combobox
        label="姓名"
        :items="name_list"
        variant="outlined"
        v-model.lazy="name"
      ></v-combobox>
    </v-col>

    <v-col>
      <v-text-field 
        label="價錢" 
        type="number" 
        variant="outlined" 
        v-model.number.lazy="price"
      ></v-text-field>
    </v-col>

    <v-col>
      <v-combobox
        label="備註"
        :items="note_list"
        variant="outlined"
        v-model.lazy="note"
      ></v-combobox>
    </v-col>

    <v-col>
      <v-btn variant="outlined" @click="deleteItem">刪除</v-btn>
    </v-col>
  </v-row>
</template>
