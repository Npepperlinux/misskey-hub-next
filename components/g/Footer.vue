<template>
    <footer class="p-6 sm:pt-12">
        <div class="mx-auto container max-w-screen-xl">
            <div class="md:flex md:justify-between">
                <div class="mb-6 md:mb-0">
                    <GNuxtLink :to="localePath('/')" class="flex items-center">
                        <MiIcon class="h-8 w-8 mr-3" />
                        <span class="self-center text-2xl font-bold font-title whitespace-nowrap">{{ $t('_seo.siteName') }}</span>
                    </GNuxtLink>
                </div>
                <div class="grid grid-cols-2 gap-8 sm:gap-6">
                    <div>
                        <h2 class="mb-6 text-sm font-bold">{{ $t('_docs.title') }}</h2>
                        <ul class="text-slate-600 dark:text-slate-400 space-y-4">
                            <li>
                                <GNuxtLink :to="localePath('/docs/')" class="hover:underline">{{ $t('_docs.indexTitle') }}</GNuxtLink>
                            </li>
                            <li>
                                <GNuxtLink :to="localePath('/docs/for-users')" class="hover:underline">{{ $t('_docs._forUsers.title') }}</GNuxtLink>
                            </li>
                            <li>
                                <GNuxtLink :to="localePath('/docs/for-admin')" class="hover:underline">{{ $t('_docs._forAdmin.title') }}</GNuxtLink>
                            </li>
                            <li>
                                <GNuxtLink :to="localePath('/docs/for-developers')" class="hover:underline">{{ $t('_docs._forDevelopers.title') }}</GNuxtLink>
                            </li>
                            <li>
                                <GNuxtLink :to="localePath('/docs/releases')" class="hover:underline">{{ $t('_docs._changelog.title') }}</GNuxtLink>
                            </li>
                        </ul>
                    </div>
                    <div>
                        <h2 class="mb-6 text-sm font-bold">{{ $t('_other.title') }}</h2>
                        <ul class="text-slate-600 dark:text-slate-400 space-y-4">
                            <li>
                                <GNuxtLink :to="localePath('/servers/')" class="hover:underline">{{ $t('_servers.title') }}</GNuxtLink>
                            </li>
                            <li>
                                <GNuxtLink :to="localePath('/brand-assets/')" class="hover:underline">{{ $t('_brandAssets.title') }}</GNuxtLink>
                            </li>
                            <li>
                                <GNuxtLink :to="localePath('/tools/')" class="hover:underline">{{ $t('_tools.title') }}</GNuxtLink>
                            </li>
                            <li>
                                <GNuxtLink :to="localePath('/learn-more/')" class="hover:underline">{{ $t('_links.title') }}</GNuxtLink>
                            </li>
                        </ul>
                    </div>
                </div>
            </div>
            <hr class="my-6 border-slate-200 sm:mx-auto dark:border-slate-700" />
            <div class="sm:flex sm:items-center sm:justify-between">
                <span class="text-sm text-slate-500 sm:text-center dark:text-slate-400">&copy; 2023 Misskey, syuilo, and other contributors
                </span>
            </div>
        </div>
    </footer>
</template>

<script setup lang="ts">
import MiIcon from '@/assets/svg/misskey_mi_bi.svg';

const localePath = useLocalePath();
</script>

<style scoped>

</style>