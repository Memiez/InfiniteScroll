<template>
    <div>
        <header class="header">
            <h1>Coding Resources</h1>
        </header>
        <div class="container">
            <div class="card" v-for="(resource, index) in resources" :key="index">
                <h2>{{ resource.title }}</h2>
                <p>{{ resource.description }}</p>
                <div class="tags">
                    <span class="tag" v-for="tag in resource.tags" :key="tag">{{ tag }}</span>
                </div>
                <button class="button">See More</button>
            </div>
        </div>
    </div>
</template>

<script>
import { ref, onMounted } from 'vue';
import axios from 'axios';

export default {
    setup() {
        const resources = ref([]);

        onMounted(async () => {
            try {
                const response = await axios.get('http://api.sampleapis.com/codingresources/codingResources');
                resources.value = response.data;
            } catch (error) {
                console.error(error);
            }
        });

        return { resources };
    }
};
</script>

<style scoped>
.header {
    background-color: #000000;
    color: #fff;
    padding: 1rem;
    text-align: center;
}

.container {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 1rem;
    padding: 1rem;
    background-color: #f0f0f0;
}

.card {
    background-color: #ffffff;
    border-radius: 10px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    padding: 1rem;
    display: flex;
    flex-direction: column;
}

.card h2 {
    font-size: 1.5rem;
    color: #333333;
}

.card p {
    font-size: 1rem;
    color: #666666;
}

.tags {
    margin-top: 0.5rem;
}

.tag {
    display: inline-block;
    background-color: rgb(0, 13, 27);
    color: white;
    padding: 0.25rem 0.5rem;
    margin-right: 0.25rem;
    border-radius: 5px;
    font-size: 0.85rem;
}

.button {
    margin-top: auto;
    padding: 0.5rem 1rem;
    background-color: #380303;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
}

.button:hover {
    background-color: #000000;
}

@media (max-width: 768px) {
    .container {
        grid-template-columns: 1fr;
    }
}</style>
