<script >
export default {
  data(){
    return {
      img: '/src/assets/default.gif',
      candy: new Audio("src/assets/candy.mp3"),
      love: new Audio("src/assets/love.mp3"),
      playing: true
    }
  },
  methods: {
      music(track){
        if(this.playing){ 
          track.play(); 
        } else {
          track.pause();
        }
        this.playing = !this.playing;
      }
  }
}
</script>

<template>
  <div class="App">
      <div class="grid-container">
      <div class="grid-item"  @click="music(candy)">Eat</div>
        <div class="grid-item image" @click="music(love)" >
          <img :src="img">
        </div>
        <div class="grid-item">Fun
        <div class="bar">
            <div class="bar-inside"></div>
          </div>
        
        </div>
        <div class="grid-item">
          Sleep
        </div>
        <div class="grid-item">
          Energy
          <div class="bar">
            <div class="bar-inside"></div>
          </div>
        </div>
        <div class="grid-item">Play</div>
        <div class="grid-item">Toilet
        <div class="bar">
            <div class="bar-inside"></div>
          </div>
        </div>
        <div class="grid-item">Dress</div>
        <div class="grid-item">&lt;&lt; Your choice &gt;&gt;</div> 
        <div class="grid-item">Use toilet!</div>
      </div>
    </div>
</template>

<style scoped>
 
</style>
