<template>
  <div class="max-w-screen-md mx-auto px-4 py-10">
    <!-- Status Message -->
    <div v-if="statusMsg || errorMsg" class="mb-10 p-4 bg-light-grey rounded-md shadow-lg">
      <p class="text-at-light-green">{{ statusMsg }}</p>
      <p class="text-red-500">{{ errorMsg }}</p>
    </div>

    <!-- Create -->
    <div class="p-8 flex items-start bg-light-grey rounded-md shadow-xl">
      <!-- Form -->
      <form @submit.prevent="createWorkout" class="flex flex-col gap-y-5 w-full">

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
            @change="workoutChange"
            v-model="workoutType">

            <option value="select-workout">Wybierz Trening</option>
            <option value="strength">Trening Siłowy</option>
            <option value="cardio">Kardio</option>
            <option value="cycling">Jazda Rowerem</option>
            <option value="stretching">Rozciąganie</option>
            <option value="swimming">Pływanie</option>
          </select>
        </div>

        <!-- Strength Training Inputs -->
        <div v-if="workoutType === 'strength'" class="flex flex-col gap-y-4">
          <div 
            class="flex flex-col gap-x-6 gap-y-2 relative md:flex-row" 
            v-for="(item, index) in exercises"
            :key="index">

            <div class="flex flex-col md:w-1/3">
              <label 
                for="exercise-name" 
                class="mb-1 text-sm text-at-light-green">
                Ćwiczenie
              </label>

              <input 
                required 
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.exercise"
              />
            </div>

            <div class="flex flex-col flex-1">
              <label 
                for="sets" 
                class="mb-1 text-sm text-at-light-green">
                Serie
              </label>

              <input 
                required 
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.sets"
              />
            </div>

            <div class="flex flex-col flex-1">
              <label 
                for="reps" 
                class="mb-1 text-sm text-at-light-green">
                Powtórzenia
              </label>

              <input 
                required 
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.reps"
              />
            </div>

            <div class="flex flex-col flex-1">
              <label 
                for="weight" 
                class="mb-1 text-sm text-at-light-green">
                Ciężar (KG's)
              </label>

              <input 
                required 
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.weight"
              />
            </div>

            <img 
              @click="deleteExercise(item.id)"
              src="@/assets/images/trash-light-green.png" 
              class="h-4 w-auto absolute -left-5 cursor-pointer" 
              alt="trash"
            />
          </div>

          <button
            @click="addExercise"
            type="button"
            class="mt-20 py-2 px-6 rounded-sm self-start text-sm text-white bg-at-light-green duration-200 border-solid
            border-2 border-transparent hover:border-at-light-green hover:bg-white hover:text-at-light-green"
          >
            Dodaj Ćwiczenie!
          </button>
        </div>

        <!-- Cardio Training Inputs -->
        <div v-if="workoutType === 'cardio'" class="flex flex-col gap-y-4">
          <div 
            class="flex flex-col gap-x-6 gap-y-2 relative md:flex-row" 
            v-for="(item, index) in exercises"
            :key="index">

            <div class="flex flex-col md:w-1/3">
              <label 
                for="cardio-type" 
                class="mb-1 text-sm text-at-light-green">
                Typ
              </label>
              <select 
                id="cardio-type" 
                class="p-2 w-full text-gray-500 focus:outline-none" 
                v-model="item.cardioType">

                <option value="#">Wybierz Typ</option>
                <option value="run">Bieganie</option>
                <option value="walk">Chodzenie</option>
              </select>
            </div>

            <div class="flex flex-col flex-1">
              <label 
                for="distance" 
                class="mb-1 text-sm text-at-light-green">
                Dystans
              </label>

              <input 
                required 
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.distance"
              />
            </div>

            <div class="flex flex-col flex-1">
              <label 
                for="duration" 
                class="mb-1 text-sm text-at-light-green">
                Czas Trwania
              </label>

              <input 
                required 
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.duration"
              />
            </div>

            <div class="flex flex-col flex-1">
              <label 
                for="pace" 
                class="mb-1 text-sm text-at-light-green">
                Tempo
              </label>

              <input 
                required 
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.pace"
              />
            </div>

            <img 
              @click="deleteExercise(item.id)"
              src="@/assets/images/trash-light-green.png" 
              class="h-4 w-auto absolute -left-5 cursor-pointer" 
              alt="trash"
            />
          </div>

          <button
            @click="addExercise"
            type="button"
            class="mt-20 py-2 px-6 rounded-sm self-start text-sm text-white bg-at-light-green duration-200 border-solid
            border-2 border-transparent hover:border-at-light-green hover:bg-white hover:text-at-light-green"
          >
            Dodaj Ćwiczenie!
          </button>
        </div>

         <!-- Cycling Training Inputs -->
        <div v-if="workoutType === 'cycling'" class="flex flex-col gap-y-4">
          <div 
            class="flex flex-col gap-x-6 gap-y-2 relative md:flex-row" 
            v-for="(item, index) in exercises"
            :key="index">

            <div class="flex flex-col md:w-1/3">
              <label 
                for="cycling-type" 
                class="mb-1 text-sm text-at-light-green">
                Typ Roweru
              </label>
              <select 
                id="cycling-type" 
                class="p-2 w-full text-gray-500 focus:outline-none" 
                v-model="item.cyclingType">

                <option value="#">Wybierz Typ</option>
                <option value="city">Rower Miejski</option>
                <option value="cross">Rower Crossowy</option>
                <option value="road">Rower Szosowy</option>
                <option value="mtb">Rower MTB</option>
              </select>
            </div>

            <div class="flex flex-col flex-1">
              <label 
                for="distance" 
                class="mb-1 text-sm text-at-light-green">
                Dystans
              </label>

              <input 
                required 
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.distance"
              />
            </div>

            <div class="flex flex-col flex-1">
              <label 
                for="duration" 
                class="mb-1 text-sm text-at-light-green">
                Czas Trwania
              </label>

              <input 
                required 
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.duration"
              />
            </div>

            <div class="flex flex-col flex-1">
              <label 
                for="pulse" 
                class="mb-1 text-sm text-at-light-green">
                Tętno (BPM)
              </label>

              <input 
                required 
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.pulse"
              />
            </div>

            <img 
              @click="deleteExercise(item.id)"
              src="@/assets/images/trash-light-green.png" 
              class="h-4 w-auto absolute -left-5 cursor-pointer" 
              alt="trash"
            />
          </div>

          <button
            @click="addExercise"
            type="button"
            class="mt-20 py-2 px-6 rounded-sm self-start text-sm text-white bg-at-light-green duration-200 border-solid
            border-2 border-transparent hover:border-at-light-green hover:bg-white hover:text-at-light-green"
          >
            Dodaj Ćwiczenie!
          </button>
        </div>

        <!-- Stretching Training Inputs -->
        <div v-if="workoutType === 'stretching'" class="flex flex-col gap-y-4">
          <div 
            class="flex flex-col gap-x-6 gap-y-2 relative md:flex-row" 
            v-for="(item, index) in exercises"
            :key="index">

            <div class="flex flex-col md:w-1/3">
              <label 
                for="exercise-name" 
                class="mb-1 text-sm text-at-light-green">
                Ćwiczenie
              </label>

              <input 
                required 
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.exercise"
              />
            </div>

            <div class="flex flex-col flex-1">
              <label 
                for="sets" 
                class="mb-1 text-sm text-at-light-green">
                Serie
              </label>

              <input 
                required 
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.sets"
              />
            </div>

            <div class="flex flex-col flex-1">
              <label 
                for="reps" 
                class="mb-1 text-sm text-at-light-green">
                Powtórzenia
              </label>

              <input 
                required 
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.reps"
              />
            </div>

            <div class="flex flex-col flex-1">
              <label 
                for="weight" 
                class="mb-1 text-sm text-at-light-green">
                Ciężar (KG's)
              </label>

              <input 
                required 
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.weight"
              />
            </div>

            <img 
              @click="deleteExercise(item.id)"
              src="@/assets/images/trash-light-green.png" 
              class="h-4 w-auto absolute -left-5 cursor-pointer" 
              alt="trash"
            />
          </div>

          <button
            @click="addExercise"
            type="button"
            class="mt-20 py-2 px-6 rounded-sm self-start text-sm text-white bg-at-light-green duration-200 border-solid
            border-2 border-transparent hover:border-at-light-green hover:bg-white hover:text-at-light-green"
          >
            Dodaj Ćwiczenie!
          </button>
        </div>

        <!-- Swimming Training Inputs -->
        <div v-if="workoutType === 'swimming'" class="flex flex-col gap-y-4">
          <div 
            class="flex flex-col gap-x-6 gap-y-2 relative md:flex-row" 
            v-for="(item, index) in exercises"
            :key="index">

            <div class="flex flex-col md:w-1/3">
              <label 
                for="swimming-type" 
                class="mb-1 text-sm text-at-light-green">
                Styl
              </label>
              <select 
                id="swimming-type" 
                class="p-2 w-full text-gray-500 focus:outline-none" 
                v-model="item.swimmingType">

                <option value="#">Wybierz Styl</option>
                <option value="crawl">Kraul</option>
                <option value="frog">Żabka</option>
                <option value="back">Grzbietowy</option>
                <option value="butterfly">Motylkowy</option>
                <option value="dog">Piesek</option>
              </select>
            </div>

            <div class="flex flex-col flex-1">
              <label 
                for="distance" 
                class="mb-1 text-sm text-at-light-green">
                Dystans
              </label>

              <input 
                required 
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.distance"
              />
            </div>

            <div class="flex flex-col flex-1">
              <label 
                for="duration" 
                class="mb-1 text-sm text-at-light-green">
                Czas Trwania
              </label>

              <input 
                required 
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.duration"
              />
            </div>

            <div class="flex flex-col flex-1">
              <label 
                for="pulse" 
                class="mb-1 text-sm text-at-light-green">
                Tętno (BPM)
              </label>

              <input 
                required 
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.pulse"
              />
            </div>

            <img 
              @click="deleteExercise(item.id)"
              src="@/assets/images/trash-light-green.png" 
              class="h-4 w-auto absolute -left-5 cursor-pointer" 
              alt="trash"
            />
          </div>

          <button
            @click="addExercise"
            type="button"
            class="mt-20 py-2 px-6 rounded-sm self-start text-sm text-white bg-at-light-green duration-200 border-solid
            border-2 border-transparent hover:border-at-light-green hover:bg-white hover:text-at-light-green"
          >
            Dodaj Ćwiczenie!
          </button>
        </div>

        <button
          type="submit"
           class="mt-20 py-2 px-6 rounded-sm self-start text-sm text-white bg-at-light-green duration-200 border-solid
           border-2 border-transparent hover:border-at-light-green hover:bg-white hover:text-at-light-green"
         >
          Zapisz Trening!
        </button>
      </form>
    </div>
  </div>
</template>

<script>
import { ref } from "vue";
import { uid } from "uid";
import { supabase } from '../supabase/init';

export default {
  name: "create",
  setup() {
    // Create data
    const workoutName = ref("");
    const workoutType = ref("select-workout");
    const exercises = ref([1]);
    const statusMsg = ref(null);
    const errorMsg = ref(null);

    // Add exercise
    const addExercise = () => {
      if (workoutType.value === 'strength') {
        exercises.value.push({
          id: uid(),
          exercise: "",
          sets: "",
          reps: "",
          weight: "",
        });
        return;

      } else if (workoutType.value === 'cardio') {
        exercises.value.push({
          id: uid(),
          cardioType: "",
          distance: "",
          duration: "",
          pace: "",
        });
      } else if (workoutType.value === 'cycling') {
        exercises.value.push({
          id: uid(),
          cyclingType: "",
          distance: "",
          duration: "",
          pulse: "",
        });
      } else if (workoutType.value === 'stretching') {
        exercises.value.push({
          id: uid(),
          exercise: "",
          sets: "",
          reps: "",
          weight: "",
        });
      } else if (workoutType.value === 'swimming') {
        exercises.value.push({
          id: uid(),
          swimmingType: "",
          distance: "",
          duration: "",
          pulse: "",
        });
      }
    }

    // Delete exercise
    const deleteExercise = (id) => {
      if (exercises.value.length > 1) {
        exercises.value = exercises.value.filter((exercise) => exercise.id !== id);
        return;
      }

      errorMsg.value = "Error: Nie można usunąć, należy mieć przynajmniej jedno ćwiczenie!";
      setTimeout(() => {
        errorMsg.value = false;
      }, 5000);
    };

    // Listens for chaging of workout type input
    const workoutChange = () => {
      exercises.value = [];
      addExercise();
    };

    // Create workout
    const createWorkout = async () => {
      try {
        const { error } = await supabase.from('workouts').insert([
          {
            workoutName: workoutName.value,
            workoutType: workoutType.value,
            exercises: exercises.value,
          }
        ]);
        if (error) throw error;
        statusMsg.value = 'Sukces: Stworzono Trening!';
        workoutName.value = null;
        workoutType.value = "select-workout";
        exercises.value = [];

        setTimeout(() => {
          statusMsg.value = false;
        }, 5000);

      } catch(error) {
        errorMsg.value = `Error: ${error.message}`;
        setTimeout(() => {
          errorMsg.value = false;
        }, 5000);
      }
    }

    return { 
      workoutName, 
      workoutType, 
      exercises, 
      statusMsg, 
      errorMsg, 
      addExercise, 
      workoutChange, 
      deleteExercise,
      createWorkout
      };
  },
};
</script>
