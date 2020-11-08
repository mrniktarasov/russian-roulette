<template>
  <div id="app" class="screen">
    <div class="annotation">Russian roulette</div>
    <carousel :cells="cells"
              :rotateStyle="rotateStyle"
              :cylinder = "cylinder"
              :fired = 'fired'
    ></carousel>
    <button class="button"
            @click="load"
    >Load{{loadedBullets}}</button>
    <button class="button"
            v-on:click="rotateCarousel"
    >Rotate</button>
    <button class="button"
            @click="fire"
    >Fire!</button>
      <div  class='message'
            v-if="message"
            :style="messageStyle"
      >{{message}}</div>
  </div>
</template>

<script>
import carousel from "@/components/carousel";
import sounds from "@/audio/sounds";

let App = {
    name: 'App',
    data: function() {
        return {
            cells: new Array(6).fill('?'),
            selectedIndex: 0,
            position: 0,
            bullets: 0,
            cylinder: new Array(6).fill(false),
            message: '',
            audioPath: './audio/',
            fired: false,
            messageStyle: {
                'color': 'black',
            },
            rotateStyleInit: {
                'transform': null,
            }
        }
    },
    methods: {
        getRandomInt: function(min, max) {
            return Math.floor(min + Math.random()*(max + 1 - min));
        },
        rotateCarousel: function() {
            this.clearData();
            this.selectedIndex++;
            this.position++;
            if(this.position > 6) {
                this.position = 0;
            }
            //let randRounds =  this.getRandomInt(1, Math.round(this.cells.length/2));
            let angle = this.selectedIndex / this.cells.length * -360 ;
            this.playAudio(sounds.spinning);
            this.rotateStyle.transform = `translateZ(-199px) rotateY(${angle}deg)`;
        },
        load: function() {
            this.clearData();
            if(this.bullets < 5) {
                if(!this.cylinder[this.position]) {
                    this.$set(this.cylinder, this.position, true);
                    this.bullets++;
                    this.playAudio(sounds.load)
                } else {
                    this.message = 'This chamber is already full!';
                }
            }
        },
        clearData: function() {
            if(this.fired) {
              this.bullets = 0;
              this.position = 0;
              this.cylinder.fill(false);
              this.fired = false;
            }
            this.message = '';
        },
        fire: function() {
            this.fired = true;
            if (this.cylinder[this.position]) {
                this.message = 'You are dead!';
                this.messageStyle.color = 'red';
                this.playAudio(sounds.fire);
            } else {
                this.playAudio(sounds.dryFire);
                this.message = 'You are lucky!';
                this.messageStyle.color = 'green';
            }
        },
        playAudio: function(soundName) {
          let audio = new Audio(require(`${this.audioPath}${soundName}`));
          audio.play().catch((err) => console.log(err));
        }
    },
    computed: {
        rotateStyle: {
            get: function() {
                return this.rotateStyleInit;
            },
            set: function (newValue) {
                let newRotateClass = Object.assign({}, this.rotateStyleInit);
                newRotateClass.transform = newValue;
                this.rotateStyleInit = newRotateClass;
            }
        },
        loadedBullets: function() {
            return this.bullets ? `(${this.bullets})` : '';
        }
    },
    components: {
        carousel,
    }
};

export default App
</script>

<style>
    .annotation {
      font-size: 50px;
      justify-self: center;
      color: black;
      margin-top: 20px;
    }
    .screen {
    display: grid;
    grid: 60px 250px 50px 50px 50px / 100%;
    grid-row-gap: 10px;
    justify-content: center;
    }

    .button {
      justify-self: center;
      width: 150px;
    }

    .message {
        justify-self: center;
        font-size: 60px;
    }
</style>
