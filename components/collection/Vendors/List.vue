<template>
    <!-- start of showing vendor in the table -->
    <div class="overflow-auto">
       <table class="shadow-2xl border-2 border-gray-800 w-full">
        <thead class="bg-gray-800 text-white text-left">
            <tr>
                <th class="py-3">Category Name</th>
                <th class="py-3">Industry</th>
                <th class="py-3">Name</th>
                <th class="py-3">Email</th>
                <th class="py-3">Edit Action</th>
                <th class="py-3">Delete Action</th>
            </tr>
        </thead>
        <tbody class="divide-y  divide-gray-800">
            <tr v-for="(vendor,index) in VendorUrlData" :key="vendor" class="text-left cursor-pointer">
              <td class="py-3 pr-6 whitespace-nowrap">
               <textarea v-if="editVendorIndex===index" v-model="editVendorForm.category" class="focus:border-purple-500" cols="10" rows="1">{{vendor.category}}</textarea>
                <p v-else>{{vendor.category}}</p>
              </td>
              <td class="py-3 pr-6 whitespace-nowrap">
               <textarea v-if="editVendorIndex===index" v-model="editVendorForm.industry" class="focus:border-purple-500" cols="10" rows="1">{{vendor.industry}}</textarea>
                <p v-else>{{vendor.industry}}</p>
              </td>
              <td class="py-3 pr-6 whitespace-nowrap">
               <textarea v-if="editVendorIndex===index" v-model="editVendorForm.name" class="focus:border-purple-500" cols="10" rows="1">{{vendor.name}}</textarea>
                <p v-else>{{vendor.name}}</p>
              </td>
              <td class="py-3 pr-6 whitespace-nowrap">
               <textarea v-if="editVendorIndex===index" v-model="editVendorForm.email" class="focus:border-purple-500" cols="10" rows="1">{{vendor.email}}</textarea>
                <p v-else>{{vendor.email}}</p>
              </td> 
              <td class="py-3 pr-6 whitespace-nowrap">
                <CollectionVendorsEdit @edit="editVendor(vendor,index)"/>
              </td>
              <td class="py-3 pr-6 whitespace-nowrap">
                <CollectionVendorsDelete @delete="deleteVendor(vendor)"/>
              </td>
            </tr>
        </tbody>
       </table>
          
    </div>
     <!-- End of showing vendor in the table -->
</template>
<script setup lang="ts">
//Defining the schema of vendorschema
interface VendorSchema{
 category:string,
 industry:string,
 name:string,
 email:string,

}
// Getting the props of vendorurlData
let props = defineProps<{ VendorUrlData:VendorSchema }>()
const emits = defineEmits(['edit','delete']);
const editVendorIndex=ref("0")


//Defining the schema of reactive vendorEditForm
interface VendorEditForm {
    category:string,
 industry:string,
 name:string,
 email:string,
 
}

//Declaring in the empty form and use it in v-model
const editVendorForm:VendorEditForm=reactive({
category:'',
industry:'',
name:'',
email:'',
})

//Emitting the delete and pass it in the main
const deleteVendor=(vendor:any)=>{
   emits('delete',vendor)
 
}
//Emitting the edit and pass it in the main
const editVendor=(vendor:any,index:any)=>{
   editVendorIndex.value=index
 emits('edit',vendor,editVendorForm)
}
</script>