<template>
    <movie-details-comp
        title="The Matrix"
        image="https://m.media-amazon.com/images/M/MV5BNzQzOTk3OTAtNDQ0Zi00ZTVkLWI0MTEtMDllZjNkYzNjNTc4L2ltYWdlXkEyXkFqcGdeQXVyNjU0OTQ0OTY@._V1_.jpg"
        :details="details"
    ></movie-details-comp>
</template>

<script>
import MovieDetailsComp from '@/components/MovieDetailsComp.vue'

export default {
    name: 'Home',
    components: {
        MovieDetailsComp
    },
    data: () => {
        return {
            details: [
                {
                    name: 'Rating',
                    value: '9/10'
                },
                {
                    name: 'Genres',
                    value: 'Action, Sci-Fi'
                },
                {
                    name: 'Description',
                    value: 'Real cool movie!'
                }
            ],
        }
    },
}
</script>

<style>

</style>