<template>
  <div class="tag">
    <div v-if="error">{{ error }}</div>
    <div v-if="posts.length" class="layout">
      <PostList :posts="filteredPosts" />
      <TagCloud :posts="posts" />
    </div>
    <div v-else>
      <Loader />
    </div>
  </div>
</template>

<script>
import Loader from '../components/Loader.vue'
import PostList from '../components/PostList.vue'
import TagCloud from '../components/TagCloud.vue'
import getPosts from '../composables/getPosts'
import { useRoute } from 'vue-router'
import { computed } from 'vue'

export default {
  components: { Loader, PostList, TagCloud },
  setup() {
    const route = useRoute()
    const { posts, error, load } = getPosts()

    load()

    const filteredPosts = computed(() => {
      return posts.value.filter((p) => p.tags.includes(route.params.tag))
    })

    return { posts, error, filteredPosts }
  }
}
</script>

<style>
.tag {
  max-width: 1200px;
  margin: 0 auto;
  padding: 10px;
}
</style>