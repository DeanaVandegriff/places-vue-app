<script>
import axios from "axios";

export default {
  data: function () {
    return {
      message: "Welcome to Places!",
      places: {},
      newPlaceParams: {},
      updatePlaceParams: {},
      errors: [],
      currentPlace: {},
    };
  },
  created: function () {
    this.placesIndex();
  },
  methods: {
    placesIndex: function () {
      axios.get("http://localhost:3000/places.json").then((response) => {
        console.log(response.data);
        this.places = response.data;
      });
    },
    placesCreate: function () {
      console.log(this.newPlaceParams);
      axios
        .post("http://localhost:3000/places.json", this.newPlaceParams)
        .then((response) => {
          console.log(response.data);
          this.places << response.data;
        })
        .catch((error) => console.log(error.response));
      this.newPlaceParams.name;
    },
    placeUpdate: function (place) {
      this.currentPlace = place;
      axios
        .patch("http://localhost:3000/places/" + this.currentPlace.id + ".json", this.editPlaceParams)
        .then((response) => {
          console.log(response.data);
        })
        .catch((error) => console.log(error.response));
    },
    placeDestroy: function (place) {
      this.currentPlace = place;
      axios
        .delete("http://localhost:3000/places/" + this.currentPlace.id + ".json")
        .then((response) => {
          console.log(response.data);
        })
        .catch((error) => console.log(error.response));
    },
  },
};
</script>

<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <div v-for="place in places" v-bind:key="place.id">
      <h2>{{ place.name }}</h2>
      <p>{{ place.address }}</p>
      <button v-on:click="placeDestroy(currentPlace)">Delete</button>
    </div>
    <div>
      <h2>Add a new place:</h2>
      <form>
        <p>
          Place name:
          <input type="text" v-model="newPlaceParams.name" placeholder="name" />
        </p>
        <p>
          Address:
          <input type="text" v-model="newPlaceParams.address" placeholder="address" />
        </p>
      </form>
      <button v-on:click="placesCreate()">add a new place</button>
    </div>
    <div>
      <p>Update a place:</p>
      <form>
        <p>
          Place name:
          <input type="text" v-model="updatePlaceParams.name" placeholder="name" />
        </p>
        <p>
          Address:
          <input type="text" v-model="updatePlaceParams.address" placeholder="address" />
        </p>
      </form>
      <button v-on:click="placesUpdate()">update place</button>
    </div>
  </div>
</template>

<style></style>
