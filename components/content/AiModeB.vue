<template>
    <button class="btn btn-primary inline-block mb-4 !text-white hover:!text-white" @click="click()">
        {{ isEnabledAiChanMode ? '藍モードを無効にする' : '藍モードを有効にする' }}
    </button>
</template>

<script setup lang="ts">
const isEnabledAiChanMode = ref<boolean>(false);

if (process.client) {
    isEnabledAiChanMode.value = ((localStorage.getItem('miHub_aichan_mode') ?? '') == 'true');

    // migration
    if (!isEnabledAiChanMode.value) {
        isEnabledAiChanMode.value = ((localStorage.getItem('aimode') ?? '') == 'true');
    }
}

function click() {
    localStorage.setItem('miHub_aichan_mode',isEnabledAiChanMode.value ? 'false' : 'true');
    location.reload();
}
</script>

<style scoped>

</style>