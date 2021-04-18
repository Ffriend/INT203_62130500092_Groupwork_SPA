<template>
  <div id="nav" class="bg-black h-12">
    <div class="text-white space-x-9 flex justify-end pt-3 pr-14">
      <router-link to="/" class="hover:text-yellow-300">Home</router-link>
      <router-link to="/about" class="hover:text-yellow-300">About</router-link>
      <router-link to="/review" class="hover:text-yellow-300"
        >Review</router-link
      >
    </div>
  </div>
  <router-view />
</template>