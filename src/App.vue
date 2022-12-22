<script>
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
      choice: "Your choice",
      active: ["dress", "eat", "play"],
      dress: {
        name: ["naked", "bread", "crocodile", "mexico", "female"],
        counter: 0,
      },
      eat: {
        name: [
          "ramen",
          "pizza",
          "egg",
          "sushi",
          "cookie",
          "peeps",
          "hair",
          "ice-cream",
          "everything",
        ],
        counter: 0,
      },
      play: {
        name: [
          "dance",
          "play",
          "rave" /*play music*/,
          "write",
          "paint",
          "call",
          "blog",
        ],
        counter: 0,
      },
    };
  },
  methods: {
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
    choose(input) {
      switch (input) {
        case "dress":
          this.choice = this.dress.name[this.dress.counter];
          break;
        case "eat":
          this.choice = this.eat.name[this.eat.counter];
          break;
        case "play":
          this.choice = this.play.name[this.play.counter];
          break;
      }
    },
    changeChoice(input) {
      switch (input) {
        case "left":
          if (this.dress.counter > 0) { //fix that part tomorrow
            this.dress.counter = this.dress.counter - 1;
            this.choose("dress");
            this.img = "/src/assets/bread.gif";
          } else {
            this.dress.counter = this.dress.name.length - 1;
          }
          break;
        case "right":
          if (this.dress.counter < this.dress.name.length - 1) {
            this.dress.counter = this.dress.counter + 1;
            this.choose("dress");
          } else {
            this.dress.counter = 0;
          }
          break;
      }
    },
  },
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
        <span @click="changeChoice('left')">&lt;&lt;</span> <span class="choice">{{ choice }}</span>
        <span @click="changeChoice('right')">&gt;&gt;</span>
      </div>
      <div class="grid-item">Use toilet!</div>
    </div>
  </div>
</template>

<style scoped></style>
