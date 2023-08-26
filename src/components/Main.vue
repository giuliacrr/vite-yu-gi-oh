<script>
import axios from "axios";

export default {
  data() {
    return {
      CardInfos: [],
      CardArchetypes: [],
      SelectArchetypeCards: "",
      metaArray: [],
    };
  },
  methods: {
    //Function to fetch cards infos from api database and push them into CardInfos array and then print them in html
    fetchCardInfos() {
      let url = "";
      if (
        this.SelectArchetypeCards === "" ||
        this.SelectArchetypeCards === undefined
      ) {
        url = "https://db.ygoprodeck.com/api/v7/cardinfo.php?num=20&offset=0";
      } else {
        url = `https://db.ygoprodeck.com/api/v7/cardinfo.php?archetype=${this.SelectArchetypeCards}&num=20&offset=0`;
      }
      axios.get(url).then((response) => {
        this.CardInfos = response.data.data;
        this.metaArray = response.data.meta;
      });
    },
    //Function to fetch archetypes names from api database and push them into CardArchetypes array and then print them in the select tag
    fetchCardArchetypes() {
      const arc = "https://db.ygoprodeck.com/api/v7/archetypes.php";
      axios.get(arc).then((response) => {
        this.CardArchetypes = response.data;
      });
    },
    // Con l'aiuto di mio padre Valerio Collura
    nextPage(newUrl) {
      axios.get(newUrl).then((response) => {
        this.CardInfos = response.data.data;
        this.metaArray = response.data.meta;
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
    <div class="select-bar mt-3 d-flex justify-content-center">
      <select
        class="form-select"
        v-model="SelectArchetypeCards"
        aria-label="Default select example"
      >
        <option selected hidden>Select Archetype - all</option>

        <!--IMPORTANTE = chiedere a Florian come prendere una carta che NON ha la key archetipo-->
        <option value="0">No Archetype</option>

        <!--Choose archetype-->
        <option
          v-for="SingleArchetype in CardArchetypes"
          :value="SingleArchetype.archetype_name"
        >
          {{ SingleArchetype.archetype_name }}
        </option>
      </select>
      <button class="search-button text-white ms-2" @click="fetchCardInfos()">
        Cerca
      </button>
    </div>
  </div>
  <!--Bonus-->
  <div id="cardsQuantyty" class="container mt-3">
    <span class="cards-quantity text-white">
      {{ this.metaArray.total_rows }} cards found
    </span>
  </div>
  <!--Button to go to next page-->
  <div class="container d-flex align-items-center justify-content-center">
    <!--Previous-->
    <button
      class="prev-next-btn"
      @click="nextPage(this.metaArray.previous_page)"
    >
      <i class="fa-solid fa-circle-chevron-left text-white fs-2"></i>
    </button>
    <!--Number of pages-->
    <div class="text-white">
      <!--IF IT'S JUST ONE PAGE-->
      <span v-if="this.metaArray.total_pages === 0">
        {{ this.metaArray.total_pages - this.metaArray.total_pages + 1 }}
        / 1
      </span>
      <!--IF IT'S JUST TWO PAGES-->
      <span v-if="this.metaArray.total_pages === 1">
        {{ this.metaArray.total_pages - this.metaArray.pages_remaining + 1 }}
        / 2
      </span>
      <!--IF IT'S MORE THAN 2 PAGES-->
      <span v-if="this.metaArray.total_pages > 1">
        {{ this.metaArray.total_pages - this.metaArray.pages_remaining }}
        /
        {{ this.metaArray.total_pages }}
      </span>
    </div>
    <!--Next-->
    <button class="prev-next-btn" @click="nextPage(this.metaArray.next_page)">
      <i class="fa-solid fa-circle-chevron-right text-white fs-2"></i>
    </button>
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
  <!--Button to go to next page-->
  <!--Button to go to next page-->
  <div
    class="container d-flex align-items-center justify-content-center mt-2 mb-2"
  >
    <!--Previous-->
    <button
      class="prev-next-btn"
      @click="nextPage(this.metaArray.previous_page)"
    >
      <a href="#header"
        ><i class="fa-solid fa-circle-chevron-left text-white fs-2"></i
      ></a>
    </button>
    <!--Number of pages-->
    <div class="text-white">
      <!--IF IT'S JUST ONE PAGE-->
      <span v-if="this.metaArray.total_pages === 0">
        {{ this.metaArray.total_pages - this.metaArray.total_pages + 1 }}
        / 1
      </span>
      <!--IF IT'S JUST TWO PAGES-->
      <span v-if="this.metaArray.total_pages === 1">
        {{ this.metaArray.total_pages - this.metaArray.pages_remaining + 1 }}
        / 2
      </span>
      <!--IF IT'S MORE THAN 2 PAGES-->
      <span v-if="this.metaArray.total_pages > 1">
        {{ this.metaArray.total_pages - this.metaArray.pages_remaining }}
        /
        {{ this.metaArray.total_pages }}
      </span>
    </div>
    <!--Next-->
    <button class="prev-next-btn" @click="nextPage(this.metaArray.next_page)">
      <a href="#header"
        ><i class="fa-solid fa-circle-chevron-right text-white fs-2"></i
      ></a>
    </button>
  </div>
</template>

<style lang="scss" scoped>
.search-button {
  background-color: #1e9e8f;
  border: transparent;
  padding: 4px;
  font-size: 1rem;
  border-radius: 10px;
}

.prev-next-btn {
  background-color: transparent;
  border: transparent;
}

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
