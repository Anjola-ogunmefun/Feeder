<template>
  <section>
    <div class="container py-3 d-flex justify-content-center">
      <div class="card d-flex">
        <img :src="image" class="card-img-top" alt="..." />
        <div class="card-body">
          <h5 class="card-title mb-5 text-center">
            <strong>{{ name }}</strong>
          </h5>
          <div class="card-text mb-5 container">
            <div class="d-flex">
              <p><strong>Diet labels:</strong></p>
              <ul>
                <li v-for="(diet, ind) in dietLabels" :key="ind.diet">
                  {{ diet }}
                </li>
              </ul>
            </div>

            <div class="d-flex">
              <p><strong>Health labels:</strong></p>
              <ul class="list-group list-group-horizontal">
                <li
                  class="list-group-item"
                  v-for="(health, ind) in healthLabels"
                  :key="ind.health"
                >
                  {{ health }}
                </li>
              </ul>
            </div>

            <div class="d-flex">
              <p><strong>Caution: </strong></p>
              <p v-if="cautions.length < 1">&nbsp; &nbsp; None</p>

              <ul v-else>
                <li v-for="(caution, ind) in cautions" :key="ind.caution">
                  {{ caution }}
                </li>
              </ul>
            </div>

            <div class="d-flex">
              <p><strong>Meal type: </strong></p>

              <p v-for="(meal, ind) in mealType" :key="ind.meal">
                &nbsp; {{ meal }}
              </p>
            </div>

            <div class="d-flex">
              <p><strong>Dish type:</strong></p>

              <ul>
                <li v-for="(dish, ind) in dishType" :key="ind.dish">
                  &nbsp; {{ dish }}
                </li>
              </ul>
            </div>

            <ul class="card-text"></ul>
            <br />

            <p><strong>Ingredients</strong></p>
            <ol class="">
              <li v-for="(ing, ind) in ingredients" :key="ind">{{ ing }}</li>
            </ol>

            <div class="mt-5">
              <p class="d-inline">Get further instructions</p>
              &nbsp;
              <a :href="link">here</a>
            </div>
          </div>
        </div>
      </div>

      <!-- <div class="row central"> 
        <div class="col-12 ">
          <img :src="image" alt="food image" class="img-fluid rounded mt-3 mb-3">
          <h4>{{ name }}</h4>
         
        </div>

        <div>
           <p class="d-inline">Get further instructions</p>
        <a :href="link">here</a>
        </div>
       
      </div> -->
    </div>
  </section>
</template>

<script>
export default {
  props: [
    'name',
    'dietLabels',
    'healthLabels',
    'cautions',
    'mealType',
    'dishType',
    'ingredients',
    'link',
    'image',
  ],

  // mounted() {
  //   console.log('caution', this.cautions)
  // },
}
</script>

<style scoped>
section {
  background: pink;
}
ul li {
  list-style: lower-roman;
}
/* .central {
  margin-left: 0.7rem;
} */
.card {
  width: 18rem;
}

/* --------------------------------------
   Media Queries
   -------------------------------------- */

/*  Small devices (landscape phones, 576px and up) */
@media (max-width: 576px) {
  .card {
    width: 25rem;
  }
}

/* Medium devices (tablets, 768px and up) */
@media (min-width: 768px) {
  .card {
    width: 40rem;
  }

  p,
  a {
    font-size: 1.35rem;
  }
  li {
    font-size: 1.2rem;
  }
  h5 {
    font-size: 1.8rem;
  }
}

/* Large devices (desktops, 992px and up) */
@media (min-width: 992px) {
  .card {
    width: 50rem;
  }
  p,
  a {
    font-size: 1.7rem;
  }
  li {
    font-size: 1.6rem;
  }
  h5 {
    font-size: 2.36rem;
  }
}

/* Extra large devices (large desktops, 1200px and up) */
@media (min-width: 1200px) {
}
</style>
