<template>
    <div
        :class="[
            'sticky top-0 z-[9900] transition',
            {
                'shadow bg-opacity-90': (!disableShadow && scrollPos <= -40),
                'bg-white dark:bg-gray-950': (disableShadow || scrollPos <= -40),
                'border-b border-slate-300 dark:border-slate-800': hasBorder,
            },
            (slim ? 'h-16' : 'h-16 lg:h-20'),
        ]">
        <nav class="container mx-auto max-w-screen-xl grid items-center grid-cols-2 lg:grid-cols-6 p-4 h-full transition-[height]">
            <div class="">
                <GNuxtLink :to="localePath('/')" class="flex items-center space-x-2 hover:opacity-80">
                    <MiIcon class="h-8 w-8" />
                    <div class="font-title font-bold text-lg">{{ $t('_seo.siteName') }}</div>
                </GNuxtLink>
            </div>
            <ul
                class="fixed z-[9902] top-16 right-0 text-right p-4 w-[80vw] sm:w-[50vw] bg-slate-100/90 dark:bg-slate-950/90 shadow-lg space-y-2 transition-[transform,border-radius] lg:transition-none lg:translate-x-0 lg:backdrop-blur-none lg:w-auto lg:rounded-none lg:shadow-none lg:space-y-0 lg:p-0 lg:relative lg:top-0 lg:right-auto lg:bg-transparent dark:lg:bg-transparent lg:col-span-4 lg:space-x-8 xl:space-x-10 lg:flex lg:justify-center"
                :class="[(scrollPos <= -40) ? 'rounded-bl-lg' : 'rounded-l-lg', navOpen ? 'translate-x-0' : 'translate-x-full']"
            >
                <li v-for="item in NavData.center">
                    <GNuxtLink :to="localePath(item.to)" @click.native="navOpen = !navOpen" :class="['block rounded-full px-4 py-2 lg:px-4 lg:py-1.5 hover:bg-slate-300 dark:hover:bg-slate-800', { 'bg-slate-300 dark:bg-slate-800 font-bold': currentPath.includes(item.to) }]">
                        <component v-if="item.icon" :is="item.icon" class="h-5 w-5" />
                        <template v-else>
                            {{ $t(item.i18n) }}
                        </template>
                    </GNuxtLink>
                </li>
                <li class="lg:hidden px-4 py-2 flex justify-end items-center space-x-4">
                    <button class="hover:opacity-80 relative before:absolute before:-z-10 before:-top-2 before:-left-2 before:w-9 before:h-9 before:rounded-full hover:before:bg-slate-300 dark:hover:before:bg-slate-600 h-5 w-5" @click="rotateColorMode()" aria-label="Change Color Mode">
                        <ClientOnly>
                            <SunIcon class="h-5 w-5" v-if="colorMode.preference === 'light'" />
                            <MoonIcon class="h-5 w-5" v-else-if="colorMode.preference === 'dark'" />
                            <DisplayIcon class="h-5 w-5" v-else />
                        </ClientOnly>
                    </button>
                    <div class="input-group">
                        <span class="input-group-text !rounded-l-full"><I18nIcon class="h-5 w-5" /><span class="sr-only">{{ $t('_nav.switchLang') }}</span></span>
                        <select class="form-select !rounded-r-full" v-model="spLocaleOption" @change="changeLocale()">
                            <option v-for="locale in locales" :value="locale.code">{{ locale.name }}</option>
                        </select>
                    </div>
                </li>
            </ul>
            <div class="text-right">
                <button class="p-1 lg:hidden" @click="navOpen = !navOpen">
                    <XIcon v-if="navOpen" class="h-5 w-5" />
                    <MenuIcon v-else class="h-5 w-5" />
                </button>
                <ul class="hidden lg:col-span-4 lg:space-x-4 lg:flex justify-center">
                    <li>
                        <button :class="['hover:opacity-80', { 'text-white 3xl:text-slate-800 3xl:dark:text-slate-200': (landing && scrollPos >= -40) }]" @click="rotateColorMode()" aria-label="Change Color Mode">
                            <ClientOnly>
                                <SunIcon class="h-5 w-5" v-if="colorMode.preference === 'light'" />
                                <MoonIcon class="h-5 w-5" v-else-if="colorMode.preference === 'dark'" />
                                <DisplayIcon class="h-5 w-5" v-else />
                            </ClientOnly>
                        </button>
                    </li>
                    <li class="relative group">
                        <button class="hover:opacity-80"><I18nIcon :class="['h-5 w-5', { 'text-white 3xl:text-slate-800 3xl:dark:text-slate-200': (landing && scrollPos >= -40) }]" /><span class="sr-only">{{ $t('_nav.switchLang') }}</span></button>
                        <div class="absolute top-6 right-0 hidden group-hover:block z-[9955]">
                            <ul class="px-4 py-2 bg-slate-50 dark:bg-slate-800 rounded-lg shadow-lg space-y-1">
                                <li v-for="locale in locales">
                                    <GNuxtLink :to="switchLocalePath(locale.code)" :lang="locale.code" :class="['_i18n whitespace-nowrap hover:text-accent-600 py-1', {'text-accent-600 font-bold': currentLocale === locale.code}]">
                                        {{ locale.name }}
                                    </GNuxtLink>
                                </li>
                            </ul>
                        </div>
                    </li>
                    <li class="border-l"></li>
                    <li v-for="item in NavData.right" :class="['transition-colors', { 'text-white 3xl:text-slate-800 3xl:dark:text-slate-200': (landing && scrollPos >= -40) }]">
                        <GNuxtLink :to="item.to" class="hover:opacity-80">
                            <component v-if="item.icon" :is="item.icon" class="h-5 w-5" />
                            <template v-else>
                                {{ $t(item.i18n) }}
                            </template>
                        </GNuxtLink>
                    </li>
                </ul>
            </div>
        </nav>
    </div>
