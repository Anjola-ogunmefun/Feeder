<template>
  <main>
    <!-- <section id="carousel" >
     <div id="carouselExampleIndicators" class="carousel slide" data-bs-ride="carousel">
  <div class="carousel-indicators">
    <button type="button" data-bs-target="#carouselExampleIndicators" data-bs-slide-to="0" class="active" aria-current="true" aria-label="Slide 1"></button>
    <button type="button" data-bs-target="#carouselExampleIndicators" data-bs-slide-to="1" aria-label="Slide 2"></button>
    <button type="button" data-bs-target="#carouselExampleIndicators" data-bs-slide-to="2" aria-label="Slide 3"></button>
        <button type="button" data-bs-target="#carouselExampleIndicators" data-bs-slide-to="3" aria-label="Slide 4"></button>

  </div>
  <div class="carousel-inner">
    <div class="carousel-item active">
      <img src="@/assets/images/fried.jpeg" class="d-block w-100 img-fluid" alt="...">
    </div>
    <div class="carousel-item">
      <img src="@/assets/images/rice.jpeg" class="d-block w-100 img-fluid" alt="...">
    </div>
    <div class="carousel-item">
      <img src="@/assets/images/group-eating.jpg" class="d-block w-100 img-fluid" alt="...">
    </div>
    <div class="carousel-item">
      <img src="@/assets/images/bread.jpg" class="d-block w-100 img-fluid" alt="...">
    </div>
  </div>
  <button class="carousel-control-prev" type="button" data-bs-target="#carouselExampleIndicators" data-bs-slide="prev">
    <span class="carousel-control-prev-icon" aria-hidden="true"></span>
    <span class="visually-hidden">Previous</span>
  </button>
  <button class="carousel-control-next" type="button" data-bs-target="#carouselExampleIndicators" data-bs-slide="next">
    <span class="carousel-control-next-icon" aria-hidden="true"></span>
    <span class="visually-hidden">Next</span>
  </button>
</div>
    </section> -->
    <transition name="modal">
      <base-dialog v-if="error" @close="error = false" :open="!!error">
        <h4 class="modal-head text-center">Error</h4>
        <p>Sorry, address not found.</p>
      </base-dialog>
    </transition>

    <section id="top">
      <div class="py-5">
      <figure class="text-center container">
        <blockquote class="blockquote">
          <p class="heading">FEEDER</p>
        </blockquote>
        <figcaption class="blockquote-footer sub-heading">
          Let's help you create up that special meal
        </figcaption>
      </figure>
      </div>
    </section>

    <section id="gallery">
      <div class="container py-5">
        <div class="row">
            <div class="col-md-12">
            <img
              src="@/assets/images/woman-cooking.jpeg"
              alt="fried rice"
              class="img-fluid"
            />
            <!-- <div class="bottom">Cooking hasn't been easier</div> -->
          </div>
          <div class="col-md-6 embed">
            <img
              src="@/assets/images/fried.jpeg"
              alt="fried rice"
              class="img-fluid"
            />
            <div class="bottom">Cooking hasn't been easier</div>
          </div>
          <div class="col-md-3 col-6 embed">
            <img
              src="@/assets/images/bread.jpg"
              alt="bread"
              class="img-fluid enlarge"
            />
            <div class="bottom">Takeaway deals</div>
          </div>
          <div class="col-md-3 col-6 embed">
            <img
              src="@/assets/images/rice.jpeg"
              alt="white rice"
              class="img-fluid enlarge"
            />
            <div class="bottom">Dine-in deals</div>
          </div>
        </div>
      </div>
    </section>

    <section id="instrution">
      <div class="container">
        <div class="row">
          <div class="col-12 text-center py-5">
            <h3>
              We love to answer your questions about the meal you would love to try out
            </h3>
          </div>

            <input
              class="form-control form-control-lg mb-5 text-center container"
              type="text"
              placeholder="Tell us what you want to cook..."
              aria-label=".form-control-lg example"
              v-model="query"
              @keyup.enter="reqRecipe"
            />
        </div>
      </div>
    </section>


    <section v-if="recipeList.length !=0">
      <!-- {{recipeList[0]}} -->
      <recipe-list v-for="recipe in recipeList" :key="recipe.uri"
        :name="recipe.label"
        :uri="recipe.uri"
        :image="recipe.image"
        :ingredient="recipe.ingredientLines"
        :cusineType="recipe.cusineType"
        :mealType="recipe.mealType"
        >
        {{recipe.label}}
      </recipe-list>
    </section>
  </main>
</template>

<script>
const axios = require('axios')

export default {
  data() {
    return {
      query: '',
      recipeList:[],
      error: null,
    }
  },

  methods: {
    async reqRecipe() {
      try {
        const response = await axios.get(
          `https://api.edamam.com/api/recipes/v2?type=public&q=${this.query}&app_id=abb2f4d9&app_key=%20f495acc1dfcdca5525d3f6cc7cf86997`
        )
        const responseData = response.data.hits
        for(const key in responseData){
          this.recipeList.push(responseData[key].recipe)
        console.log(responseData[key].recipe)
        }
        console.log(typeof this.recipeList)
      } catch (error) {
        console.error(error)
      }
    },
  },
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Arvo&family=Hina+Mincho&display=swap');
main {
  height: 100%;
  margin: 0px;
  -webkit-font-smoothing: antialiased;
  text-rendering: optimizeLegibility;
  font-family: 'Arvo', serif;
}

.heading {
  font-size: 40px;
}
.sub-heading {
  font-size: 20px;
  color: black;
}
img {
  border-radius: 5%;
  margin: 20px 0px;
}
.enlarge {
  object-fit: cover;
}

.embed {
  position: relative;
  text-align: center;
}
.bottom {
  position: inherit;
  display: flex;
  text-align: center;
  bottom: 60px;
  justify-content: center;
  font-size: 16px;
  color: #ffb703;
  box-shadow: inset 0 0 0 2000px rgba(15, 1, 9, 0.4);
}
.m-b{
  margin-bottom: -30px;
}
.modal-head {
  background: #ffb703;
  color: white;
}

section:nth-last-child(odd) {
  background: #e2eafc;
}



/* --------------------------------------
   Media Queries
   -------------------------------------- */

/*  Small devices (landscape phones, 576px and up) */
@media (min-width: 576px) {

}

/* Medium devices (tablets, 768px and up) */
@media (min-width: 768px) {
}

/* Large devices (desktops, 992px and up) */
@media (min-width: 992px) {
  .enlarge {
    margin-top: 20px;
    height: 22rem;
  }
  .bottom {
    font-size: 22px;
    bottom: 60px;
  }
}

/* Extra large devices (large desktops, 1200px and up) */
@media (min-width: 1200px) {
}
</style>
