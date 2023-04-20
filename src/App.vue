<template>
    <header>
        <div class="header">
            <div class="heading">Shopware Listing</div>
            <div>    
                <select class="sorting form-select form-select-md" @change="fetchData()" v-model="state.sorting">
                    <option value="price-asc">Najtańsze</option>
                    <option value="price-desc">Najdroższe</option>
                </select>
            </div>
        </div>
    </header>

    <main>
        <div class="content">
            <div class="search__container">
                <input class="search form-control" v-model="state.searchPhrase" @input="fetchData()" type="search" placeholder="Szukaj...">
            </div>

            <div v-if="state.errors.length" class="alert alert-danger" role="alert">
                {{ state.errors[0].detail }}
            </div>

            <div v-else class="cards-container">
                <CardsGroup :items="state.items"/>
            </div>
        </div>
    </main>
</template>

<script setup>
    import { ref, reactive } from 'vue'
    import { APISettings } from './api/config';
    import CardsGroup from './components/CardsGroup.vue'

    const state = reactive({
        items: [],
        searchPhrase: '',
        sorting: 'price-desc',
        errors: []
    });

    const fetchData = async () => {
        const options = {
            method: "POST",
            headers: APISettings.headers
        }
        let res

        if (state.searchPhrase.length) {
            res = await fetch(`${APISettings.baseURL}/search?order=${state.sorting}&search=${state.searchPhrase}`, options);
        } else {
            res = await fetch(`${APISettings.baseURL}/product-listing/e435c9763b0d44fcab67ea1c0fdb3fa0?order=${state.sorting}`, options);
        }

        const json = await res.json();
        state.items = json.elements;

        if (!res.ok) {
            state.errors=json.errors
        } else {
            state.errors = []
        }
    }

    // initial fetch
    fetchData()
</script>

<style scoped>
    header {
        background-color: var(--bgc-header);
    }
  
    .header {
        display: flex;
        justify-content: space-between;
        padding: 16px;
        margin: 0 auto;
        color: var(--c-header-text);
    }

    .heading {
        font-weight: 700;
        line-height: 38px;
        font-size: 22px;
    }

    .sorting {
        width: 150px;
        background-color: var(--bgc-header);
        color: var(--c-sorting);
        border-color: var(--c-sorting);
    }

    .search__container {
        display: flex;
        justify-content: center;
        max-width: 1200px;
        margin: 100px auto;
    }

    .search {
        width: 40%;
    }

    .cards-container {
        background-color: var(--bgc-cards);
    }

    .alert {
        max-width: 1200px;
        margin: 8px auto;
    }

    @media (min-width: 1200px) {
        .header {
            max-width: 1200px;
            padding: 16px 0;
        }

        .sorting {
            width: 300px;
        }
    }
</style>
