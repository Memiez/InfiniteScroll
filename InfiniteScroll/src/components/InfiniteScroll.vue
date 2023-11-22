<template>
    <div class="resource-container">
        <div class="resource-card" v-for="(resource, index) in resources" :key="index">
            <h3>{{ resource.title }}</h3>
            <div class="tags">
                <span class="tag" v-for="tag in resource.tags" :key="tag">{{ tag }}</span>
            </div>
            <button class="see-more">See More</button>
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
.resource-container {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 1rem;
}

.resource-card {
    border: 1px solid #eaeaea;
    border-radius: 8px;
    padding: 1rem;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.resource-card:hover {
    box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
}

.tags {
    margin-bottom: 1rem;
}

.tag {
    display: inline-block;
    background: #eaeaea;
    padding: 0.25rem 0.5rem;
    margin-right: 0.25rem;
    border-radius: 8px;
}

.see-more {
    background-color: #007bff;
    color: white;
    padding: 0.5rem 1rem;
    border: none;
    border-radius: 8px;
    cursor: pointer;
}

/* Responsive layout for smaller screens */
@media (max-width: 1200px) {
    .resource-container {
        grid-template-columns: repeat(2, 1fr);
    }
}

@media (max-width: 768px) {
    .resource-container {
        grid-template-columns: 1fr;
    }
}
</style>
  