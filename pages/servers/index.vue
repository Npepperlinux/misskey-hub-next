<template>
    <div>
        <GHero>
            <template #title>{{ $t('_servers.title') }}</template>
            <template #description>
                {{ $t('_servers.description') }}<br>
                <I18nT keypath="_servers.addYourServer" tag="span">
                    <GNuxtLink class="font-bold hover:underline underline-offset-4" to="https://github.com/joinmisskey/api">{{ $t('_servers.addYourServerLink') }}</GNuxtLink>
                </I18nT>
            </template>
            <template #icon>
                <div class="relative px-6 py-8">
                    <GDots class="absolute top-0 left-0 w-32 h-32 text-accent-600" />
                    <GDots class="absolute bottom-0 right-0 w-32 h-32 text-accent-600" />
                    <div class="relative bg-white dark:bg-slate-800 shadow-lg rounded-lg w-full lg:w-80 p-6 space-y-4 break-words">
                        <dl>
                            <dt>{{ $t('_servers._statistics.notes') }}</dt>
                            <dd class="font-bold text-accent-600 text-2xl">{{ instancesStats?.notesCount ? $n(instancesStats.notesCount) : $t('loading') }}</dd>
                        </dl>
                        <dl>
                            <dt>{{ $t('_servers._statistics.users') }}</dt>
                            <dd class="font-bold text-accent-600 text-2xl">{{ instancesStats?.usersCount ? $n(instancesStats.usersCount) : $t('loading') }}</dd>
                        </dl>
                        <dl>
                            <dt>{{ $t('_servers._statistics.servers') }}</dt>
                            <dd class="font-bold text-accent-600 text-2xl">{{ instancesStats?.instancesCount ? $n(instancesStats.instancesCount) : $t('loading') }}</dd>
                        </dl>
                        <div class="!mt-2">
                            <GNuxtLink class="hover:underline underline-offset-2" :to="localePath('/servers/stats/')">{{ $t('_servers._statistics.viewFullStats') }}<ArrowRightIco class="ml-1" /></GNuxtLink>
                        </div>
                    </div>
                </div>
            </template>
        </GHero>
        <div class="pb-12 lg:mt-12 pt-6 bg-white dark:bg-slate-950 min-h-screen">
            <ClientOnly>
                <ServersFinder @load="setServerStats" />
                <template #fallback>
                    <div class="container mx-auto max-w-screen-xl p-6">
                        <MkLoading class="mx-auto text-accent-600"></MkLoading>
                        <p class="text-center">{{ $t('loading') }}</p>
                    </div>
                </template>
            </ClientOnly>
        </div>
    </div>
</template>

<script setup lang="ts">
import type { InstancesStatsObj } from '@/types/instances-info';
import ArrowRightIco from "bi/arrow-right.svg";

const { t, locale } = useI18n();
const localePath = useLocalePath();
const route = useRoute();

const instancesStats = ref<InstancesStatsObj>();

function setServerStats(val: InstancesStatsObj) {
    instancesStats.value = val;
}

route.meta.title = t('_servers.title');
route.meta.description = t('_servers.description');
</script>

<style scoped>
</style>