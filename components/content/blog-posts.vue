<template>
    <section class="not-prose font-mono">
        <div class="column text-gray-400 text-sm">
            <div>Date</div>
            <div>Title</div>
        </div>
        <ul>
            <li
                v-for="post in posts"
                :key="post._path"
            >
                <NuxtLink
                    :to="post._path"
                    class="column hover:bg-gray-200 dark:hover:bg-gray-800"
                >
                    <div
                        class="text-gray-500"
                        :class="{ 'invisible': !post.displayYear }"
                    >
                        {{ post.year }}
                    </div>
                    <div>{{ post.title }}</div>
                </NuxtLink>
            </li>
        </ul>
    </section>
</template>

<script setup>
const { data } = await useAsyncData(
    'blog-list',
    () => queryContent("/blog")
        .where({ _path: { $ne: '/blog' } })
        .only(['_path', 'title', 'publishedAt'])
        .sort({ publishedAt: -1 })
        .find()
);

const posts = computed(() => {
    if (!data.value) {
        return []
    }

    const result = [];
    let lastYear = null;

    for (const post of data.value) {
        const year = useDateFormat(post.publishedAt, "YYYY").value;
        const displayYear = year !== lastYear;
        post.displayYear = displayYear;
        post.year = year;
        lastYear = year;
        result.push(post);
    }
    return result
});
</script>

<style scoped>
.column {
    @apply flex items-center space-x-8 py-2 border-b border-gray-200 dark:border-gray-700 px-2
}
</style>