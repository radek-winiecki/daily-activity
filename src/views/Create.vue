<template>
  <div class="max-w-screen-md mx-auto px-4 py-10">
    <!-- Status Message -->
    <div v-if="statusMsg || errorMsg" class="mb-10 p-4 bg-light-grey rounded-md shadow-lg">
      <p class="text-at-light-green">{{ statusMsg }}</p>
      <p class="text-red-500">{{ errorMsg }}</p>
    </div>

    <!-- Create -->
    <div class="p-8 flex items-start bg-light-grey rounded-md shadow-lg">
      <!-- Form -->
      <form class="flex flex-col gap-y-5 w-full">

        <h1 class="text-2xl text-at-light-green">Zapisz Trening</h1>

        <!-- Workout Name -->
        <div class="flex flex-col">
          <label 
          for="workout-name" 
          class="mb-1 text-sm text-at-light-green">Nazwa Treningu
          </label>

          <input
           type="text" 
           required 
           class="p-2 text-grey-500 focus:outline-none" 
           id="workout-name" 
           v-model="workoutName"
           >
        </div>

        <!-- Workout Type -->
        <div class="flex flex-col">
          <label 
            for="workout-type" 
            class="mb-1 text-sm text-at-light-green">Typ Treningu
          </label>

          <select 
            id="workout-type" 
            class="p-2 text-grey-500 focus:outline-none" 
            required 
            v-model="workoutType">

            <option value="select-workout">Wybierz Trening</option>
            <option value="strength">Trening Siłowy</option>
            <option value="cardio">Kardio</option>
          </select>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
import { ref } from "vue";
export default {
  name: "create",
  setup() {
    // Create data
    const workoutName = ref("");
    const workoutType = ref("select-workout");
    const excercises = ref();
    const statusMsg = ref(null);
    const errorMsg = ref(null);

    // Add exercise

    // Delete exercise

    // Listens for chaging of workout type input

    // Create workout

    return { workoutName, workoutType, excercises, statusMsg, errorMsg };
  },
};
</script>
