<template>
    <p class="item item-time">{{ entry[0] }} Uhr, {{ entry[1].replaceAll("/", ".") }} </p>
    <p class="item item-title">{{ entry[2] }}</p>
    <p class="item item-description">{{ entry[3] }}</p>
</template>

<!--
    
-->



<script>
    export default {
        props: ['entry'],
        // TEST:
       /*data() {
            return {
                counter: 0
            }
        } */
    }
</script>