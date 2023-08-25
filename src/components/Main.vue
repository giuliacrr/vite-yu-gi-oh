<script>
import axios from "axios";

export default {
  data() {
    return {
      CardInfos: [],
      CardArchetypes: [],
    };
  },
  methods: {
    fetchCardInfos() {
      const url =
        "https://db.ygoprodeck.com/api/v7/cardinfo.php?num=20&offset=0";
      axios.get(url).then((response) => {
        this.CardInfos = response.data.data;
      });
    },
    fetchCardArchetypes() {
      const arc = "https://db.ygoprodeck.com/api/v7/archetypes.php";
      axios.get(arc).then((response) => {
        this.CardArchetypes = response.data;
      });
    },
  },
  mounted() {
    this.fetchCardInfos();
    this.fetchCardArchetypes();
  },
};
</script>

<template>
  <!--Filter-->
  <div class="container d-flex">
    <div class="select-bar mt-3">
      <select class="form-select" aria-label="Default select example">
        <option selected hidden>Select Archetype</option>
        <option value="0">No Archetype</option>
        <option v-for="(CardArchetypes, i) in CardArchetypes" :value="1 + i++">
          {{ CardArchetypes.archetype_name }}
        </option>
      </select>
    </div>
  </div>
  <!--Bonus-->
  <div class="container mt-3">
    <span class="cards-quantity text-white">
      {{ CardInfos.length }} cards found
    </span>
  </div>
  <div class="container bg-light mt-3 p-3 d-flex flex-wrap">
    <!--Card-->
    <div class="card m-3" v-for="infoCarta in CardInfos">
      <!-- Card Image -->
      <img
        :src="infoCarta.card_images[0].image_url"
        class="card-img-top"
        alt="carta"
      />
      <div class="card-body">
        <!-- Card Title -->
        <h5 class="titolo-carta text-center text-white">
          {{ infoCarta.name }}
        </h5>
        <!-- Card type -->
        <p class="card-text text-center fw-bold">{{ infoCarta.type }}</p>
      </div>
    </div>
    <!--End Card-->
  </div>
</template>

<style lang="scss" scoped>
.cards-quantity {
  background-color: #1e9e8f;
  padding: 8px;
  font-size: 1.2rem;
  border-radius: 10px;
}

.select-bar {
  width: 250px;
}

.card-body {
  background-color: #d48f38;
}

.titolo-carta {
  font-size: 1rem;
}

.card-text {
  font-size: 0.7rem;
}

.card {
  width: calc((100% / 5) - 2rem);
}
</style>
