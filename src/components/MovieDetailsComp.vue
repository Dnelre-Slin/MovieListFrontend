<template>
    <v-card>
        <v-card-title>
            {{title}}
        </v-card-title>
        <v-card-text>
            <v-img
                :src="image"
                :lazy-src="lazyImage"
                :width="width"
                :height="height"
            ></v-img>
            <div
                v-for="(detail, index) in details"
                :key="'detail_' + index"
            >
                {{detail.name}}: {{detail.value}}
            </div>
        </v-card-text>
    </v-card>
</template>

<script>
export default {
    name: 'MovieDetailsComp',
    props: {
        title: {
            type: String,
            required: true
        },
        image: {
            type: String,
            required: true
        },
        lazyImage: {
            type: String,
            default: ""
        },
        width: {
            type: Number,
            default: 200
        },
        height: {
            type: Number,
            default: 300
        },
        details: Array,

    }
}
</script>

<style>

</style>