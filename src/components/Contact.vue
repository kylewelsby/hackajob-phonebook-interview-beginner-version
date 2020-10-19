<template>
  <a :href="tel" class="w-full flex flex-row items-center my-4">
    <img
      :src="picture.large"
      class="rounded-full w-12 mr-2"
      width="48"
      height="48"
    />
    <div class="flex flex-col">
      <strong class="font-medium">{{ name }}</strong>
      <span class="text-sm">{{ formattedPhoneNumber }}</span>
    </div>
  </a>
</template>
<script>
  import parsePhoneNumber from "libphonenumber-js";
  export default {
    props: ["contact"],
    computed: {
      name() {
        return this.contact.name;
      },
      phoneNumber() {
        return parsePhoneNumber(this.contact["phone_number"]);
      },
      address() {
        return this.contact.address;
      },
      formattedPhoneNumber() {
        return this.phoneNumber.formatNational();
      },
      tel() {
        return this.phoneNumber.getURI();
      },
      picture() {
        return this.contact.picture || {};
      }
    }
  };
</script>
