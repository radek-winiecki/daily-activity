<template>
  <div class="max-w-screen-sm mx-auto px-4 py-10">
    <!-- App Message -->
    <div v-if="statusMsg || errorMsg" class="mb-10 p-4 rounded-md shawod-md bg-light-grey">
      <p class="text-at-light-green">{{ statusMsg }}</p>
      <p class="text-red-500">{{ errorMsg }}</p>
    </div>

    <div v-if="dataLoaded">
      <!-- General Workout Info -->
      <div class="flex flex-col items-center p-8 rounded-md shadow-md bg-light-grey relative">
        <div v-if="user" class="flex absolute left-2 top-2 gap-x-2">
          <div @click="editMode" class="h-7 w-7 rounded-full flex justify-center items-center cursor-pointer bg-at-light-green shadow-lg">
            <img class="h-3.5 w-auto" src="@/assets/images/pencil-light.png" alt="pencil-light">
          </div>

          <div class="h-7 w-7 rounded-full flex justify-center items-center cursor-pointer bg-at-light-green shadow-lg">
            <img class="h-3.5 w-auto" src="@/assets/images/trash-light.png" alt="pencil-light">
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, computed } from 'vue';
import { supabase } from '../supabase/init';
import { useRoute } from 'vue-router';
import store from "../store/index"

export default {
  name: "view-workout",
  setup() {
    // Create data / vars
    const data = ref(null);
    const dataLoaded = ref(null);
    const errorMsg = ref(null);
    const statusMsg = ref(null);
    const route = useRoute();
    const user = computed(() => store.state.user);

    // Get current Id of route
    const currentId = route.params.workoutId;

    // Get workout data
    const getData = async () => {
      try {
        const { data: workouts, error } = await supabase
          .from("workouts")
          .select("*")
          .eq("id", currentId);
        if (error) throw error;
        data.value = workouts[0];
        dataLoaded.value = true;
        console.log(data.value);
      } catch (error) {
        errorMsg.value = error.message;
        setTimeout(() => {
          errorMsg.value = false;
        }, 5000);
      }
    };
    getData();

    // Delete workout

    // Edit mode
    const edit = ref(null);

    const editMode = () => {
      edit.value = !edit.value;
    }

    // Add exercise

    // Delete exercise

    // Update Workout

    return { statusMsg, data, dataLoaded, errorMsg, editMode, user };
  },
};
</script>
