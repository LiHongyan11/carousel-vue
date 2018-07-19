<template>
    <carousel-3d :inverse-scaling="100" :space="400">
        <slide v-for="(slide, i) in slides" :key="i" :index="i">
        </slide>
    </carousel-3d>
</template>

<script>
import {Carousel3d, Slide} from 'vue-carousel-3d';

export default {
    name: 'Test',
    components: {
       Carousel3d,
       Slide
    },
    props: {
        list: {
            type: Array,
            default() {
                return [];
            }
        },
    },
    data() {
        return {
            slides: 15
        }
    }
}
</script>
