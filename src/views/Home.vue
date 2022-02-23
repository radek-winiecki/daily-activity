<template>
  <div v-if="dataLoaded" class="container mt-10 px-4">
    
    <!-- No Data -->
    <div v-if="data.length === 0" class="w-full flex flex-col items-center">
      <h1 class="text-2xl">Jeszcze nic tutaj nie ma...</h1>
      <router-link
        class="mt-20 py-2 px-6 rounded-sm self-start text-sm text-white bg-at-light-green duration-200 border-solid
               border-2 border-transparent hover:border-at-light-green hover:bg-white hover:text-at-light-green"
       :to="{ name: 'create' }">Stwórz Trening</router-link>
    </div>
  </div>
</template>

<script>
import { ref } from "vue";
import { supabase } from "../supabase/init";

export default {
  name: "Home",
  components: {},
  setup() {
    // Create data / vars
    const data = ref([]);
    const dataLoaded = ref(null);

    // Get data
    const getData = async () => {
      try {
        const { data: workouts, error } = await supabase.from("workouts").select("*");
        if (error) throw error;
        data.value = workouts;
        dataLoaded.value = true;
      }
      catch(error) {
        console.warn(error.message);
      }
    };

    // Run data function
    getData();

    return { data, dataLoaded };
  },
};
</script>
