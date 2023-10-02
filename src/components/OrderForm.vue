
<script setup>
import OrderFormItem from './OrderFormItem.vue'
import { ref, reactive, computed, onMounted } from 'vue';
import uniqueId from "lodash.uniqueid"
const valid = ref(false)
const dialog = ref(false)
const detailedInvoice = ref(true)
const isDialogResult = ref(false)
const dialogAlert = ref(false)
const dialogDeleteAll = ref(false)
const orderItems = ref({
  // uniqueId('order-'): {price:0},
  // 'order-0':{order:"蔡", name:"哈哈", price:123, note:"加飯"},
  // 'order-1':{order:"蔡蔡", name:"我不是哈哈", price:456},
})

onMounted(()=>{
  orderItems[uniqueId("order-")] = {}
})

// caculate amount of all items then call for restaurant 
const orderResult = computed(() =>{
  let total = {}
  for (let key in orderItems.value){
    let order_name = orderItems.value[key]['order']
    if (Object.hasOwn(total, order_name)){
      total[order_name] += 1
    }
    else{
      total[order_name] = 1
    }
  }

  let result = ''
  for(let key in total){
    result += (key + '*' + total[key] + '<br>')
  }
  return result
})

// for people who order know how much should pay
const orderInvoice = computed(()=>{
  let result = ''
  for(let key in orderItems.value){
    let i = orderItems.value[key]
    result += (i.name + ' ')
    result += (i.price + ' ')
    result += (detailedInvoice.value) ? i.order + (i.note || '') : ''
    result += '<br>'
  }
  return result
})

function addOrder(){
  orderItems.value[uniqueId('order-')] = {}
}

function deleteAll(){
  dialogDeleteAll.value = false
  orderItems.value = {}
  orderItems.value[uniqueId("order-")] = {}
}

function changeOrder(order_id, new_data){
  Object.assign(orderItems.value[order_id], new_data)
}

function deleteOrder(order_id){
  delete orderItems.value[order_id]
}

function copyText(){
  let text = ((isDialogResult.value) ? orderResult.value : orderInvoice.value).replace(/<br>/g, "\n") 
  navigator.clipboard.writeText(text)
  dialogAlert.value = true 
}
// export default {
//   data: () => ({
//     valid: false,
//     firstname: '',
//     lastname: '',
//     nameRules: [
//       value => {
//         if (value) return true

//         return 'Name is required.'
//       },
//       value => {
//         if (value?.length <= 10) return true

//         return 'Name must be less than 10 characters.'
//       },
//     ],
//     email: '',
//     emailRules: [
//       value => {
//         if (value) return true

//         return 'E-mail is requred.'
//       },
//       value => {
//         if (/.+@.+\..+/.test(value)) return true

//         return 'E-mail must be valid.'
//       },
//     ],
//   }),
// }
</script>
<template>
    <v-form v-model="valid" class="w-75" @submit.prevent="">
      <v-container class="pa-2 ma-1">
        <v-row>
          <v-col>
            餐點   
          </v-col>
          <v-col>
            訂的人
          </v-col>
          <v-col>
            價錢
          </v-col>
          <v-col>
            備註
          </v-col>
          <v-col>
            <v-btn variant="outlined">
              刪除全部
              <v-dialog
                v-model="dialogDeleteAll"
                activator="parent"
                width="auto"
              >
                <v-alert
                  type="warning"
                >
                  確定要刪除全部的訂單嗎?
                  <v-spacer class="mt-6 justify-space-around">
                    <v-btn @click="deleteAll" class="mx-2">是</v-btn>
                    <v-btn color="primary" class="mx-2" @click="dialogDeleteAll = false">否</v-btn>
                  </v-spacer>
                </v-alert>
              </v-dialog>
            </v-btn>
          </v-col>
        </v-row>
        <div v-for="(item, key, index) in orderItems" :key="key">
          <order-form-item 
            :id="key" 
            @order_change="changeOrder" 
            @delete_item="deleteOrder"
            />
        </div>
        
        <v-row>
          <v-col>
            <v-btn @click="addOrder" variant="outlined">新增</v-btn>
          </v-col>
          <v-col></v-col>
          <v-col></v-col>
          <v-col></v-col>
          <v-col>
          <v-dialog
            v-model="dialog"
            width="800"
          >
            <template v-slot:activator="{ props }">
              <v-btn
                color="primary"
                v-bind="props"
              >
                匯出整理
              </v-btn>
            </template>
            <v-card>
              <v-card-title>
                <span class="text-h5">Result</span>
              </v-card-title>
              <v-card-text v-html="orderInvoice"></v-card-text>
              <v-alert
                v-model="dialogAlert"
                variant="tonal"
                closable
                close-label="Close Alert"
                type="info"
                title="文字已複製!"
              ></v-alert>
              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn
                  color="primary"
                  variant="outlined"
                  @click="copyText"
                >
                  複製文字
                </v-btn>
                <v-btn
                  color="default"
                  variant="outlined"
                  @click="dialog = false; dialogAlert=false"
                >
                  關閉
                </v-btn>
              </v-card-actions>
            </v-card>
          </v-dialog>
          </v-col>
        </v-row>
      </v-container>
    </v-form>
  </template>