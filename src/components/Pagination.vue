<template>
    <div class="container">
        <nav class="navbar navbar-light bg-light mb-3">
            <span>Всего постов :{{posts.length}}</span>
            <div class="col-auto my-1">
                <select class="custom-select mr-sm-2" id="inlineFormCustomSelect" v-model="perPage"
                        @change="changePage">
                    <option value="10" selected>10</option>
                    <option value="25">25</option>
                    <option value="50">50</option>
                </select>
            </div>
        </nav>
        <div class="row">
            <div class="clearfix btn-group col-md-12 mb-5">
                <button type="button" class="btn btn-sm btn-outline-secondary" @click="page !== 1 ? page--: page = 1">
                    Предыдущая
                </button>
                <button type="button" class="btn btn-sm btn-outline-secondary"> {{page}}</button>
                <button type="button" @click="page < pages ? page++ : page = pages"
                        class="btn btn-sm btn-outline-secondary"> Следующая
                </button>
            </div>

            <div class="col-md-4" v-for="post in displayedPosts" v-bind:key="post.id">
                <div class="card mb-4 box-shadow post-cards">
                    <div class="card-body">
                        <h5 class="capitalize">{{post.title}}</h5>
                        <p class="card-text">{{post.body.slice(0, 120)}}...</p>
                        <h4> User Name : {{post.userId}}</h4>
                    </div>
                </div>
            </div>

            <div class="clearfix btn-group col-md-12 mb-5">
                <button type="button" class="btn btn-sm btn-outline-secondary" @click="page !== 1 ? page--: page = 1">
                    Предыдущая
                </button>
                <button type="button" class="btn btn-sm btn-outline-secondary"> {{page}}</button>
                <button type="button" @click="page < pages ? page++ : page = pages"
                        class="btn btn-sm btn-outline-secondary"> Следующая
                </button>
            </div>
        </div>

    </div>
</template>

<script>
    export default {
        name: "Pagination",
        data() {
            return {
                posts: [],
                baseUrl: 'https://jsonplaceholder.typicode.com/',
                page: 1,
                perPage: 10,
                pages: 10,
            }
        },
        methods: {
            async getPosts() {
                //post fetch
                fetch(this.baseUrl + 'posts')
                    .then(data => data.json())
                    .then(res => this.posts = res)
                    .catch(e => console.log(e))
                //user fetch

            },
            changePage() {
                this.page = 1
                this.setPages()

            },
            setPages() {
                this.pages = Math.ceil(this.posts.length / this.perPage);
            },
            paginate(posts) {
                let page = this.page;
                let perPage = this.perPage;
                let from = (page * perPage) - perPage;
                let to = (page * perPage);
                // console.log(posts.slice(from, to))
                return posts.slice(from, to);
            }
        },
        created() {
            this.getPosts();

        },
        watch: {
            pages() {
                this.setPages();
            }
        },
        computed: {
            displayedPosts() {
                return this.paginate(this.posts);
            }
        }

    }
</script>

<style scoped>

</style>