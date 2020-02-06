<template>
  <div class="carousel">
    <slot></slot>
    <button class="carousel__nav carousel__next" @click.prevent="next"></button>
    <button class="carousel__nav carousel__prev" @click.prevent="prev"></button>
    <div class="carousel__pagination">
      <button v-for="n in slidesCount" :key="n" @click="goTo(n-1)" :class="{active: n-1 == index}"></button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      index: 0,
      slides: [],
      direction: "right"
    };
  },
  watch: {
    slides(slides) {
      if (this.index >= this.slidesCount) {
        this.index = this.slidesCount - 1;
      }
    }
  },
  computed: {
    slidesCount() {
      return this.slides.length;
    }
  },
  methods: {
    next() {
      this.index++;
      this.direction = "right";
      if (this.index >= this.slidesCount) {
        this.index = 0;
      }
    },
    prev() {
      this.index--;
      this.direction = "left";
      if (this.index < 0) {
        this.index = this.slidesCount - 1;
      }
    },
    goTo(index) {
      this.direction = index > this.index ? "right" : "left";
      this.index = index;
    }
  },
  mounted() {
    this.slides = this.$children;
    // Pour ne pas injecter l'index. On passe en props dans l'enfant
    // this.slides.forEach((slide, i) => {
    //   slide.index = i;
    // });
  }
};
</script>

<style scoped>
.carousel {
  position: relative;
  overflow: hidden;
}
.carousel__nav {
  position: absolute;
  top: 50%;
  margin-top: -31px;
  left: 10px;
  background-image: url(prev.png);
  width: 63px;
  height: 63px;
}
.carousel__nav.carousel__next {
  right: 10px;
  left: auto;
  background-image: url(next.png);
}
.carousel__pagination {
  position: absolute;
  bottom: 10px;
  left: 0;
  right: 0;
  text-align: center;
}
.carousel__pagination button {
  display: inline-block;
  width: 10px;
  height: 10px;
  background-color: orange;
  opacity: 0.8;
  border-radius: 10px;
  margin: 10px 10px;
}
.carousel__pagination button.active {
  background-color: #fff;
}
</style>
