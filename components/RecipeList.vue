<template>
  <section>
    <div class="container py-3">
      <!-- <div class="card">
        <img :src="image" class="img-fluid" alt="image" />
        <div class="card-body">
          <h5 class="card-title">{{ name }}</h5>
                <p>Ingredients</p>
                <ol class="card-text line-clamp">
                  <li v-for="ing in ingredient" :key="ing">{{ ing }}</li>
                </ol>
                  <p class="text-primary" @click="show">see more...</p>
        </div>
      </div> -->
      <div class="card mb-3 px-4" style="">
        <div class="row g-0">
          <div class="col-5">
            <img :src="image" class="img-fluid py-2" alt="restaurant image" />
          </div>
          <div class="col-7">
            <div class="card-body ml">
              <h5 class="card-title"><strong>{{ name }}</strong></h5>
                <p>Ingredients</p>
                <ol class="card-text line-clamp">
                  <li v-for="(ing, index) in ingredient" :key="index">{{ ing }}</li>
                </ol>
                  <p class="text-primary more" @click="show">see more...</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
const axios = require('axios')

export default {
  props: ['name', 'image', 'ingredient', 'mealType', 'cusineType', 'uri'],

  methods: {
    show() {
       console.log(this.uri)
      // $nuxt.$emit('more-details', this.uri)
       this.$emit('expand-details', this.uri)
    },
  },
}
</script>

<style scoped>
.inline {
  display: flex;
  flex: 1;
}
.more:hover{
  cursor: pointer;
}

img {
  object-fit: cover;
  object-position: center;
  height: 100%;
}

.line-clamp {
  display: -webkit-box;
  -webkit-line-clamp: 5;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

.links {
  text-decoration: none;
}

/* Large devices (desktops, 992px and up) */
@media (min-width: 992px) {
  .card-title {
    font-size: 28px;
  }
  .card-text,p{
    font-size: 20px;
  }
  .ml{
    margin-left: -10rem;
  }
}
</style>
