// needs to update msg for cases without energy, fun or toilet
<script>
export default {
  data() {
    return {
      img: "/assets/default.gif",
      audio: {
        song: new Audio("/assets/candy.mp3"),
        playing: true
      },
      current: {
        activity: {
          eat: ["ramen", "pizza", "egg", "sushi", "cookie", "peeps", "hair", "ice-cream", "everything"],
          dress: ["naked", "bread", "crocodile", "Mexican", "female"],
          play: ["dancing", "wrestling", "raving", "writing", "painting", "calling", "blogging"],
          is: "do nothing"
        },
        counter: 0,
        arrLength: 0
      },
      choice: "Pusheen tamagotchi 🐈",
      barStatus: {
        energy: 50,
        toilet: 50,
        fun: 50,
        active: "inactive"
      }
    };
  },
  methods: {
    music(track) {
      if (this.audio.playing) {
        track.play();
      } else {
        track.pause();
      }
      this.audio.playing = !this.audio.playing;
    },
    chooseActivity(input) {
      let string = input;
      switch (input) {
        case "sleep":
          this.choice = "sleeping 😴";
          this.current.arrLength = 0;
          this.current.activity.is = "sleeping";
          break;
        case "toilet":
          this.choice = "pooping 💩";
          this.current.arrLength = 0;
          this.current.activity.is = "usingToilet";
          break;
        case "eat":
          string = this.current.activity.eat[0];
          this.choice = string;
          this.current.arrLength = this.current.activity.eat.length;
          this.current.counter = 0;
          this.current.activity.is = "eating";
          break;
        case "dress":
          string = this.current.activity.dress[0];
          this.choice = string;
          this.current.arrLength = this.current.activity.dress.length;
          this.current.counter = 0;
          this.current.activity.is = "dressing";
          break;
        case "play":
          string = this.current.activity.play[0];
          this.choice = string;
          this.current.arrLength = this.current.activity.play.length;
          this.current.counter = 0;
          this.current.activity.is = "playing";
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
    changeImg(src) {
      this.img = "/assets/" + src + ".gif";
    },
    welcome(msg) {
      let doing = this.choice;
      let addLetterA = ["bread", "female", "crocodile"];
      let you = "You're now ";
      if (doing.slice(0, 4) === "need") you = "You "; 
      if (addLetterA.includes(doing)) doing = "a " + doing;
      if (doing === "Pusheen tamagotchi 🐈") doing = "our favourite user 😘";
      if (this.current.arrLength === this.current.activity.eat.length)
        doing = "eating " + doing;
      if (msg.slice(0, 12) === "Your current") {
        switch (msg.slice(13, 16)) {
          case "Ene":
            alert(msg + (100 - this.barStatus.energy) + " jumps/hour. " + you + doing+".");
            break;
          case "Fun":
            alert(msg + (100 - this.barStatus.fun) + " joy/neuron. " + you + doing+".");
            break;
          case "Toi":
            alert(msg + (100 - this.barStatus.toilet) + " poops/litter box. " + you + doing+".");
            break;
          // later customize message depending on bar level
        }
      }
      else {
        alert(msg + you + doing+".");
      }
    },
    changeBar(bar) {
      this.barStatus.active = bar; //then we go to watch: "barStatus.active" 
    },
    entropy() { //lowers every bar constantly since page load
      setInterval(() => {
        if (this.barStatus.energy < 100) this.barStatus.energy = this.barStatus.energy + 1;
        if (this.barStatus.fun < 100) this.barStatus.fun = this.barStatus.fun + 1;
        if (this.barStatus.toilet < 100) this.barStatus.toilet = this.barStatus.toilet + 1;
      }, 2000);
    },
    eatNow(action) {
      let increaseHealthbar = setInterval(() => {
        if (this.current.activity.is === "eating") {
          if (this.barStatus.energy === 100) this.barStatus.energy = 95; // need this to escape a bug with entropy changing it back to 100 and image to noEnergy.gif
          if (this.barStatus.energy >= 0) this.barStatus.energy = this.barStatus.energy - 1;
          if (this.barStatus.toilet <= 99) this.barStatus.toilet = this.barStatus.toilet + 1;
        } else {
          clearInterval(increaseHealthbar);
        }
      }, 250);
      increaseHealthbar;
    },
    sleepNow(action) {
      let increaseHealthbar = setInterval(() => {
        if (this.current.activity.is === "sleeping") {
          if (this.barStatus.energy === 100) this.barStatus.energy = 95;
          if (this.barStatus.energy >= 0) this.barStatus.energy = this.barStatus.energy - 1;
        } else {
          clearInterval(increaseHealthbar);
        }
      }, 500);
      increaseHealthbar;
    },
    useToiletNow(action) {
      let increaseHealthbar = setInterval(() => {
        if (this.current.activity.is === "usingToilet") {
          if (this.barStatus.toilet === 100) this.barStatus.toilet = 95;
          if (this.barStatus.toilet >= 0) this.barStatus.toilet = this.barStatus.toilet - 1;
        } else {
          clearInterval(increaseHealthbar);
        }
      }, 250);
      increaseHealthbar;
    },
    playNow(action) {
      let increaseHealthbar = setInterval(() => {
        if (this.current.activity.is === "playing") {
          if (this.barStatus.fun === 100) this.barStatus.fun = 95;
          if (this.barStatus.fun >= 0) this.barStatus.fun = this.barStatus.fun - 1;
          if (this.barStatus.energy <= 99) this.barStatus.energy = this.barStatus.energy + 1;
        } else {
          clearInterval(increaseHealthbar);
        }
      }, 250);
      increaseHealthbar;
    },
    reset() {
      this.barStatus.active = "inactive";
      this.current.activity.is = "do nothing";
      this.current.arrLength = 0;
      this.current.counter = 0;
    }
  },
  watch: {
    "current.counter"(NewValue) { //this one changes the image and choice field
      let obj = Object.values(this.current.activity);
      let string = "";
      for (let i = 0; i < obj.length; i++) {
        if (obj[i].includes(this.choice)) { //I checked if arrays in current.activity (eat, dress and play) include value to determine currently used activity - this is an unnecessary "flex", just to play with objects and arrays. Later I added current.activity.is to keep track of currently used activity
          string = obj[i][this.current.counter];
          this.choice = string;
          this.changeImg(string);
        }
      }
    },
    'current.activity.is'() {
      switch (this.current.activity.is) {
        case "playing": {
          this.playNow();
        }
          break;
        case "sleeping": {
          this.sleepNow();
        }
          break;
        case "usingToilet": {
          this.useToiletNow();
        }
          break;
        case "eating": {
          this.eatNow();
        }
          break;
      }
    },
    'barStatus.energy'() {
      if (this.barStatus.energy === 100) {
        this.changeImg("noEnergy");
        this.choice = "need energy";
        this.reset();
      };
    },
    'barStatus.fun'() {
      if (this.barStatus.fun === 100) {
        this.changeImg("noFun");
        this.choice = "need fun";
        this.reset();
      }
    },
    'barStatus.toilet'() {
      if (this.barStatus.toilet === 100) {
        this.changeImg("noToilet");
        this.choice = "need toilet";
        this.reset();
      }
    }
  },
  created: function () {
    this.entropy();
  },
};
</script>

<template>
  <div class="App">
    <div class="grid-container">
      <div class="grid-item" @click="chooseActivity('eat'), changeBar('eat')">
        Eat
      </div>
      <div class="grid-item image" @click="music(audio.song)">
        <img :src="img" />
      </div>
      <div class="grid-item column" @click="welcome('Your current Fun level is ')">
        Fun
        <div class="bar">
          <div class="bar-inside" :style="{ width: barStatus.fun + '%' }"></div>
        </div>
      </div>
      <div class="grid-item" @click="chooseActivity('sleep'), changeBar('sleep')">
        Sleep
      </div>
      <div class="grid-item column" @click="welcome('Your current Energy level is ')">
        Energy
        <div class="bar">
          <div class="bar-inside" :style="{ width: barStatus.energy + '%' }"></div>
        </div>
      </div>
      <div class="grid-item" @click="chooseActivity('play'), changeBar('play')">
        Play
      </div>
      <div class="grid-item column" @click="welcome('Your current Toilet level is ')">
        Toilet
        <div class="bar">
          <div class="bar-inside" :style="{ width: barStatus.toilet + '%' }"></div>
        </div>
      </div>
      <div class="grid-item" @click="chooseActivity('dress')">Dress</div>
      <div class="grid-item">
        <span class="arrow" v-if="current.arrLength !== 0" @click="changeChoice('left')">&lt;&lt;</span>
        <span class="choice" @click="welcome('Welcome to Pusheen tamagotchi 1.0.1 😻')">{{ choice }}</span>
        <span class="arrow" v-if="current.arrLength !== 0" @click="changeChoice('right')">&gt;&gt;</span>
      </div>
      <div class="grid-item" @click="chooseActivity('toilet'), changeBar('toilet')">
        Use toilet!
      </div>
    </div>
  </div>
</template>

<style scoped>

</style>
