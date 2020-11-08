<template>
    <div
        v-bind:style="cellStyle"
    >{{cell}}</div>
</template>

<script>
    export default {
        name: "carousel_cell",
        props: {
            cell: String,
            bullet: Boolean,
            fired: Boolean,
        },
        computed: {
            cellStyle: function() {
                if(this.fired) {
                    console.log(this.bullet);
                    if(this.bullet) {
                        return {background: 'red'};
                    }
                    else {
                        return {background: 'green'};
                    }
                }
                return null;
            }
        }
    }
</script>

<style scoped>

</style>