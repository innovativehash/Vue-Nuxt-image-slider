<template>
  <v-layout column justify-center align-center>
    <v-flex xs12 md6 class="slider-container">
      <a @click="prev" class="navigate-icon slide-prev"></a>
        
      <transition-group name='fade' tag='div' v-if="isImageLoaded">
        <v-flex class="slide-item" :key="imageIndex" xs12 d-flex>
          <v-card flat tile class="d-flex">
            <v-img
              :src="this.images[imageIndex]"
              :lazy-src="this.images[imageIndex]"
            >
              <v-layout
                slot="placeholder"
                fill-height
                align-center
                justify-center
                ma-0
              >
                <v-progress-circular indeterminate color="grey lighten-5"></v-progress-circular>
              </v-layout>
            </v-img>
          </v-card>
        </v-flex>
      </transition-group>

      <a @click="next" class="navigate-icon slide-next"></a>
    </v-flex>
  </v-layout>
</template>

<script>
import axios from 'axios'

export default {
  data: function() {
    return {
      isImageLoaded: false,
      images: [],
      imageIndex: 0,
    }
  },    
  methods: {
    next: function() {
        this.imageIndex = (this.imageIndex + 1) < this.images.length ? this.imageIndex + 1 : 0
    },
    prev: function() {
        this.imageIndex = (this.imageIndex - 1) >= 0 ? this.imageIndex - 1 : this.images.length - 1
    }
  },
  mounted: function() {
    axios.get('http://www.mocky.io/v2/5b753d5f2e00006900535f6c').then(({data}) => {
      const { images } = data
      this.isImageLoaded = true
      this.images = images || []
    })
  }
}
</script>

<style>
.slider-container {
  min-width: 800px;
  min-height: 600px;
  position: relative;
}
.navigate-icon {
  position: absolute;
  top: 50%;
}
.navigate-icon:after {
  content: '';
  display: block;
  width: 25px;
  height: 25px;
  background: transparent;
  border-style: solid;
  border-width: 2px;
}
.slide-prev {
  left: -40px;
}
.slide-prev:after {
  border-color: #fff transparent transparent #fff;
  transform: rotate(-45deg);
}
.slide-next {
  right: -40px;
}
.slide-next:after {
  border-color: #fff #fff transparent transparent;
  transform: rotate(45deg);
}

.slide-item {
  width: 100%;
  height: 100%;
}
.fade-enter-active, .fade-leave-active {
  transition: all 0.8s ease;
  overflow: hidden;
  visibility: visible;
  opacity: 1;
  position: absolute;
}
.fade-enter, .fade-leave-to {
  opacity: 0;
  visibility: hidden;
}
</style>