<template>
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-md-8">
                <div class="card">
                    <div class="card-header">Example Component</div>
                    <input v-model="searchTerm">

                    <div v-if="repos" class="card-body">
                        <div v-for="(repo, key) in filteredRepos" :key="key">
                            <h1>{{ repo.name }}</h1>
                            <p>{{ repo.description }}</p>

                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        mounted() {
            console.log('Component mounted.')
            this.fetchRepos()
        },
        data() {
            return {
                searchTerm: '',
                repos: null,
            }
        },
        computed: {
            filteredRepos() {
                return this.repos.items.filter(repo => {
                    if(this.searchTerm == '') {
                        return true
                    }
                    return repo.name == this.searchTerm
                })
            }
        },
        methods: {
            fetchRepos() {
                fetch(`https://api.github.com/search/repositories?q=created:%3E2019-01-22&sort=stars&order=asc`)
                .then(response => response.json())
                .then(response => {
                    this.repos = response
                })
                .catch(error => {

                })
            },
        },
    }
</script>
