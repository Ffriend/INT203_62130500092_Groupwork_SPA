<template>
  <div
    class="h-screen relative"
    style="
      background-image: url(https://www.bluporthuahin.com/bluport/wp-content/uploads/2017/07/G13-14-1-GREYHOUND-CAFE-05.jpg);
    "
  >
    <div class="home">
      <h1
        class="pt-60 text-white text-5xl text-center font-black tracking-widest"
      >
        GREYHOUND
      </h1>
      <div class="pt-5 flex justify-center">
        <router-link to="/review">
          <base-button
            bgcolor="bg-yellow-400"
            txtcolor="text-white"
            label="Review!"
            class="hover:bg-red-700 p-2 rounded-xl text-xl font-semibold"
          >
          </base-button>
        </router-link>
      </div>
    </div>
  </div>
</template>