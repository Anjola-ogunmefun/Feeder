<template>
  <section>
    <div class="container py-3">
      <div class="card mb-3 px-4" style="">
        <div class="row g-0">
          <div class="col-4">
            <img :src="image" class="img-fluid py-2" alt="restaurant image" />
          </div>
          <div class="col-8 inline">
            <div class="card-body">
              <h5 class="card-title">{{ name }}</h5>
                <p>ingredients</p>
                <ol class="card-text line-clamp">
                  <li v-for="ing in ingredient" :key="ing">{{ ing }}</li>
                </ol>
                <nuxt-link to="#" class="links" @click="uniqueId(uri)">see more..</nuxt-link>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
export default {
  props: ['name', 'image', 'ingredient', 'mealType', 'cusineType', 'uri'],
  data(){
    return{
      id:''
    }
  },
  methods:{
    uniqueId(uri){
      const idArr = uri.split('#')
      console.log('uri', idArr)
      this.id = idArr[1]
      console.log(this.id)
    }
  },
  // mounted(){
  //   console.log(this.uri)
  //     const idArr = this.uri.split('#')
  //     console.log('uri', idArr)
  //     this.id = idArr[1]
  //     console.log(this.id)
  // }


}
</script>

<style scoped>
.inline {
  display: flex;
  flex: 1;
}
img {
  object-fit: cover;
  object-position: center;
  height: 100%;
  border-radius: 10% !important;
}

.line-clamp{
   display: -webkit-box;
  -webkit-line-clamp: 5;
  -webkit-box-orient: vertical;  
  overflow: hidden;
}

.links{
  text-decoration: none;
}

/* Large devices (desktops, 992px and up) */
@media (min-width: 992px) {
  .card-title {
    font-size: 28px;
  }
  .card-text {
    font-size: 20px;
  }
}
</style>
