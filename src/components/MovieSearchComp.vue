<template>
    <v-card>
        <v-card-title>Search</v-card-title>
        <v-card-text>
            <v-text-field
                ref="search_field"
                label="Movie name"
                v-model="search_value"
                @keydown.enter="DoSearch()"
            ></v-text-field>
        </v-card-text>
    </v-card>
</template>

<script>
export default {
    name: 'MovieSearchComp',
    data: () => {
        return {
            search_value: ""
        }
    },
    methods: {
        DoSearch(e) {
            console.log("Inputed : ");
            console.log(e);
            console.log(this.$refs.search_field.value)
        },
    }
}
</script>

<style>

</style>