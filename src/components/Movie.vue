<template>
    <div class="flex flex-col overflow-hidden bg-white rounded-xl text-black transition hover:scale-110">
        <div>
            <img class="w-full rounded-t-xl" :src="imageURL" alt="">
        </div>
        <div class="p-2 flex flex-col">
            <h1 class="text-xl font-bold">{{ movie.title }}</h1>
            <div>
                <span>
                    {{ overview }}
                </span>
                <a @click="isOverviewExpanded = true" v-if="overview.length != movie.overview.length"
                    class="font-bold text-gray-600 cursor-pointer text-sm"> Read More....</a>
            </div>
        </div>
    </div>
</template>

<script setup>
import { computed, ref } from 'vue';

const props = defineProps({
    movie: Object
})
const isOverviewExpanded = ref(false)
const imageURL = computed(() => {
    return `https://image.tmdb.org/t/p/w500/${props.movie.backdrop_path}`
})
const overview = computed((() => {
    if (isOverviewExpanded.value) {
        return props.movie.overview;
    }
    return props.movie.overview.substr(0, 80);
}))
</script>