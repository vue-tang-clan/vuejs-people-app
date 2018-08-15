<template>
  <div class="home">

    <section id="main-slider" class="no-margin">
        <div class="carousel slide">
            <ol class="carousel-indicators">
                <li data-target="#main-slider" data-slide-to="0" class="active"></li>
                <li data-target="#main-slider" data-slide-to="1"></li>
                <li data-target="#main-slider" data-slide-to="2"></li>
            </ol>
            <div class="carousel-inner">

                <div class="item active" style="background-image: url(images/slider/bg1.jpg)">
                    <div class="container">
                        <div class="row slide-margin">
                            <div class="col-sm-6">
                                <div class="carousel-content">
                                    <h1 class="animation animated-item-1">Lorem ipsum dolor sit amet consectetur adipisicing elit</h1>
                                    <h2 class="animation animated-item-2">Accusantium doloremque laudantium totam rem aperiam, eaque ipsa...</h2>
                                    <a class="btn-slide animation animated-item-3" href="#">Read More</a>
                                </div>
                            </div>

                            <div class="col-sm-6 hidden-xs animation animated-item-4">
                                <div class="slider-img">
                                    <img src="images/slider/img1.png" class="img-responsive">
                                </div>
                            </div>

                        </div>
                    </div>
                </div><!--/.item-->

                <div class="item" style="background-image: url(images/slider/bg2.jpg)">
                    <div class="container">
                        <div class="row slide-margin">
                            <div class="col-sm-6">
                                <div class="carousel-content">
                                    <h1 class="animation animated-item-1">Lorem ipsum dolor sit amet consectetur adipisicing elit</h1>
                                    <h2 class="animation animated-item-2">Accusantium doloremque laudantium totam rem aperiam, eaque ipsa...</h2>
                                    <a class="btn-slide animation animated-item-3" href="#">Read More</a>
                                </div>
                            </div>

                            <div class="col-sm-6 hidden-xs animation animated-item-4">
                                <div class="slider-img">
                                    <img src="images/slider/img2.png" class="img-responsive">
                                </div>
                            </div>

                        </div>
                    </div>
                </div><!--/.item-->

                <div class="item" style="background-image: url(images/slider/bg3.jpg)">
                    <div class="container">
                        <div class="row slide-margin">
                            <div class="col-sm-6">
                                <div class="carousel-content">
                                    <h1 class="animation animated-item-1">Lorem ipsum dolor sit amet consectetur adipisicing elit</h1>
                                    <h2 class="animation animated-item-2">Accusantium doloremque laudantium totam rem aperiam, eaque ipsa...</h2>
                                    <a class="btn-slide animation animated-item-3" href="#">Read More</a>
                                </div>
                            </div>
                            <div class="col-sm-6 hidden-xs animation animated-item-4">
                                <div class="slider-img">
                                    <img src="images/slider/img3.png" class="img-responsive">
                                </div>
                            </div>
                        </div>
                    </div>
                </div><!--/.item-->
            </div><!--/.carousel-inner-->
        </div><!--/.carousel-->
        <a class="prev hidden-xs" href="#main-slider" data-slide="prev">
            <i class="fa fa-chevron-left"></i>
        </a>
        <a class="next hidden-xs" href="#main-slider" data-slide="next">
            <i class="fa fa-chevron-right"></i>
        </a>
    </section><!--/#main-slider-->



    <h1>Add a person</h1>
    <div>
      Name: <input type="text" v-model="newPerson.name">
      Bio: <input type="text" v-model="newPerson.bio">
      <button v-on:click="addPerson()">Add person</button>
    </div>

    <form v-on:submit.prevent="submit()">
      <h2>Create a new person:</h2>
      <div>
        Name: <input type="text" name="title" v-model="newPerson.name">
      </div>
      <div>
        Bio: <input type="text" name="body" v-model="newPerson.bio">
      </div>
      <div>
        Avatar: <input type="file" v-on:change="setFile($event)" ref="fileInput">
      </div>
      <input type="submit" value="Submit">
    </form>


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
      avatar: "",
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
    setFile: function(event) {
      if (event.target.files.length > 0) {
        this.avatar = event.target.files[0];
      }
    },
    submit: function() {
      var formData = new FormData();
      formData.append("name", this.newPerson.name);
      formData.append("bio", this.newPerson.bio);
      formData.append("avatar", this.avatar);

      axios
        .post("http://localhost:3000/api/people", formData)
        .then(response => {
          this.people.push(response.data);
          this.newPerson = { name: "", bio: "", bioVisible: true };
          this.$refs.fileInput.value = "";
        });
    },
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