</template>

<script setup lang="ts">
import MiIcon from '@/assets/svg/misskey_mi_bi.svg';
import I18nIcon from 'bi/translate.svg';
import SunIcon from 'bi/sun.svg';
import MoonIcon from 'bi/moon-stars.svg';
import DisplayIcon from 'bi/display.svg';
import MenuIcon from 'bi/list.svg';
import XIcon from 'bi/x.svg';
import NavData from '@/assets/data/nav';

const props = withDefaults(defineProps<{
    disableShadow?: boolean;
    slim?: boolean;
    hasBorder?: boolean;
    landing?: boolean;
}>(), {
    disableShadow: false,
    slim: false,
    hasBorder: false,
    landing: false,
});

const navOpen = ref(false);

const { locales, locale: currentLocale } = useI18n();
const route = useRoute();
const { afterEach, push } = useRouter();
const currentPath = ref(route.path);

watch(() => route.path,(to) => {
    currentPath.value = to;
}, {
    immediate: true,
});

const switchLocalePath = useSwitchLocalePath();
const localePath = useLocalePath();
const spLocaleOption = ref<string>(currentLocale.value);
function changeLocale() {
    const path = switchLocalePath(spLocaleOption.value);
    push(path);
}

const colorMode = useColorMode();
function rotateColorMode() {
    const values = ['system', 'light', 'dark'];
    const index = values.indexOf(colorMode.preference);
    const next = (index + 1) % values.length;

    colorMode.preference = values[next];
}

const scrollPos = ref(0);

async function updatePos() {
    scrollPos.value = document.body.getBoundingClientRect().y;
}

if (process.client) {
    window.addEventListener('scroll', updatePos);
    window.addEventListener('resize', updatePos);
}

onUnmounted(() => {
    if (process.client) {
        window.removeEventListener('scroll', updatePos);
        window.removeEventListener('resize', updatePos);
    }
});
</script>
