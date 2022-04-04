<template>
    <form @submit.prevent="searchNews" class="d-flex flex-column justify-content-center">
        <div class="input-group mx-sm-3 mb-2">
            <label class="visually-hidden" for="search">Search</label>
            <input type="search" name="search" v-model="searchTerm"
            id="search" class="form-control mb-2 mr-sm-2" placeholder="Enter search term here" />
            <button class="btn btn-primary mb-2">Search</button>
        </div>
        <p>You are searching for {{ searchTerm }}</p>
    </form>
    <ul class="news__list">
        <li v-for="article in articles" class="news__item">
            <div class="card">
                <img class="card-img" :src="article.urlToImage"><br>
                <div class="info">
                    <div class="card-title">
                        {{ article.title }}<br>
                    </div>
                    {{article.description}} 
                </div>
            </div>
        </li>
    </ul>
    
</template>

<script>
    export default {
        data() {
            return {
                articles: [],
                searchTerm: ''
            };
        },
        methods: {
            searchNews() {
                let self = this;
                fetch('https://newsapi.org/v2/everything?q='+
                self.searchTerm + '&language=en', {
                    headers: {
                    'Authorization': `Bearer ${import.meta.env.VITE_NEWSAPI_TOKEN}`,
                    }
                })
                .then(function(response) {
                    return response.json();
                })
                .then(function(data) {
                    console.log(data);
                    self.articles = data.articles;
                });
            }
        },
        created() {
            let self = this;
            fetch('https://newsapi.org/v2/top-headlines?country=us',
            {
                headers: {
                'Authorization': `Bearer ${import.meta.env.VITE_NEWSAPI_TOKEN}`
                }
            })
            .then(function(response) {
                return response.json();
            })
            .then(function(data) {
                console.log(data);
                self.articles=data.articles;
            });
        }
    };
</script>

<style>
    .card{
        width: 300px;
        height:450px;
        
        border: 1px solid rgb(181, 176, 176);
    }
    .info{
        padding:5px;
    }
    .card-img{
        width: 100%;
        height: 180px;
        border-radius:0;
    }
    .card-title{
        font-weight: bold;
    }
    ul{
        list-style: none;
        display: grid;
        gap: 2rem;
        grid-template-columns: 300px 300px 300px;
    }
</style>