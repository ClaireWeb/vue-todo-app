<template>
  <div id="app">
    <carousel>
      <!-- <carousel-slide>
        <div class="carousel__caption">Hello</div>
        <img src="https://picsum.photos/id/237/830/400" alt="dog" width="100%" />
      </carousel-slide>
      <carousel-slide>
        <div class="carousel__caption">Goodbye</div>
        <img src="https://picsum.photos/id/1002/830/400" alt="landscape" width="100%" />
      </carousel-slide>
      <carousel-slide>
        <div class="carousel__caption">Hello</div>
        <img src="https://picsum.photos/id/1024/830/400" alt="vautour" width="100%" />
      </carousel-slide>-->
      <carousel-slide v-for="n in slides" :key="n" :index="n - 1">
        <div class="carousel__caption">Slide {{ n }}</div>
        <img
          :src="'https://picsum.photos/830/400?random=' + n"
          alt="random"
          width="100%"
        />
      </carousel-slide>
    </carousel>
    <button @click="addSlide">Ajouter un slide</button>
    <button @click="removeSlide">Enlever un slide</button>

    <Todos v-model="todos" />
    <Todos />
    <Todos />
    <button @click="addTodo">Ajouter une tâche</button>
  </div>
</template>

<script>
import Todos from "./components/Todos";
import Carousel from "./components/carousel/Carousel";
import CarouselSlide from "./components/carousel/CarouselSlide";

export default {
  name: "App",
  data() {
    return {
      slides: 5,
      todos: [
        {
          name: "Demo",
          completed: true
        }
      ]
    };
  },
  components: {
    Todos,
    Carousel,
    CarouselSlide
  },
  methods: {
    addTodo() {
      this.todos.push({
        name: "Nouvelle tâche",
        completed: false
      });
    },
    addSlide() {
      this.slides++;
    },
    removeSlide() {
      this.slides--;
    }
  }
};
</script>

<style>
.carousel__caption {
  position: absolute;
  left: 0;
  right: 0;
  text-align: center;
  top: 50%;
  color: aliceblue;
  font-size: 16px;
}
</style>
