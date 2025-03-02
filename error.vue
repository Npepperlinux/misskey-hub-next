<template>
	<div class="min-h-screen text-slate-800 dark:text-slate-200 bg-slate-100 dark:bg-gray-900">
        <NuxtIsland name="GNoScript" />
        <MkAnimBg
            class="fixed z-0 top-0 left-0 w-screen h-screen transition-opacity duration-[2s]"
            :class="isCanvasLoaded ? 'opacity-100' : 'opacity-0'"
            @load="isCanvasLoaded = true"
        ></MkAnimBg>
        <GNav @toggleNav="isNavOpen = !isNavOpen" :is-open="isNavOpen" />
        <div :class="$style.errorContainer" class="relative z-10 flex items-center justify-center">
            <div class="max-w-xl w-full p-6 mx-6 bg-white/70 dark:bg-white/20 shadow-black/10 shadow-xl rounded-xl">
                <img src="https://xn--931a.moe/assets/error.jpg" class="w-32 h-32 rounded-xl mx-auto mb-6" />
                <div v-if="error">
                    <h1 class="text-center font-bold text-2xl mb-4">{{ error?.statusCode === 404 ? $t('_error.notFound') : $t('_error.generalError') }}</h1>
                    <p class="text-center mb-4">{{ error?.statusCode === 404 ? $t('_error.notFoundDesc') : $t('_error.generalErrorDesc') }}</p>
                    <div class="max-w-lg w-full mx-auto grid grid-cols-1 sm:grid-cols-2 gap-4">
                        <button class="block rounded-full text-center px-4 py-2 border-2 hover:opacity-70" @click="handleError">{{ $t('_error.goToTop') }}</button>
                        <GNuxtLink class="block rounded-full text-center px-4 py-2 border-2 hover:opacity-70" target="_blank" :to="`${runtimeConfig.public.repositoryUrl}/issues/new`">{{ $t('_error.reportProblem') }}</GNuxtLink>
                    </div>
                </div>
                <div v-else class="mx-auto py-5">
                    <MkLoading />
                </div>
            </div>
        </div>
	</div>
</template>

<script setup lang="ts">
import NProgress from 'nprogress';

const error = useError();
const colorMode = useColorMode();
const runtimeConfig = useRuntimeConfig();

const { locale, locales } = useI18n();
const localePath = useLocalePath();
const currentLocaleIso = computed(() => (locales.value as LocaleObject[]).find((e) => e?.code === locale.value)?.iso);

const isNavOpen = ref<boolean>(false);
const isCanvasLoaded = ref<boolean>(false);

const handleError = () => clearError({ redirect: localePath('/') });

/** 
 * 中国大陸で Google Fonts を使う裏技
 * fonts.googleapis.com → fonts.googleapis.cn
 **/
 const cnHead = (locale.value === 'cn') ? [
    { rel: 'preconnect', href: 'https://fonts.googleapis.cn' },
    { rel: 'preconnect', href: 'https://fonts.gstatic.cn' },
    { rel: 'stylesheet', href: 'https://fonts.googleapis.cn/css2?family=Capriola&family=Nunito:ital,wght@0,400;0,700;1,400;1,700&display=swap' }
] : [
    { rel: 'preconnect', href: 'https://fonts.googleapis.com' },
    { rel: 'preconnect', href: 'https://fonts.gstatic.com' },
    { rel: 'stylesheet', href: 'https://fonts.googleapis.com/css2?family=Capriola&family=Nunito:ital,wght@0,400;0,700;1,400;1,700&display=swap' },
];

useHead({
    htmlAttrs: {
        lang: currentLocaleIso.value,
        'data-bs-theme': colorMode.value,
        class: 'scroll-pt-20 lg:scroll-pt-24',
    },
    link: [
        ...cnHead,
    ],
});

onMounted(() => {
    nextTick(() => {
        setTimeout(() => {
            NProgress.done();
        }, 100);
    });
})
</script>

<style module>
.errorContainer {
    min-height: calc(100vh - 4rem);
}

@screen lg {
    .errorContainer {
        min-height: calc(100vh - 5rem);
    }
}
</style>
