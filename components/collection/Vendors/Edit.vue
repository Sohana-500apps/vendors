<template>
    <!-- start of edit  -->
         <button @click="emitEditVendor">      
             <PencilIcon class="h-6 w-6"/>
         </button>
          <!-- end of edit  -->
 </template>
 <script setup lang="ts">
 //importing the Heroicon
  import { PencilIcon } from "@heroicons/vue/24/outline"
  const emits = defineEmits(['edit']);
//  Emitting the edit to the list
 const emitEditVendor=()=>{
    emits('edit')
 }
 </script>