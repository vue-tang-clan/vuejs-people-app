<template>
  <div class="home">
    <h1>Add a person</h1>
    <div>
      Name: <input type="text" v-model="newPerson.name">
      Bio: <input type="text" v-model="newPerson.bio">
      <button v-on:click="addPerson()">Add person</button>
    </div>
    <h1>People ({{ people.length }} total)</h1>
    <div v-for="x in people">
      <h2 v-on:click="toggleBio(x)">{{ x.name }}</h2>
      <p v-bind:class="{strike: x.bioVisible}">{{ x.bio }}</p>
      <button v-on:click="deletePerson(x)">Delete</button>
    </div>
  </div>
</template> 

<style>
.strike {
  text-decoration: line-through;
}
</style>

<script>
var axios = require("axios");

export default {
  data: function() {
    return {
      people: [],
      newPerson: { name: "", bio: "", bioVisible: true }
    };
  },
  created: function() {
    console.log("starting up Home.vue...");
    axios.get("http://localhost:3000/api/people").then(
      function(response) {
        console.table(response.data);
        this.people = response.data;
      }.bind(this)
    );
  },
  methods: {
    toggleBio: function(inputPerson) {
      inputPerson.bioVisible = !inputPerson.bioVisible;
    },
    addPerson: function() {
      var params = {
        name: this.newPerson.name,
        bio: this.newPerson.bio
      };
      axios.post("http://localhost:3000/api/people", params).then(
        function(response) {
          this.people.push(response.data);
          this.newPerson = { name: "", bio: "", bioVisible: true };
        }.bind(this)
      );
    },
    deletePerson: function(inputPerson) {
      var index = this.people.indexOf(inputPerson);
      this.people.splice(index, 1);
    }
  },
  computed: {}
};
</script>
