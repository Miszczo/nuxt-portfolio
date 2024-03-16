<template>
    <p class="mb-10">
        Take lool at my Github projects!
    </p>

    <section v-if="pending">Pending...</section>
    <section v-else-if="error">Something wen wrong... try again</section>
    <section v-else="error">
        <ul class="grid grid-cols-1 gap-4">
            <li
                v-for="repository in repos"
                :key="repository.id"
                class="border border-gray-200 rounded-sm p-4 hover:bg-gray-100 font-monofont"
            >
                <a
                    :href="repository.html_url"
                    target="_blank"
                >
                    <div class="flex items-center justify-between">
                        <div class="font-semibold">{{ repository.name }}</div>
                        <div>{{ repository.stargazers_count }} ⭐</div>
                    </div>
                    <p class="text-sm">
                        {{ repository.description }}
                    </p>
                </a>
            </li>
        </ul>
    </section>
</template>


<script setup>
const { error, pending, data } = await useFetch('https://api.github.com/users/miszczo/repos');
console.log(data)

const repos = computed(() => {
    return data.value.filter(repo => repo.description)
        .sort((a, b) => {
            const dateA = new Date(a.created_at);
            const dateB = new Date(b.created_at);
            return dateB - dateA;
        });
})
</script>