<template>
  <div class="gallery">
    <div class="gallery__image">
      <img :src="images[currentIndex].src" class="gallery__image--active">
        <div class="gallery__image__overlay">
          <h2 class="gallery__image__overlay__title">
            {{ images[currentIndex].title }}
        </h2>
          <p class="gallery__image__overlay__description">
            {{ images[currentIndex].description }}
        </p>
        </div>
      </div>
      <div class="gallery__navigation">
        <button class="gallery__navigation__button" @click="() => prevImage()">
            &#10094;
        </button>
        <div class="gallery__navigation__thumbnails">
            <div v-for="(image, index) in displayedImages" :key="image.id" 
                class="gallery__navigation__thumbnails__thumbnail"
                :class="{ 'gallery__navigation__thumbnails__thumbnail--selected': image.id === images[currentIndex].id }"
                @click="() => setCurrentIndex(index)">
                <img :src="image.src" :alt="image.title" class="gallery__navigation__thumbnails__thumbnail--img">
            </div>
        </div>
        <button class="gallery__navigation__button" @click="() => nextImage()">
            &#10095;
        </button>
      </div>
    </div>
</template>


<script>
export default {
name: 'GaleryComponent',
  props: {
    images: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      currentIndex: 0,
    }
  },
  computed: {
    displayedImages () {
      return this.images
    }
  },
  methods: {
    prevImage () {
      this.currentIndex = (this.currentIndex - 1 + this.images.length) % this.images.length
    },
    nextImage () {
      this.currentIndex = (this.currentIndex + 1) % this.images.length
    },
    setCurrentIndex (index) {
      this.currentIndex = index
    }
  }
}
</script>


<style scoped lang="less">
.gallery {
  position: relative;
  width: 100%;
  max-width: 800px;
  margin: auto;

  &__image {
    position: relative; 

    &--active {
      width: 100%;
      height: auto;
      display: block;
    }

    &__overlay {
      position: absolute;
      bottom: 0;
      left: 0;
      right: 0;
      text-align: center;
      background: linear-gradient(to top, rgba(0, 0, 0, 0.7) 0%, rgba(0, 0, 0, 0) 100%);
      padding: 20px;

      &__title,
      &__description {
        color: white;
      }
    }
  }

  &__navigation {
    display: flex;
    align-items: center;
    justify-content: center;
    margin-top: 10px;

    &__button {
      background: none;
      border: none;
      font-size: 24px;
      cursor: pointer;
      padding: 0;
      color: white;
      background: rgba(0, 0, 0, 0.5);
      border-radius: 50%;
      width: 48px;
      height: 48px;
      display: flex;
      align-items: center;
      justify-content: center;

      &:hover {
        background: rgba(0, 0, 0, 0.7);
      }
    }

    &__thumbnails {
      display: flex;
      flex-wrap: nowrap;
      overflow-x: hidden;
      justify-content: center;
      margin: 0 auto;

      &__thumbnail {
        transition: transform 0.3s ease, opacity 0.3s ease;
        display: flex;
        align-items: center;
        justify-content: center;

        &--selected {
          .gallery__navigation__thumbnails__thumbnail--img {
            width: 80px;
            opacity: 1;
          }
        }

        &--img {
          width: 60px;
          height: auto;
          margin: 2px;
          opacity: 0.6;
        }
      }
    }
  }
}
</style>
