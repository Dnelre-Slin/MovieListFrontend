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
            <v-data-table
                :headers="movie_table_headers"
                :items="movie_table_data"
                :items-per-page="50"
                :expanded.sync="expanded"
                class="elevation-1"
                hide-default-header
                item-key="url"
                show-expand
                @item-expanded="TableRowClicked"
            >
                <!-- eslint-disable-next-line -->
                <template v-slot:item.image="{ item }">
                    <v-img
                        :src="item.image"
                        height="44"
                        width="32"
                    ></v-img>
                </template>
                <template v-slot:expanded-item="{ headers, item }">
                    <td :colspan="headers.length">
                        <movie-details-comp
                            :title="item.title"
                            :image="detail_result.image"
                            :lazy-image="item.image"
                            :details="detail_info"
                        ></movie-details-comp>
                    </td>
                </template>
            </v-data-table>
        </v-card-text>
    </v-card>
</template>

<script>
// image="https://m.media-amazon.com/images/M/MV5BNzQzOTk3OTAtNDQ0Zi00ZTVkLWI0MTEtMDllZjNkYzNjNTc4L2ltYWdlXkEyXkFqcGdeQXVyNjU0OTQ0OTY@._V1_.jpg"
import MovieDetailsComp from '@/components/MovieDetailsComp.vue'

export default {
    name: 'MovieSearchComp',
    components: {
        MovieDetailsComp
    },
    data: () => {
        return {
            search_value: "",
            search_result: [],
            detail_result: {},
            expanded: [],
            temp_image: new Image(),
            movie_table_headers: [
                {
                    text: 'Image',
                    value: 'image'
                },
                {
                    text: 'Title',
                    align: 'start',
                    sortable: false,
                    value: 'title'
                },
                {
                    text: 'Extra',
                    value: 'extra'
                }
            ],
        }
    },
    computed: {
        // movie_table_headers: function() {
        //     console.log(this);
        //     let headers = []
        //     if (this.search_result.length > 0) {
        //         for (const key in this.search_result[0]) {
        //             headers.push({key: this.search_result[0][key]});
        //         }
        //     }
        //     console.log(headers);
        //     return headers
        // },
        movie_table_data: function() {
            return this.search_result;
        },
        detail_info: function() {
            let info = [];
            if (this.detail_result.aggregateRating) {
                info.push({
                    name: 'Rating',
                    value: `${this.detail_result.aggregateRating.ratingValue} / 10`
                });
            }
            if (this.detail_result.genre) {
                info.push({
                    name: 'Genres',
                    value: this.detail_result.genre.join()
                });
            }
            if (this.detail_result.description) {
                info.push({
                    name: 'Description',
                    value: this.detail_result.description
                });
            }
            console.log(info);
            return info;
        }
    },
    methods: {
        ready_image(small_image) {
            console.log("WHATRATLAJTLKJTA");
            if (this.detail_result.image)
            {
                return this.detail_result.image;
            }
            return small_image;
        },
        DoSearch(e) {
            console.log("Inputed : ");
            console.log(e);
            console.log(this.search_value)
            // this.axios.get("/api/imdb-search/").then((response) => {
            //     console.log(response.data);
            // })
            console.log(process.env.VUE_APP_USERNAME);
            this.axios.get("/api/imdb-search/", {
                auth: {
                    username: process.env.VUE_APP_USERNAME,
                    password: process.env.VUE_APP_PASSWORD
                },
                params: {
                    "movie_name": this.search_value
                }
            }).then((response) => {
                console.log(response.data);
                this.search_result = response.data;
            })
        },
        TableRowClicked(item) {
            console.log(item);
            if (item.value)
            {
                console.log(item.item);
                this.detail_result = {}
                this.axios.get("/api/imdb-detail/", {
                    auth: {
                        username: process.env.VUE_APP_USERNAME,
                        password: process.env.VUE_APP_PASSWORD
                    },
                    params: {
                        "movie_url": item.item.url
                    }
                }).then((response) => {
                    console.log(response.data);
                    console.log(response.data.duration);
                    this.detail_result = response.data;
                    console.log(response.data.image);
                    this.temp_image.src = response.data.image;
                })
            }
        }
    }
}
</script>

<style>

</style>