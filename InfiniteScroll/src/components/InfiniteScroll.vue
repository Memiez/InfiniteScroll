<template>
    <div class="infinite-scroll-container">
        <div class="cards-container">
            <div class="card" v-for="(post, index) in posts" :key="index">
                <h3>{{ post.title }}</h3>
                <p>{{ post.body }}</p>
            </div>
        </div>
    </div>
</template>

<script lang="ts">
import { onMounted, onUnmounted, ref } from 'vue';
import axios from 'axios';

export default {
    name: 'InfiniteScroll',
    setup() {
        const posts = ref([]);
        const loading = ref(false);
        const page = ref(1);
        const perPage = 20;

        const loadPosts = async () => {
            if (loading.value) return;
            loading.value = true;

            try {
                const response = await axios.get('http://api.sampleapis.com/codingresources/codingResources', {
                    params: {
                        _page: page.value,
                        _limit: perPage
                    }
                });
                posts.value.push(...response.data);
                page.value++;
            } catch (error) {
                console.error('Error loading posts:', error);
            } finally {
                loading.value = false;
            }
        };

        onMounted(loadPosts);

        const onScroll = () => {
            if (window.innerHeight + window.scrollY >= document.body.offsetHeight - 200 && !loading.value) {
                loadPosts();
            }
        };

        window.addEventListener('scroll', onScroll);

        onUnmounted(() => {
            window.removeEventListener('scroll', onScroll);
        });

        return {
            posts,
            loading
        };
    }
};
</script>

<style scoped>
.infinite-scroll-container {
    padding: 2rem;
}

.cards-container {
    display: flex;
    flex-wrap: wrap;
    margin: -5px;
}

.card {
    flex: 1 0 calc(25% - 10px);
    margin: 5px;
    box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2);
    padding: 15px;
}
</style>