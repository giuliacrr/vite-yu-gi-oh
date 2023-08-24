<script>
export default {};
</script>

<template>
  <div class="container bg-light mt-5 p-3 d-flex flex-wrap">
    <!--Card-->
    <div class="card m-3">
      <!-- Card Image -->
      <img
        src="https://images.ygoprodeck.com/images/cards/6850209.jpg"
        class="card-img-top"
        alt="carta"
      />
      <div class="card-body">
        <!-- Card Title -->
        <h5 class="text-center text-white">Card name</h5>
        <!-- Card type -->
        <p class="card-text text-center fw-bold">type</p>
      </div>
    </div>
    <!--End Card-->
  </div>
</template>

<style lang="scss" scoped>
.card-body {
  background-color: #d48f38;
}

.card-title {
  font-size: 0.9rem;
}

.card-text {
  font-size: 0.7rem;
}

.card {
  width: calc((100% / 5) - 2rem);
}
</style>
