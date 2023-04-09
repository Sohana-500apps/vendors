<template>
    <!-- start of delete -->
    <button id="deletevendorfield"  @click="emitDeleteEdit">
        <TrashIcon class="h-6 w-6"/>
    </button>
     <!-- end of delete -->
</template>
<script setup lang="ts">
// importing the HeroIcon
import { TrashIcon } from "@heroicons/vue/24/outline"
const emits = defineEmits(['delete']);
//  Emitting the Delete to the list
const  emitDeleteEdit=()=>{
emits('delete')
}
</script>