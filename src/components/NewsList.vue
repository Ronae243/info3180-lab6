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
    <div class="grid">
      <ul v-for="article in articles" class="news__list">
        <div class="cards">
             <img :src = article.urlToImage alt="img">
             <li class="news__item"> {{ article.title }}</li>
             <li class="news__item2"> {{ article.description }}</li>
        </div>
      </ul>
    </div>
</template>

<script>
export default {
    data() {
        return {
            articles:[],
            searchTerm: ''
        };
        
    },
    
    
    created() {
        let self = this;
        fetch('https://newsapi.org/v2/top-headlines?country=us',
{
    headers: {
        Authorization: `Bearer ${import.meta.env.VITE_NEWSAPI_TOKEN}`
    }
})
        .then(function(response) {
            return response.json();
        })
        .then(function(data) {
            console.log(data);
            self.articles = data.articles;
        });
    },

    methods: {
        searchNews() {
            let self = this;
            fetch('https://newsapi.org/v2/everything?q='+
            self.searchTerm + '&language=en', {
            headers: {'Authorization': `Bearer ${import.meta.env.VITE_NEWSAPI_TOKEN}`,
            }})
            .then(function(response) {
                return response.json();
            })
            .then(function(data) {
            console.log(data);
            self.articles = data.articles;
            });
        }
    }
};
</script>

<style>
.grid{
    display: grid;
    justify-content: center;
    margin-top: 60px;
    grid-template-columns: repeat(auto-fit, minmax(auto,400px));
    column-gap: 25px;
    row-gap: 20px;
}

.cards{
    display: flex;   
    border-radius: 10px;
    padding-top: -5px;
    height: 100%;
    background-color: FFFFFF;
    box-shadow: 3px 6px 14px 1px rgba(0,0,0,0.24);
    flex-flow: column nowrap;
    flex-flow:row wrap;
   
}

.cards img{
    border-top-left-radius: 10px;
    border-top-right-radius: 10px;
    height: 290px;
    width: 100%;
    transition: transform .5s ease-in-out;
    flex-shrink: 1 1 0;
    object-fit:cover;
}

.news__item{
    font-size: 20px;
    padding-top:10px;
    list-style-type: none;
    font-weight: 800;
    text-align: left;
    padding-left: 10px;
    padding-right: 10px;
    
}

.news__item2{
    padding-top:10px;
    list-style-type: none;
    text-align: left;
    padding-left: 10px;
    padding-right: 10px;
}

p{
    text-align: center;
}
</style>
