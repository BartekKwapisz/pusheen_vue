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
      playing: true,
      energy: 54,
      toilet: 70,
      fun: 30,
      choice: "Pusheen tamagotchi 🐈",
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
      let string = "";
      switch(input){
        case "eat": string = this.current.activity.eat[0];
                    this.choice = string; 
                    this.current.arrLength = this.current.activity.eat.length;
                    this.current.counter = 0;
        break;
        case "dress": string = this.current.activity.dress[0];
                      this.choice = string;
                      this.current.arrLength = this.current.activity.dress.length; 
                      this.current.counter = 0;
        break;
        case "play": string = this.current.activity.play[0];
                     this.choice =  string
                     this.current.arrLength = this.current.activity.play.length;
                     this.current.counter = 0;
        break;
      }
      this.changeImg(string);
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
    changeImg(src){
      this.img = "/src/assets/"+src+".gif"; 
      switch(src) {
        case "sleep":
          this.choice = "sleeping 😴";
          this.current.arrLength = 0;
          break;
        case "happy":
          this.choice = "pooping 💩";
          this.current.arrLength = 0;
          break;
      }
    },
    changeBar(bar) { 
      let foo = "this."+bar+"="+"this."+bar+"-10"; // results with: this.energy = this.energy - 20; issue with that - it won't go up to 0
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
    welcome(msg){
      alert(msg);
    }
  },
  watch: { 
    'current.counter'(NewValue){
      let obj = Object.values(this.current.activity);
      let string = "";
      for(let i=0; i<obj.length; i++){
        if(obj[i].includes(this.choice)) { 
          string = obj[i][this.current.counter]
          this.choice = string;
          this.changeImg(string);
        }
      } //add changeImg(src)
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
      <div class="grid-item column" @click="welcome('Your current Fun level is ...')">
        Fun
        <div class="bar">
          <div class="bar-inside" :style="{ width: fun + '%' }"></div>
        </div>
      </div>
      <div class="grid-item" @click="changeImg('sleep'), changeBar('energy')">
        Sleep
      </div>
      <div class="grid-item column" @click="welcome('Your current Energy level is ...')">
        Energy
        <div class="bar">
          <div class="bar-inside" :style="{ width: energy + '%' }"></div>
        </div>
      </div>
      <div class="grid-item" @click="choose('play')">Play</div>
      <div class="grid-item column" @click="welcome('Your current Toilet level is ...')">
        Toilet
        <div class="bar">
          <div class="bar-inside" :style="{ width: toilet + '%' }"></div>
        </div>
      </div>
      <div class="grid-item" @click="choose('dress')">Dress</div>
      <div class="grid-item">
        <span class="arrow" v-if="current.arrLength !== 0" @click="changeChoice('left')">&lt;&lt;</span> <span class="choice" @click="welcome('Welcome to Pusheen tamagotchi 1.0.0 😻')">{{ choice }}</span>
        <span class="arrow" v-if="current.arrLength !== 0" @click="changeChoice('right')">&gt;&gt;</span>
      </div>
      <div class="grid-item" @click="changeImg('happy')">Use toilet!</div>
    </div>
  </div>
</template>

<style scoped></style>
