<template>
    <div class="scene">
        <div class="carousel"
             v-bind:style="[rotateStyle.transform ? rotateStyle : '']">
            <carousel_cell class="carousel_cell"
                 v-for="(cell, index) in cells"
                 v-bind:key="index"
                 v-bind:cell = 'cell'
                 v-bind:fired="fired"
                 v-bind:bullet = 'cylinder[index]'
            ></carousel_cell>
        </div>
    </div>
</template>

<script>
    import carousel_cell from "@/components/carousel_cell";

    export default {
        name: "carousel",
        props: {
            cells: Array,
            cylinder: Array,
            rotateStyle: Object,
            fired: Boolean,
        },
        components: {
            carousel_cell,
        }
    }
</script>

<style scoped>
    .scene {
        border: 1px solid #CCC;
        position: relative;
        width: 230px;
        height: 170px;
        margin: 50px auto;
        perspective: 1000px;
    }

    .carousel {
        width: 100%;
        height: 100%;
        position: absolute;
        transform: translateZ(-199px);
        transform-style: preserve-3d;
        transition: transform 1s;
    }

    .carousel_cell {
        position: absolute;
        width: 205px;
        height: 140px;
        margin: 10px;
        border: 2px solid black;
        line-height: 116px;
        font-size: 80px;
        font-weight: bold;
        color: white;
        text-align: center;
    }

    .carousel_cell:nth-child(6n+1) { background: hsla(62, 100%, 52%, 0.8); }
    .carousel_cell:nth-child(6n+2) { background: hsla(62, 100%, 52%, 0.8); }
    .carousel_cell:nth-child(6n+3) { background: hsla(62, 100%, 52%, 0.8); }
    .carousel_cell:nth-child(6n+4) { background: hsla(62, 100%, 52%, 0.8); }
    .carousel_cell:nth-child(6n+5) { background: hsla(62, 100%, 52%, 0.8); }
    .carousel_cell:nth-child(6n+0) { background: hsla(62, 100%, 52%, 0.8); }

    .carousel_cell:nth-child(1) { transform: rotateY(  0deg) translateZ(199px); }
    .carousel_cell:nth-child(2) { transform: rotateY( 60deg) translateZ(199px); }
    .carousel_cell:nth-child(3) { transform: rotateY( 120deg) translateZ(199px); }
    .carousel_cell:nth-child(4) { transform: rotateY( 180deg) translateZ(199px); }
    .carousel_cell:nth-child(5) { transform: rotateY( 240deg) translateZ(199px); }
    .carousel_cell:nth-child(6) { transform: rotateY( 300deg) translateZ(199px); }
</style>