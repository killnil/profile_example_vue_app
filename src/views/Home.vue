<template>
  <div class="home">
    <form v-on:submit.prevent="submit()">
      <div>
        Name: <input v-model="newName">
      </div>
      <div>
        Image: <input type="file" v-on:change="setFile($event)" ref="fileInput">
      </div>
      <input type="submit" value="Go">
    </form>
    <div v-for="profile in profiles">
      <h2>{{ profile.name }}</h2>
      <img :src="profile.image_url" alt="">
    </div>
  </div>
</template>

<style>
</style>

<script>
  var axios = require('axios');

export default {
  data: function() {
    return {
      profiles: [],
      newName: "",
      newImage: ""
    };
  },
  created: function() {
    axios.get("/api/profiles")
      .then(response => {
        this.profiles = response.data;
      });
  },
  methods: {
    submit: function() {
      var params = new FormData();
      params.append("name", this.newName);
      params.append("image", this.newImage);

      axios.post("/api/profiles", params)
        .then(response => {
          this.profiles = response.data;
          this.newName = "";
          this.newImage = "";
          this.$refs.fileInput.value = "";
        });
    },
    setFile: function(event) {
      if (event.target.files.length > 0) {
        this.newImage = event.target.files[0];
      }
    }
  }
};
</script>












