<template>
  <div class="p-4 md:p-10 flex flex-col items-center justify-center w-full max-w-6xl m-auto">
    <div class="w-full">
      <PhoneBook />
    </div>
  </div>
</template>

<script>
  import PhoneBook from "./components/PhoneBook.vue";

  export default {
    name: "App",
    components: {
      PhoneBook
    }
  };
</script>
