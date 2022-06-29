<template>
    <div>
        <bannerComponent />

        <section class="posts pt-5">
            <div class="container">
                <div class="row row-cols-1 row-cols-sm-2 row-cols-md-3 g-4">
                    <div
                        class="col"
                        v-for="post in postResponse.data"
                        :key="post.id"
                    >
                        <div class="product card">
                            <img
                                :src="'storage/' + post.cover_image"
                                :alt="post.title"
                            />
                            <div class="card-body">
                                <h3>{{ post.title }}</h3>
                                <p>{{ post.content }}</p>
                            </div>

                            <div class="card-footer">
                                <div class="row">
                                    <div class="col">
                                        <div class="author" v-if="post.user">
                                            <strong>Author: </strong>
                                            {{ post.user.name }}
                                        </div>
                                    </div>

                                    <div class="col">
                                        <span v-if="post.category">
                                            <strong>Category: </strong>
                                            {{ post.category.name }}
                                        </span>
                                        <div
                                            class="tags"
                                            v-if="post.tags.length > 0"
                                        >
                                            <strong>Tags: </strong>
                                            <ul class="list-unstyled">
                                                <li
                                                    v-for="tag in post.tags"
                                                    :key="tag.id"
                                                >
                                                    {{ tag.name }}
                                                </li>
                                            </ul>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>

                <nav aria-label="Page navigation" class="py-5">
                    <ul class="pagination justify-content-center">
                        <li
                            class="page-item"
                            v-if="postResponse.current_page > 1"
                        >
                            <a
                                class="page-link"
                                href="#"
                                aria-label="Previous"
                                @click.prevent="
                                    getAllPosts(postResponse.current_page + 1)
                                "
                            >
                                <span aria-hidden="true">&laquo;</span>
                                <span class="sr-only">Previous</span>
                            </a>
                        </li>

                        <li
                            :class="{
                                'page-item': true,
                                active: page == postResponse.current_page,
                            }"
                            v-for="page in postResponse.last_page"
                            :key="page"
                        >
                            <a
                                class="page-link"
                                href="#"
                                @click.prevent="getAllPosts(page)"
                                >{{ page }}</a
                            >
                        </li>

                        <li
                            class="page-item"
                            v-if="
                                postResponse.current_page <
                                postResponse.last_page
                            "
                        >
                            <a
                                class="page-link"
                                href="#"
                                aria-label="Next"
                                @click.prevent="
                                    getAllPosts(postResponse.current_page + 1)
                                "
                            >
                                <span aria-hidden="true">&raquo;</span>
                                <span class="sr-only">Next</span>
                            </a>
                        </li>
                    </ul>
                </nav>
            </div>
        </section>
    </div>
</template>

<script>
import bannerComponent from "../components/bannerComponent.vue";
export default {
    name: "App",
    components: { bannerComponent },

    data() {
        return {
            posts: "",
            postResponse: "",
        };
    },
    methods: {
        getAllPosts(postPage) {
            axios
                .get("/api/posts", {
                    params: {
                        page: postPage,
                    },
                })

                .then((response) => {
                    console.log(response);
                    this.postResponse = response.data;
                })
                .catch((e) => {
                    console.error(e);
                });
        },
    },
    mounted() {
        this.getAllPosts(1);
    },
};
</script>
