<template>
  <header class="bg-at-light-green text-white">
    <nav class="container py-5 px-4 flex flex-col gap-4 items-center sm:flex-row">
      <div class="flex items-center gap-x-4">
          <img class="w-14" src="../assets/images/dumbbell-light.png" alt="dumbbell-light">
          <h1 class="text-lg">Codzienna Aktywność Fizyczna</h1>
      </div>
      <ul class="flex flex-1 justify-end gap-x-10">
        <router-link class="cursor-pointer" :to="{ name: 'Home' }">Strona główna</router-link>
        <router-link class="cursor-pointer" :to="{ name: '' }">Dodaj</router-link>
        <router-link class="cursor-pointer" :to="{ name: 'Login' }">Zaloguj się</router-link>
        <li @click="logout" class="cursor-pointer">Wyloguj się</li>
      </ul>
    </nav>
  </header>
</template>

<script>
import { supabase } from '../supabase/init';
import { useRouter } from "vue-router";
export default {
  setup() {
    // Get user from store

    // Setup ref to router
    const router = useRouter();

    // Logout function
    const logout = async () => {
      await supabase.auth.signOut();
      router.push({ name: "Home" });
    }

    return { logout };
  },
};
</script>
