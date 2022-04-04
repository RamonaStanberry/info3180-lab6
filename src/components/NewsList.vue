<template>
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
                articles: []
            };
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