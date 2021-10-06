<template>
  <div class="home">
    <h1>New Place</h1>
    <div>
      Name:
      <input type="text" v-model="newPlaceParams.name" />
    </div>
    <div>
      Address:
      <input type="text" v-model="newPlaceParams.address" />
    </div>
    <button v-on:click="createPlaces()">New Place</button>
    <div v-for="place in places" v-bind:key="place.id">
      <h2>Place: {{ place.name }}</h2>
      <button v-on:click="showPlace(place)">Show More Information</button>
    </div>
    <dialog id="place-info">
      <form method="dialog">
        <h1>Place Details</h1>
        <p>
          Name:
          <input type="text" v-model="currentPlace.name" />
        </p>
        <p>
          Address:
          <input type="text" v-model="currentPlace.address" />
        </p>
        <button v-on:click="updatePlace(currentPlace)">Update Place</button>
        <button v-on:click="destroyPlace(currentPlace)">Delete Place</button>
        <button>Close</button>
      </form>
    </dialog>
  </div>
</template>

<style></style>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      places: [],
      newPlaceParams: {},
      currentPlace: {},
    };
  },
  created: function () {
    this.indexPlaces();
  },
  methods: {
    indexPlaces: function () {
      axios.get("http://localhost:3000/places").then((response) => {
        console.log(response.data);
        this.places = response.data;
      });
    },
    createPlaces: function () {
      axios
        .post("http://localhost:3000/places", this.newPlaceParams)
        .then((response) => {
          console.log(response.data);
          this.places.push(response.data);
        })
        .catch((err) => console.log(err.response.data.errors));
    },
    showPlace: function (place) {
      console.log(place);
      this.currentPlace = place;
      document.querySelector("#place-info").showModal();
    },
    updatePlace: function (place) {
      var editPlaceParams = place;
      axios
        .patch(`http://localhost:3000/places/${place.id}`, editPlaceParams)
        .then((response) => {
          console.log(response.data);
        })
        .catch((error) => {
          console.log(error.response.data.errors);
        });
    },
    destroyPlace: function (place) {
      axios
        .delete(`http://localhost:3000/places/${place.id}`)
        .then((response) => {
          console.log("Success,", response.data);
          var index = this.place.indexOf(place);
          this.places.split(index, 1);
        });
    },
  },
};
</script>
