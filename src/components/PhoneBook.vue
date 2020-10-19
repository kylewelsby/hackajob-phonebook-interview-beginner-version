<template>
  <input
    type="search"
    v-model="term"
    @keyup="search"
    class="w-full rounded bg-gray-200 py-3 px-3 focus:outline-none focus:shadow-outline text-md shadow-lg"
    placeholder="Search by name or number"
  />
  <Contact
    v-for="result in filteredResults"
    :contact="result.item"
    :key="result.indexRef"
  />
</template>
<script>
  import Contact from "./Contact.vue";
  import Fuse from "fuse.js";

  export default {
    components: {
      Contact
    },
    data() {
      return {
        contacts: [],
        filteredResults: [],
        term: "",
        fuseOptions: {
          keys: ["name"],
          threshold: 0.4
        },
        fuse: undefined
      };
    },
    beforeMount() {
      return this.fetchUsers()
        .then(() => {
          this.createIndex();
          return this.fetchFaces();
        })
        .then(() => {
          this.search();
        });
    },
    computed: {
      sortedContacts() {
        return this.contacts.slice(0).sort((a, b) => {
          if (a.name > b.name) {
            return 1;
          } else if (a.name < b.name) {
            return -1;
          } else {
            return 0;
          }
        });
      }
    },
    methods: {
      fetchUsers() {
        return fetch("https://www.mocky.io/v2/581335f71000004204abaf83")
          .then((res) => res.json())
          .then((res) => {
            this.contacts = res.contacts;
          });
      },
      fetchFaces() {
        return fetch(
          `https://randomuser.me/api/?inc=picture&results=${this.contacts.length}&seed=hackerrank`
        )
          .then((res) => res.json())
          .then((res) => {
            res.results.forEach((item, index) => {
              this.contacts[index].picture = item.picture;
            });
          });
      },
      createIndex() {
        const myIndex = Fuse.createIndex(this.fuseOptions.keys, this.contacts);
        this.fuse = new Fuse(this.contacts, this.fuseOptions, myIndex);
      },
      search() {
        if (this.term.length) {
          this.filteredResults = this.fuse.search(this.term);
        } else {
          this.filteredResults = this.sortedContacts.map((i) => {
            return { item: i };
          });
        }
      }
    }
  };
</script>
