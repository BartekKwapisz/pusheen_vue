<script>
// prevent span arrow from selecting text when clicking
//try performance.now()?
// still to add: picture change and bar mechanic
export default {
  data() {
    return { 
      img: "/src/assets/default.gif",
      imgDefault: "/src/assets/default.gif",
      candy: new Audio("src/assets/candy.mp3"),
      love: new Audio("src/assets/love.mp3"),
      playing: true,
      energy: 50,
      toilet: 70,
      fun: 30,
      choice: "Pusheen tamagotchi",
      current: {
          activity: {
            eat: ["ramen","pizza","egg","sushi","cookie","peeps","hair","ice-cream","everything"],
            dress: ["naked", "bread", "crocodile", "mexico", "female"],
            play: ["dance","play","rave","write","paint","call","blog"]
          },
          counter: 0,
          arrLength: 0
      },
    };
  },
  methods: {
    choose(input) {  
      this.bulik = true;
      switch(input){
        case "eat": this.choice = this.current.activity.eat[0]; 
                    this.current.arrLength = this.current.activity.eat.length;
                    this.current.counter = 0;
        break;
        case "dress": this.choice = this.current.activity.dress[0];
                      this.current.arrLength = this.current.activity.dress.length; 
                      this.current.counter = 0;
        break;
        case "play": this.choice = this.current.activity.play[0]; 
                     this.current.arrLength = this.current.activity.play.length;
                     this.current.counter = 0;
        break;
      }
    },
    changeChoice(input) {
      switch (input) {
        case "right":  
          if(this.current.counter < this.current.arrLength-1){
            this.current.counter = this.current.counter+1; 
          } else {
            this.current.counter = 0;
          }
        break;
        case "left":  
          if(this.current.counter > 0){
            this.current.counter = this.current.counter-1;
          } else {   
            this.current.counter = this.current.arrLength-1;
          }
        break;
      } 
    },
    music(track) {
      if (this.playing) {
        track.play();
      } else {
        track.pause();
      }
      this.playing = !this.playing;
    },
    sleep(src, bar) {
      this.img = src;
      let foo = "this."+bar+"="+"this."+bar+"-20"; // results with: this.energy = this.energy - 20;
      console.log(typeof eval(foo));
      eval(foo);
      // const increaseHealthbar = setInterval(() => {
      //   if (this.energy > 0) {
      //     this.energy = eval("this." + bar) - 10;
      //   } else {
      //     setTimeout(() => {
      //       this.img = "/src/assets/default.gif";
      //     }, 1000);
      //     clearInterval(increaseHealthbar);
      //   }
      // }, 1000);
      // increaseHealthbar;
    },
    
  },
  watch: { 
    'current.counter'(NewValue){
      let obj = Object.values(this.current.activity);
      for(let i=0; i<obj.length; i++){
        if(obj[i].includes(this.choice)) { 
          this.choice = obj[i][this.current.counter];
        }
      }
    }
  }
};
</script>

<template>
  <div class="App">
    <div class="grid-container">
      <div class="grid-item" @click="choose('eat')">Eat</div>
      <div class="grid-item image" @click="music(candy)">
        <img :src="img" />
      </div>
      <div class="grid-item column">
        Fun
        <div class="bar">
          <div class="bar-inside" :style="{ width: fun + '%' }"></div>
        </div>
      </div>
      <div class="grid-item" @click="sleep('/src/assets/sleep.gif', 'energy')">
        Sleep
      </div>
      <div class="grid-item column">
        Energy
        <div class="bar">
          <div class="bar-inside" :style="{ width: energy + '%' }"></div>
        </div>
      </div>
      <div class="grid-item" @click="choose('play')">Play</div>
      <div class="grid-item column">
        Toilet
        <div class="bar">
          <div class="bar-inside" :style="{ width: toilet + '%' }"></div>
        </div>
      </div>
      <div class="grid-item" @click="choose('dress')">Dress</div>
      <div class="grid-item">
        <span v-if="current.arrLength !== 0" @click="changeChoice('left')">&lt;&lt;</span> <span class="choice">{{ choice }}</span>
        <span v-if="current.arrLength !== 0" @click="changeChoice('right')">&gt;&gt;</span>
      </div>
      <div class="grid-item">Use toilet!</div>
    </div>
  </div>
</template>

<style scoped></style>
