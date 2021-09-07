<template>
  <main>
    <transition name="modal">
      <base-dialog v-if="error" @close="error = false" :open="!!error">
        <p class="notFound text-center">Sorry, meal not found. Try again</p>
      </base-dialog>
    </transition>

    <section id="top">
      <div class="py-5">
        <figure class="text-center container">
          <blockquote class="blockquote">
            <p class="heading">Meal-prep</p>
          </blockquote>
          <figcaption class="blockquote-footer sub-heading notFound">
            Let's help you bring your meals to life
          </figcaption>
        </figure>
      </div>
    </section>

    <section id="gallery">
      <div class="container py-5">
        <div class="row m-b">
          <div class="col-md-12">
            <img
              src="@/assets/images/woman-cooking.jpeg"
              alt="fried rice"
              class="img-fluid mb-3"
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
              src="@/assets/images/combo.jpeg"
              alt="bread"
              class="img-fluid enlarge"
            />
            <!-- <div class="bottom">Takeaway deals</div> -->
          </div>
          <div class="col-md-3 col-6 embed">
            <img
              src="@/assets/images/rice.jpeg"
              alt="white rice"
              class="img-fluid enlarge"
            />
            <!-- <div class="bottom">Dine-in deals</div> -->
          </div>
        </div>
      </div>
    </section>

    <section id="instruction">
      <div class="container">
        <div class="row">
          <div class="col-12 text-center py-5">
            <h3>
              We love to answer your questions about the meal you want to
              try out
            </h3>
          </div>

          <input
            class="
              form-control form-control-lg
              mb-5
              text-center
              container
              placeholder
            "
            type="text"
            placeholder="Tell us what you want to cook..."
            aria-label=".form-control-lg example"
            v-model="query"
            @change="reqRecipe"
          />
        </div>
      </div>
    </section>

    <div v-if="isLoading" class="mb-5">
      <div class="d-flex justify-content-center">
        <h5><strong>Loading...</strong> &nbsp; &nbsp;</h5>
        <div class="spinner-border" role="status" aria-hidden="true"></div>
      </div>
    </div>

    <section v-if="recipeList.length != 0 && !details">
      <recipe-list
        v-for="(recipe, index) in recipeList"
        :key="index.recipe"
        :name="recipe.label"
        :uri="recipe.uri"
        :image="recipe.image"
        :ingredient="recipe.ingredientLines"
        :cusineType="recipe.cusineType"
        :mealType="recipe.mealType"
        @expand-details="showDetails"
      >
      </recipe-list>
    </section>

      <section v-else>
      <div class="container mb-2">
        <div class="row">
          <video controls loop autoplay muted>
            <source src="@/assets/video/cooking-video.mp4" type="video/mp4" />
          </video>
        </div>
      </div>
    </section>

    <section v-if="details">
      <more-details
        :name="name"
        :dietLabels="dietLabels"
        :heathLabels="healthLabels"
        :cautions="cautions"
        :mealType="mealType"
        :dishType="dishType"
        :ingredients="ingredients"
        :link="link"
        :image="image"
      >
      </more-details>
    </section>
  </main>
</template>

<script>
const axios = require('axios')

export default {
  data() {
    return {
      query: '',
      recipeList: [],
      details: false,
      id: '',
      name: '',
      dietLabels: [],
      healthLabels: [],
      cautions: [],
      mealType: [],
      dishType: [],
      ingredients: [],
      link: '',
      image: '',
      isLoading: false,
      error: null,
    }
  },

  methods: {
    async reqRecipe() {
      this.isLoading = true
      try {
        this.details = false
        this.recipeList = []
        const response = await axios.get(
          `https://api.edamam.com/api/recipes/v2?type=public&q=${this.query}&app_id=abb2f4d9&app_key=%20f495acc1dfcdca5525d3f6cc7cf86997`
        )
        const responseData = response.data.hits
        if (responseData.length <= 0) {
          this.isLoading = false
          this.error = true
          throw new Error('meal not found')
        }
        console.log(responseData)

        for (const key in responseData) {
          this.recipeList.push(responseData[key].recipe)
          console.log(this.recipeList)
        }
        this.isLoading = false
      } catch (error) {
        this.isLoading = false
        this.error = true
        // throw new Error('Something went wrong')
      }
    },

    async showDetails(uri) {
      this.details = true
      this.isLoading = true
      const idArr = uri.split('#')
      console.log('uri', idArr)
      this.id = idArr[1]
      console.log(this.id, 'id')
      try {
        const response = await axios.get(
          `https://api.edamam.com/api/recipes/v2/${this.id}?type=public&app_id=abb2f4d9&app_key=f495acc1dfcdca5525d3f6cc7cf86997`
        )
        const recipe = response.data.recipe
        this.name = recipe.label
        this.dietLabels = recipe.dietLabels
        this.healthLabels = recipe.healthLabels
        this.cautions = recipe.cautions
        this.mealType = recipe.mealType
        this.dishType = recipe.dishType
        this.ingredients = recipe.ingredientLines
        this.link = recipe.url
        this.image = recipe.image

        this.isLoading = false
        console.log(recipe)
      } catch (error) {
        this.isLoading = false
        this.error = true
        error = new Error('Something went wrong')
        throw error
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
  overflow-x: hidden;
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
.m-b {
  margin-bottom: -30px;
}
.modal-head {
  background: #0077b6;
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
  .notFound {
    font-size: 1.2rem;
  }
}

/* Medium devices (tablets, 768px and up) */
@media (min-width: 768px) {
  .enlarge {
    margin-top: 8px;
    height: 11rem;
  }

  .placeholder {
    font-size: 1.5rem;
  }
}

/* Large devices (desktops, 992px and up) */
@media (min-width: 992px) {
  .enlarge {
    margin-top: 5px;
    height: 22rem;
  }
  h3 {
    font-size: 2rem;
  }
  .bottom {
    font-size: 22px;
    bottom: 60px;
  }
  .placeholder {
    font-size: 1.8rem;
  }
  .notFound {
    font-size: 1.5rem;
  }
}

/* Extra large devices (large desktops, 1200px and up) */
@media (min-width: 1200px) {
}
</style>
