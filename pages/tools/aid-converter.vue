<template>
    <div class='container mx-auto max-w-screen-xl px-6 py-6'>
        <h1 class='text-2xl lg:text-3xl font-bold mb-4'>
            {{ $t('_aidConverter.title') }}
        </h1>
        <div class="grid grid-cols-2 gap-2 md:gap-8 mx-auto max-w-lg mb-4">
            <button :class="['rounded-full py-2 hover:bg-accent-600/60', {'bg-accent-600/40': (tab === 'aidToDate')}]" @click="tab = 'aidToDate'">{{ $t('_aidConverter.aidToDate') }}</button>
            <button :class="['rounded-full py-2 hover:bg-accent-600/60', {'bg-accent-600/40': (tab === 'dateToAid')}]" @click="tab = 'dateToAid'">{{ $t('_aidConverter.dateToAid') }}</button>
        </div>
        <div v-if="tab === 'aidToDate'" class="mx-auto max-w-lg">
            <label class="mb-1" for="aidToDateAid">aid / aidx</label>
            <input class="form-control" id="aidToDateAid" v-model="aidToDateAid" />
            <div class="my-2">
                <button class="btn btn-primary !text-white" @click="doAidToDate()">{{ $t('_aidConverter.aidToDate') }}</button>
            </div>
            <div class="mb-2 p-4 rounded-lg border bg-white dark:bg-[#212529] border-gray-200 dark:border-gray-600">
                {{ aidToDateResult }}
            </div>
        </div>
        <div v-if="tab === 'dateToAid'" class="mx-auto max-w-lg">
            <label class="mb-1" for="dateToAidDate">{{ $t('_aidConverter.date') }}</label>
            <input class="form-control mb-2" id="dateToAidDate" type="datetime-local" v-model="dateToAidDate" />
            <label class="mb-1" for="DateToAidMode">{{ $t('_aidConverter.mode') }}</label>
            <select class="form-select" id="DateToAidMode" v-model="dateToAidMode">
                <option value="aid">aid</option>
                <option value="aidx">aidx</option>
            </select>
            <div class="my-2">
                <button class="btn btn-primary !text-white" @click="doDateToAid()">{{ $t('_aidConverter.dateToAid') }}</button>
            </div>
            <div class="mb-2 p-4 rounded-lg border bg-white dark:bg-[#212529] border-gray-200 dark:border-gray-600">
                {{ dateToAidResult }}
            </div>
        </div>
    </div>
</template>

<script setup lang='ts'>
import { genAid } from '~/assets/js/mi/aid';
import { genAidx } from '~/assets/js/mi/aidx';

definePageMeta({
    layout: 'tools',
});
const TIME2000 = 946684800000;

const tab = ref<'aidToDate' | 'dateToAid'>('aidToDate');

const aidToDateAid = ref<string>('');
const aidToDateResult = ref<string>('');
function doAidToDate() {
    const time = parseInt(aidToDateAid.value.slice(0, 8), 36) + TIME2000;
    const d = new Date(time);
    aidToDateResult.value = d.toLocaleString();
}

const dateToAidDate = ref<string>('');
const dateToAidMode = ref<'aid' | 'aidx'>('aid');
const dateToAidResult = ref<string>('');
function doDateToAid() {
    const d = new Date(dateToAidDate.value);
    if (dateToAidMode.value === 'aid') {
        dateToAidResult.value = genAid(d.getTime());
    } else if (dateToAidMode.value === 'aidx') {
        dateToAidResult.value = genAidx(d.getTime());
    }
}

const { t } = useI18n();
const route = useRoute();

route.meta.title = t('_aidConverter.title');
route.meta.description = t('_aidConverter.description');
</script>

<style module>
.mfmRoot {
    @apply rounded-lg p-6 border break-words overflow-hidden;
    font-family: Hiragino Maru Gothic Pro,BIZ UDGothic,Roboto,HelveticaNeue,Arial,sans-serif;
    line-height: 1.35;
}

.mfmRoot img {
    display: inline;
}
</style>
