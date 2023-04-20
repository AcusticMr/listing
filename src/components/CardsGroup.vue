<template>
    <div class="cards-group">
        <div class="card" v-for="item in items">
            <div class="card-body">
                <div class="card-info">
                    <p class="card-title">{{item.translated.name}}</p>
                    <p class="card-text">{{item.translated.description}}</p>
                </div>
                <p class="card-price">
                    <span class="card-price-label">Cena </span>{{ item.calculatedPrice.totalPrice }} zł
                </p>
            </div>
        </div>
    </div>
</template>

<script setup>
    const props = defineProps({
        items: Array
    })
</script>

<style scoped>
    .cards-group {
        display: grid;    
        grid-template-columns: 1fr 1fr;
        gap: 1%;
        margin: 0 auto;
        padding: 64px 16px;
        max-width: 1200px;
    }

    .card-body {
        display: flex;
        flex-direction: column;
        justify-content: space-between;
    }

    .card-info {
        margin-bottom: 10px;
    }

    .card-title,
    .card-price-label {
        font-weight: 700;
    }

    .card-price {
        margin: 0;
    }

    @media (min-width: 1200px) {
        .cards-group {
            padding: 64px 0;
        }
    }

    @media (min-width: 900px) {
        .cards-group {
            grid-template-columns: 1fr 1fr 1fr;
        }
    }
</style>
