<script> 
// still to add: eating double increase energy, decrease toilet, playing decrease energy (just add another functions for that) 
//pictures and alerts on low level (another watcher)
// prevent covering bar from taking 100% width
export default {
  data() {
    return {
      img: "/src/assets/default.gif",
      imgDefault: "/src/assets/default.gif",
      candy: new Audio("src/assets/candy.mp3"),
      playing: true,
      barStatus: {
        energy: 50,
        toilet: 50,
        fun: 50,
        active: "inactive",
      },
      choice: "Pusheen tamagotchi 🐈",
      current: {
        activity: {
          eat: [
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
          dress: ["naked", "bread", "crocodile", "Mexican", "female"],
          play: [
            "dancing",
            "playing",
            "raving",
            "writing",
            "painting",
            "calling",
            "blogging",
          ],
        },
        counter: 0,
        arrLength: 0,
      },
    };
  },
  methods: {
    inevitableFlowOfTime(){
      setInterval(() => {
        this.barStatus.energy = this.barStatus.energy+1;
        this.barStatus.fun = this.barStatus.fun+1;
        this.barStatus.toilet = this.barStatus.toilet+1;
      }, 1000);
    },
    choose(input) {
      let string = "";
      switch (input) {
        case "eat":
          string = this.current.activity.eat[0];
          this.choice = string;
          this.current.arrLength = this.current.activity.eat.length;
          this.current.counter = 0;
          break;
        case "dress":
          string = this.current.activity.dress[0];
          this.choice = string;
          this.current.arrLength = this.current.activity.dress.length;
          this.current.counter = 0;
          break;
        case "play":
          string = this.current.activity.play[0];
          this.choice = string;
          this.current.arrLength = this.current.activity.play.length;
          this.current.counter = 0;
          break;
      }
      this.changeImg(string);
    },
    changeChoice(input) {
      switch (input) {
        case "right":
          if (this.current.counter < this.current.arrLength - 1) {
            this.current.counter = this.current.counter + 1;
          } else {
            this.current.counter = 0;
          }
          break;
        case "left":
          if (this.current.counter > 0) {
            this.current.counter = this.current.counter - 1;
          } else {
            this.current.counter = this.current.arrLength - 1;
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
    changeImg(src) {
      this.img = "/src/assets/" + src + ".gif";
      switch (src) {
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
      this.barStatus.active = bar;
    },
    welcome(msg) {
      let doing = this.choice;
      let addA = ["bread", "female", "crocodile"];
      if (addA.includes(doing)) doing = "a " + doing;
      if (doing === "Pusheen tamagotchi 🐈") doing = "our favourite user 😘";
      if (this.current.arrLength === this.current.activity.eat.length)
        doing = "eating " + doing;
      if (msg.slice(0, 12) === "Your current") {
        switch(msg.slice(13, 16)) {
          case "Ene": alert(msg + (100-this.barStatus.energy) + " jumps/hour. You're now " + doing);
          break;
          case "Fun": alert(msg + (100-this.barStatus.fun) + " joy/hour. You're now " + doing);
          break;
          case "Toi": alert(msg + (100-this.barStatus.toilet) + " poops/litter box. You're now " + doing);
          break;
          // later customize message depending on bar level
        }
      } else {
        alert(msg + "You're now " + doing);
      }
    },
  },
  watch: {
    "current.counter"(NewValue) {
      let obj = Object.values(this.current.activity);
      let string = "";
      for (let i = 0; i < obj.length; i++) {
        if (obj[i].includes(this.choice)) {
          string = obj[i][this.current.counter];
          this.choice = string;
          this.changeImg(string);
        }
      } //add changeImg(src)
    },
    "barStatus.active"() {
      switch (this.barStatus.active) {
        case "energy":
          {
            let increaseHealthbar = setInterval(() => {
              if (this.barStatus.active === "energy" && this.barStatus.energy > 0) {
                this.barStatus.energy = this.barStatus.energy - 3;
              } else if(this.barStatus.energy === 0){
                alert(
                  "You're full of energy and ready for the day full of adventures. Good job! 👏"
                );
                clearInterval(increaseHealthbar);
              }
              else {
                clearInterval(increaseHealthbar);
              }
            }, 1000);
            increaseHealthbar;
          }
          break;
        case "fun":
          {
            let increaseHealthbar = setInterval(() => {
              if (this.barStatus.active === "fun" && this.barStatus.fun > 0) {
                this.barStatus.fun = this.barStatus.fun - 3;
              } else if (this.barStatus.fun === 0) {
                alert(
                  "Your current entertainment level is optimal, and you're a very happy cat. Good job! 👏"
                );
                clearInterval(increaseHealthbar);
              }
              else {
                clearInterval(increaseHealthbar);
              }
            }, 1000);
            increaseHealthbar;
          }
          break;
        case "toilet": {
          let increaseHealthbar = setInterval(() => {
            if (this.barStatus.active === "toilet" && this.barStatus.toilet > 0) {
              this.barStatus.toilet = this.barStatus.toilet - 3;
            } else if (this.barStatus.toilet === 0) {
              alert(
                "You no longer need to use a toilet. Good job! 👏 Maybe it's time to eat something?🤔"
              );
              clearInterval(increaseHealthbar);
            }
            else {
              clearInterval(increaseHealthbar);
            }
          }, 1000);
          increaseHealthbar;
        }
      }
    },
  },
  created: function(){
        this.inevitableFlowOfTime();
  }
};
</script>

<template>
  <div class="App">
    <div class="grid-container">
      <div class="grid-item" @click="choose('eat'), changeBar('energy')">
        Eat
      </div>
      <div class="grid-item image" @click="music(candy)">
        <img :src="img" />
      </div>
      <div
        class="grid-item column"
        @click="welcome('Your current Fun level is ')"
      >
        Fun
        <div class="bar">
          <div class="bar-inside" :style="{ width: barStatus.fun + '%' }"></div>
        </div>
      </div>
      <div class="grid-item" @click="changeImg('sleep'), changeBar('energy')">
        Sleep
      </div>
      <div
        class="grid-item column"
        @click="welcome('Your current Energy level is ')"
      >
        Energy
        <div class="bar">
          <div
            class="bar-inside"
            :style="{ width: barStatus.energy + '%' }"
          ></div>
        </div>
      </div>
      <div class="grid-item" @click="choose('play'), changeBar('fun')">
        Play
      </div>
      <div
        class="grid-item column"
        @click="welcome('Your current Toilet level is ')"
      >
        Toilet
        <div class="bar">
          <div
            class="bar-inside"
            :style="{ width: barStatus.toilet + '%' }"
          ></div>
        </div>
      </div>
      <div class="grid-item" @click="choose('dress')">Dress</div>
      <div class="grid-item">
        <span
          class="arrow"
          v-if="current.arrLength !== 0"
          @click="changeChoice('left')"
          >&lt;&lt;</span
        >
        <span
          class="choice"
          @click="welcome('Welcome to Pusheen tamagotchi 1.0.0 😻')"
          >{{ choice }}</span
        >
        <span
          class="arrow"
          v-if="current.arrLength !== 0"
          @click="changeChoice('right')"
          >&gt;&gt;</span
        >
      </div>
      <div class="grid-item" @click="changeImg('happy'), changeBar('toilet')">
        Use toilet!
      </div>
    </div>
  </div>
</template>

<style scoped></style>
