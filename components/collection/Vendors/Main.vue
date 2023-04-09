<template>
    <div>
        <!-- start of Floor -->
       <div class="p-[50px] flex divide-y-2 border-y">
           
        <h1>Vendors</h1>
        <!-- start of add button -->
        <div class="ml-[56rem]">
            <button class="flex bg-slate-100" @click="openVendor">
            <PlusIcon class="h-6 w-6"/>
            <p class="ml-3">Add Vendors</p>
            </button>

        </div>
          <!-- end of add button -->
            <!-- opening the Floor modal by clicking add Floor -->
        <CollectionVendorsAdd v-show="isVendor" @cancel="isVendor=false" @save="saveVendor"/>
       </div>
    <div class="text-center" v-if="!isVendor">
        <h1 class="text-3xl">vendors List</h1>
         <div>
            <CollectionVendorsList   :VendorUrlData="VendorUrlData" @delete="deleteVendorListener" @edit="editVendorListener"/>
        </div>
      </div>
    </div> 
     <!-- End of Floor -->
</template>
<script setup lang="ts">
//importing the HeroIcon and  useAuthLazyFetch
import { PlusIcon } from "@heroicons/vue/24/outline"
import {useAuthLazyFetch}  from "../../../composables/useAuthLazyFetch"




//Define the schema of VendorUrl
interface VendorUrlSchema{
    VendorUrl:string,
    url:string
}
// Get the props of VendorUrl and url
const props = withDefaults(defineProps<VendorUrlSchema>(), {
    VendorUrl: "",
    url:""
})

//Showing the dynamic Floor in the table
const getVendorUrl = async () => {
    try {
    const { data: VendorData } = await useAuthLazyFetch(props.VendorUrl, {});
    return VendorData;
  } catch (error) {
    console.error("Error fetching tag URL", error);
    return null;
  }
}
const VendorUrlData = await getVendorUrl();

const isVendor=ref(false)
////opening the  Add model
const openVendor=()=>{
    isVendor.value=!isVendor.value
}
//saving the Vendor

const saveVendor=(addVendorForm)=>{

useAuthLazyFetchPost(`${props.url}/`, {
    body: {
    category:addVendorForm.category,
    industry:addVendorForm.industry,
    name:addVendorForm.name,
    email:addVendorForm.email,
    rating:0,
    type:"Vendor",
    address: {
    street: "string",
    city: "string",
    state: "string",
    country: "string",
    zip_code: "string"
  },
  website: "string",
  status: 1,
  profile_id: "string"
    }
  });
}

//Deleting the vendor
const deleteVendorListener=(vendor)=>{

if(confirm('Are you sure to delete this record?')){
    useAuthLazyFetchDelete(`${props.url}/${vendor.uid}`, {});
    let VendorUrlIndex=VendorUrlData.value.findIndex(item => item.uid === vendor.uid)
    if(VendorUrlIndex!==-1){
        VendorUrlData.value.splice(VendorUrlIndex,1)
    }
}
}
// //Editing the vendor

const editVendorListener=(vendor,editVendorForm)=>{
 useAuthLazyFetchPut(`${props.url}/${vendor.uid}?category=${editVendorForm.category}&industry=${editVendorForm.industry}&name=${editVendorForm.name}&email=${editVendorForm.email}`, {
    body: {
        category:editVendorForm.category,
    industry:editVendorForm.industry,
    name:editVendorForm.name,
    email:editVendorForm.email,
    rating:vendor.rating,
    type:vendor.type,
    address: {
    street: vendor.street,
    city: vendor.city,
    state: vendor.state,
    country: vendor.country,
    zip_code: vendor.zip_code
  },
  website: vendor.website,
  status: vendor.status,
  profile_id: vendor.profile_id
    },
  });
}
</script>