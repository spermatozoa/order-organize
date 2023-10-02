<template>
  <v-layout class="rounded rounded-md">
    <v-navigation-drawer>
      <v-list>
        <v-list-item title="Navigation drawer"></v-list-item>
      </v-list>
    </v-navigation-drawer>
    
    <v-app-bar title="訂餐整理器">
    </v-app-bar>

    <v-main class="d-flex align-center justify-center" style="min-height: 300px;">
      <order-form/>
    </v-main>
  </v-layout>
</template>
<script setup>
import OrderForm from '@/components/OrderForm.vue';
</script>
