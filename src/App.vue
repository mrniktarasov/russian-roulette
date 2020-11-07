<template>
  <div id="app" class="screen">
    <carousel :cells="cells"
              :rotateStyle="rotateStyle"
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
      <div v-if="message">{{message}}</div>
  </div>
</template>

<script>
import carousel from "@/components/carousel";

export default {
    name: 'App',
    data: function() {
        return {
            cells: new Array(6).fill('?'),
            selectedIndex: 0,
            position: 0,
            bullets: 0,
            cylinder: new Array(6).fill(0),
            message: '',
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
            this.clearMessage();
            this.selectedIndex++;
            if(this.position < 6) {
                this.position++;
            } else {
                this.position = 0;
            }
            //let randRounds =  this.getRandomInt(1, Math.round(this.cells.length/2));
            let angle = this.selectedIndex / this.cells.length * -360 ;
            this.rotateStyle.transform = `translateZ(-199px) rotateY(${angle}deg)`;
        },
        load: function() {
            this.clearMessage();
            if(this.bullets < 5) {
                if(!this.cylinder[this.position]) {
                    this.cylinder[this.position] = 1;
                    this.bullets++;
                } else {
                    this.message = 'This chamber is already full!';
                }
            }
        },
        clearMessage: function() {
            this.message = '';
        },
        fire: function() {
            this.bullets = 0;
            this.position = 0;
            this.cylinder.fill(0);
            if (this.cylinder[this.position]) {
                this.message = 'You are dead!';
            } else {
                this.message = 'You are lucky bastard!';
            }
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
}
</script>

<style>
  .screen {
    display: grid;
    grid: 250px 50px 50px 50px / 100%;
    grid-row-gap: 10px;
    justify-content: center;
  }

  .button {
      justify-self: center;
      width: 150px;
  }
</style>
