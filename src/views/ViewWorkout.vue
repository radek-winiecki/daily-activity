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

          <div @click="deleteWorkout" class="h-7 w-7 rounded-full flex justify-center items-center cursor-pointer bg-at-light-green shadow-lg">
            <img class="h-3.5 w-auto" src="@/assets/images/trash-light.png" alt="trash-light">
          </div>
        </div>

        <img 
          v-if="data.workoutType === 'cardio'" 
          class="h-28 w-auto" 
          src="@/assets/images/running.png" 
          alt="running"
        />

        <img 
          v-else-if="data.workoutType === 'strength'" 
          class="h-28 w-auto" 
          src="@/assets/images/dumbbell.png" 
          alt="dumbbell"
        />

        <img 
          v-else-if="data.workoutType === 'stretching'" 
          class="h-28 w-auto" 
          src="@/assets/images/stretching.png" 
          alt="stretching"
        />

        <img 
          v-else-if="data.workoutType === 'cycling'" 
          class="h-28 w-auto" 
          src="@/assets/images/cycling.png" 
          alt="cycling"
        />

        <img 
          v-else-if="data.workoutType === 'swimming'" 
          class="h-28 w-auto" 
          src="@/assets/images/swimming.png" 
          alt="swimming"
        />

        <span class="mt-6 py-1.5 px-5 text-xs text-white bg-at-light-green rounded-lg shadow-md">{{ data.workoutType }}</span>

        <div class="w-full mt-6">
          <input v-if="edit" type="text" v-model="data.workoutName" class="p-2 w-full text-grey-500 focus:outline-none">

          <h1 v-else class="text-at-light-green text-2xl text-center">{{ data.workoutName }}</h1>
        
        </div>
      </div>

      <!-- Exercises -->
      <div class="mt-10 p-8 rounded-md flex flex-col item-center bg-light-grey shadow-md">

        <!-- Strength Training -->
        <div v-if="data.workoutType === 'strength'" class="flex flex-col gap-y-4 w-full">
          <div class="flex flex-col gap-x-6 gap-y-2 relative sm:flex-row" v-for="(item, index) in data.exercises" :key="index">
            <div class="flex flex-2 flex-col md:w-1/3">
            <label for="exercise-name" class="mb-1 text-sm text-at-light-green">Ćwiczenie</label>
              <input 
                v-if="edit" 
                id="exercise-name"
                class="p-2 w-full text-gray-500 focus:outline-none" 
                type="text" 
                v-model="item.exercise"
              />
              <p v-else>{{ item.exercise }}</p>
            </div>

            <div class="flex flex-1 flex-col">
            <label for="sets" class="mb-1 text-sm text-at-light-green">Serie</label>
              <input 
                v-if="edit" 
                id="sets"
                class="p-2 w-full text-gray-500 focus:outline-none" 
                type="text" 
                v-model="item.sets"
              />
              <p v-else>{{ item.sets }}</p>
            </div>

            <div class="flex flex-1 flex-col">
            <label for="reps" class="mb-1 text-sm text-at-light-green">Powtórzenia</label>
              <input 
                v-if="edit" 
                id="reps"
                class="p-2 w-full text-gray-500 focus:outline-none" 
                type="text" 
                v-model="item.reps"
              />
              <p v-else>{{ item.reps }}</p>
            </div>

            <div class="flex flex-1 flex-col">
            <label for="weight" class="mb-1 text-sm text-at-light-green">Ciężar (KG's)</label>
              <input 
                v-if="edit" 
                id="weight"
                class="p-2 w-full text-gray-500 focus:outline-none" 
                type="text" 
                v-model="item.weight"
              />
              <p v-else>{{ item.weight }}</p>
            </div>
            <img 
              v-if="edit"
              @click="deleteExercise(item.id)"
              class="absolute h-4 w-auto -left-5 cursor-pointer"
              src="@/assets/images/trash-light-green.png" 
              alt="trash-light-green"
            />
          </div>
          <button 
            v-if="edit"
            @click="addExercise"
            type="button" 
            class="mt-20 py-2 px-6 rounded-sm self-start text-sm text-white bg-at-light-green duration-200 border-solid
               border-2 border-transparent hover:border-at-light-green hover:bg-white hover:text-at-light-green">
              Dodaj Ćwiczenie
          </button>
        </div>

        <!-- Stretching Training -->
        <div v-else-if="data.workoutType === 'stretching'" class="flex flex-col gap-y-4 w-full">
          <div class="flex flex-col gap-x-6 gap-y-2 relative sm:flex-row" v-for="(item, index) in data.exercises" :key="index">
            <div class="flex flex-2 flex-col md:w-1/3">
            <label for="exercise-name" class="mb-1 text-sm text-at-light-green">Ćwiczenie</label>
              <input 
                v-if="edit" 
                id="exercise-name"
                class="p-2 w-full text-gray-500 focus:outline-none" 
                type="text" 
                v-model="item.exercise"
              />
              <p v-else>{{ item.exercise }}</p>
            </div>

            <div class="flex flex-1 flex-col">
            <label for="sets" class="mb-1 text-sm text-at-light-green">Serie</label>
              <input 
                v-if="edit" 
                id="sets"
                class="p-2 w-full text-gray-500 focus:outline-none" 
                type="text" 
                v-model="item.sets"
              />
              <p v-else>{{ item.sets }}</p>
            </div>

            <div class="flex flex-1 flex-col">
            <label for="reps" class="mb-1 text-sm text-at-light-green">Powtórzenia</label>
              <input 
                v-if="edit" 
                id="reps"
                class="p-2 w-full text-gray-500 focus:outline-none" 
                type="text" 
                v-model="item.reps"
              />
              <p v-else>{{ item.reps }}</p>
            </div>

            <div class="flex flex-1 flex-col">
            <label for="weight" class="mb-1 text-sm text-at-light-green">Ciężar (KG's)</label>
              <input 
                v-if="edit" 
                id="weight"
                class="p-2 w-full text-gray-500 focus:outline-none" 
                type="text" 
                v-model="item.weight"
              />
              <p v-else>{{ item.weight }}</p>
            </div>
            <img 
              v-if="edit"
              @click="deleteExercise(item.id)"
              class="absolute h-4 w-auto -left-5 cursor-pointer"
              src="@/assets/images/trash-light-green.png" 
              alt="trash-light-green"
            />
          </div>
          <button 
            v-if="edit"
            @click="addExercise"
            type="button" 
            class="mt-20 py-2 px-6 rounded-sm self-start text-sm text-white bg-at-light-green duration-200 border-solid
               border-2 border-transparent hover:border-at-light-green hover:bg-white hover:text-at-light-green">
              Dodaj Ćwiczenie
          </button>
        </div>

        <!-- Cardio Training -->
        <div v-else-if="data.workoutType === 'cardio'" class="flex flex-col gap-y-4 w-full">
          <div
            class="flex flex-col gap-x-6 gap-y-2 relative sm:flex-row"
            v-for="(item, index) in data.exercises"
            :key="index"
          >
            <div class="flex flex-2 flex-col md:w-1/3">
              <label for="cardioType" class="mb-1 text-sm text-at-light-green">
                Typ
              </label>
              <select
                id="cardioType"
                v-if="edit"
                class="p-2 w-full text-gray-500 focus:outline-none"
                type="text"
                v-model="item.cardioType"
              >
                <option value="#">Wybierz Typ</option>
                <option value="run">Bieganie</option>
                <option value="walk">Chodzenie</option>
              </select>
              <p v-else>{{ item.cardioType }}</p>
            </div>
            <div class="flex flex-1 flex-col">
              <label for="distance" class="mb-1 text-sm text-at-light-green">
                Dystans
              </label>
              <input
                v-if="edit"
                id="distance"
                class="p-2 w-full text-gray-500 focus:outline-none"
                type="text"
                v-model="item.distance"
              />
              <p v-else>{{ item.distance }}</p>
            </div>
            <div class="flex flex-1 flex-col">
              <label for="duration" class="mb-1 text-sm text-at-light-green">
                Czas Trwania
              </label>
              <input
                v-if="edit"
                id="duration"
                class="p-2 w-full text-gray-500 focus:outline-none"
                type="text"
                v-model="item.duration"
              />
              <p v-else>{{ item.duration }}</p>
            </div>
            <div class="flex flex-1 flex-col">
              <label for="pace" class="mb-1 text-sm text-at-light-green">
                Tempo
              </label>
              <input
                v-if="edit"
                id="pace"
                class="p-2 w-full text-gray-500 focus:outline-none"
                type="text"
                v-model="item.pace"
              />
              <p v-else>{{ item.pace }}</p>
            </div>
            <img
              @click="deleteExercise(item.id)"
              v-if="edit"
              class="absolute h-4 w-auto -left-5 cursor-pointer"
              src="@/assets/images/trash-light-green.png"
              alt=""
            />
          </div>
           <button
            @click="addExercise"
            v-if="edit"
            type="button"
            class="py-2 px-6 rounded-sm self-start text-sm text-white
            bg-at-light-green duration-200 border-solid border-2 border-transparent
            hover:border-at-light-green hover:bg-white hover:text-at-light-green"
          >
            Dodaj Ćwiczenie
          </button>
        </div>

        <!-- Swimming Training -->
        <div v-else-if="data.workoutType === 'swimming'" class="flex flex-col gap-y-4 w-full">
          <div
            class="flex flex-col gap-x-6 gap-y-2 relative sm:flex-row"
            v-for="(item, index) in data.exercises"
            :key="index"
          >
            <div class="flex flex-2 flex-col md:w-1/3">
              <label for="swimmingType" class="mb-1 text-sm text-at-light-green">
                Styl
              </label>
              <select
                id="swimmingType"
                v-if="edit"
                class="p-2 w-full text-gray-500 focus:outline-none"
                type="text"
                v-model="item.swimmingType"
              >
                <option value="#">Wybierz Styl</option>
                <option value="crawl">Kraul</option>
                <option value="frog">Żabka</option>
                <option value="back">Grzbietowy</option>
                <option value="butterfly">Motylkowy</option>
                <option value="dog">Piesek</option>
              </select>
              <p v-else>{{ item.swimmingType }}</p>
            </div>
            <div class="flex flex-1 flex-col">
              <label for="distance" class="mb-1 text-sm text-at-light-green">
                Dystans
              </label>
              <input
                v-if="edit"
                id="distance"
                class="p-2 w-full text-gray-500 focus:outline-none"
                type="text"
                v-model="item.distance"
              />
              <p v-else>{{ item.distance }}</p>
            </div>
            <div class="flex flex-1 flex-col">
              <label for="duration" class="mb-1 text-sm text-at-light-green">
                Czas Trwania
              </label>
              <input
                v-if="edit"
                id="duration"
                class="p-2 w-full text-gray-500 focus:outline-none"
                type="text"
                v-model="item.duration"
              />
              <p v-else>{{ item.duration }}</p>
            </div>
            <div class="flex flex-1 flex-col">
              <label for="pulse" class="mb-1 text-sm text-at-light-green">
                Tętno (BPM)
              </label>
              <input
                v-if="edit"
                id="pulse"
                class="p-2 w-full text-gray-500 focus:outline-none"
                type="text"
                v-model="item.pulse"
              />
              <p v-else>{{ item.pulse }}</p>
            </div>
            <img
              @click="deleteExercise(item.id)"
              v-if="edit"
              class="absolute h-4 w-auto -left-5 cursor-pointer"
              src="@/assets/images/trash-light-green.png"
              alt=""
            />
          </div>
           <button
            @click="addExercise"
            v-if="edit"
            type="button"
            class="py-2 px-6 rounded-sm self-start text-sm text-white
            bg-at-light-green duration-200 border-solid border-2 border-transparent
            hover:border-at-light-green hover:bg-white hover:text-at-light-green"
          >
            Dodaj Ćwiczenie
          </button>
        </div>

        <!-- Cycling Training -->
        <div v-else-if="data.workoutType === 'cycling'" class="flex flex-col gap-y-4 w-full">
          <div
            class="flex flex-col gap-x-6 gap-y-2 relative sm:flex-row"
            v-for="(item, index) in data.exercises"
            :key="index"
          >
            <div class="flex flex-2 flex-col md:w-1/3">
              <label for="cyclingType" class="mb-1 text-sm text-at-light-green">
                Typ Roweru
              </label>
              <select
                id="cyclingType"
                v-if="edit"
                class="p-2 w-full text-gray-500 focus:outline-none"
                type="text"
                v-model="item.cyclingType"
              >
                <option value="#">Wybierz Typ</option>
                <option value="city">Rower Miejski</option>
                <option value="cross">Rower Crossowy</option>
                <option value="road">Rower Szosowy</option>
                <option value="mtb">Rower MTB</option>
              </select>
              <p v-else>{{ item.cyclingType }}</p>
            </div>
            <div class="flex flex-1 flex-col">
              <label for="distance" class="mb-1 text-sm text-at-light-green">
                Dystans
              </label>
              <input
                v-if="edit"
                id="distance"
                class="p-2 w-full text-gray-500 focus:outline-none"
                type="text"
                v-model="item.distance"
              />
              <p v-else>{{ item.distance }}</p>
            </div>
            <div class="flex flex-1 flex-col">
              <label for="duration" class="mb-1 text-sm text-at-light-green">
                Czas Trwania
              </label>
              <input
                v-if="edit"
                id="duration"
                class="p-2 w-full text-gray-500 focus:outline-none"
                type="text"
                v-model="item.duration"
              />
              <p v-else>{{ item.duration }}</p>
            </div>
            <div class="flex flex-1 flex-col">
              <label for="pulse" class="mb-1 text-sm text-at-light-green">
                Tętno (BPM)
              </label>
              <input
                v-if="edit"
                id="pulse"
                class="p-2 w-full text-gray-500 focus:outline-none"
                type="text"
                v-model="item.pulse"
              />
              <p v-else>{{ item.pulse }}</p>
            </div>
            <img
              @click="deleteExercise(item.id)"
              v-if="edit"
              class="absolute h-4 w-auto -left-5 cursor-pointer"
              src="@/assets/images/trash-light-green.png"
              alt=""
            />
          </div>
           <button
            @click="addExercise"
            v-if="edit"
            type="button"
            class="py-2 px-6 rounded-sm self-start text-sm text-white
            bg-at-light-green duration-200 border-solid border-2 border-transparent
            hover:border-at-light-green hover:bg-white hover:text-at-light-green"
          >
            Dodaj Ćwiczenie
          </button>
        </div>
      </div>

      <!-- Update -->
      <button 
        v-if="edit"
        @click="update"
        type="button" 
        class="mt-10 py-2 px-6 rounded-sm self-start text-sm text-white bg-at-light-green duration-200 border-solid
               border-2 border-transparent hover:border-at-light-green hover:bg-white hover:text-at-light-green">
          Aktualizuj Ćwiczenie
      </button>
    </div>
  </div>
</template>

<script>
import { ref, computed } from 'vue';
import { supabase } from '../supabase/init';
import { useRoute, useRouter } from 'vue-router';
import store from "../store/index"
import { uid } from "uid";

export default {
  name: "view-workout",
  setup() {
    // Create data / vars
    const data = ref(null);
    const dataLoaded = ref(null);
    const errorMsg = ref(null);
    const statusMsg = ref(null);
    const route = useRoute();
    const router = useRouter();
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
    const deleteWorkout = async () => {
      try {
        const { error } = await supabase
          .from("workouts")
          .delete()
          .eq("id", currentId);
        if (error) throw error;
        router.push({ name: "Home" });
      } catch (error) {
        errorMsg.value = `Error: ${error.message}`;
        setTimeout(() => {
          errorMsg.value = false;
        }, 5000);
      }
    };

    // Edit mode
    const edit = ref(null);

    const editMode = () => {
      edit.value = !edit.value;
    }

    // Add exercise
    const addExercise = () => {
      if (data.value.workoutType === "strength") {
        data.value.exercises.push({
          id: uid(),
          exercise: "",
          sets: "",
          reps: "",
          weight: "",
        });
        return;
      } else if (data.value.workoutType === "cardio") {
        data.value.exercises.push({
          id: uid(),
          cardioType: "",
          distance: "",
          duration: "",
          pace: "",
        });
        return;
      }
    };

    // Delete exercise
    const deleteExercise = (id) => {
      if (data.value.exercises.length > 1) {
        data.value.exercises = data.value.exercises.filter(
          (exercise) => exercise.id !== id
        );
        return;
      }
      errorMsg.value = "Error: Nie można usunąć, trzeba mieć przynajmniej jedno ćwiczenie.";
      setTimeout(() => {
        errorMsg.value = false;
      }, 5000);
    };

    // Update Workout
    const update = async () => {
      try {
        const { error } = await supabase
          .from("workouts")
          .update({
            workoutName: data.value.workoutName,
            exercises: data.value.exercises,
          })
          .eq("id", currentId);
        if (error) throw error;
        edit.value = false;
        statusMsg.value = "Success: Zaktualizowano trening!";
        setTimeout(() => {
          statusMsg.value = false;
        }, 5000);
      } catch (error) {
        errorMsg.value`Error: ${error.message}`;
        setTimeout(() => {
          errorMsg.value = false;
        }, 5000);
      }
    };

    return { 
      statusMsg, 
      data, 
      dataLoaded, 
      errorMsg, 
      edit,
      editMode, 
      user, 
      deleteWorkout, 
      addExercise,
      deleteExercise,
      update
    };
  },
};
</script>
