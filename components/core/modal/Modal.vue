<template>
    <div v-show="shouldShowModal" class="modal">
        <div class="closingArea" @click="close"></div>
        <div class="lightBox">
            <div class="lightBox__closeButtonContainer" @click="close">
                <fa icon="times" class="lightBox__closeButton" />
            </div>
            <div class="lightBox__photo__box">
                <div
                    v-for="ImgUrl in getImgUrlArr()"
                    :key="ImgUrl"
                    class="lightBox__photo"
                    :class="{ img__bg: imgBg }"
                >
                    <img :src="ImgUrl" alt="photo" />
                </div>
            </div>
            <div class="lightBox__header">
                <h1 class="lightBox__title">
                    {{ name }}
                </h1>
            </div>
            <div class="lightBox__intro">
                <p class="text-sm">{{ intro }}</p>
                <div v-for="item in description" :key="item.id">
                    <p v-for="line in item" :key="line.id" class="text-sm">
                        {{ line }}
                    </p>
                </div>
            </div>
            <div v-if="websiteUrl" class="lightBox__buttons">
                <text-button
                    :href="websiteUrl"
                    :secondary="true"
                    :primary="false"
                    >{{ $t('website') }}</text-button
                >
            </div>
        </div>
    </div>
</template>

<script>
import i18n from '../../core/modal/Modal.i18n'
import TextButton from '~/components/core/buttons/TextButton'

export default {
    i18n,
    name: 'Modal',
    components: {
        TextButton,
    },
    props: {
        value: { type: Boolean, default: false },
        imgUrls: { type: [Array, String], default: '' },
        imgBg: { type: Boolean, default: false },
        name: { type: String, default: '' },
        intro: { type: String, default: '' },
        websiteUrl: { type: String, default: '' },
        description: { type: Array, default: () => [] },
    },
    data() {
        return {
            shouldShowModal: this.value,
        }
    },
    watch: {
        value(newVal) {
            this.shouldShowModal = newVal
        },
    },
    methods: {
        close() {
            this.shouldShowModal = false
            this.$emit('input', false)
        },
        getImgUrlArr() {
            let arr = []
            if (Array.isArray(this.imgUrls) === true) {
                arr = this.imgUrls
            } else {
                arr.push(this.imgUrls)
            }
            return arr
        },
    },
}
</script>

<style lang="postcss" scoped>
.modal {
    @apply flex justify-center items-center fixed top-0 left-0 w-full h-screen z-[9998];
    background-color: rgba(18, 16, 35, 0.6);
    backdrop-filter: blur(5px);
}

.closingArea {
    @apply w-full h-full absolute z-[9999];
}

.lightBox {
    @apply flex flex-col px-4 py-5 rounded-3xl border-3 w-11/12 relative bg-black-900 border-pink-500;
    @apply md:w-4/5 md:max-w-2xl md:pt-14 md:pb-6 md:px-12 md:border-2 z-[10000];
}

.lightBox__closeButtonContainer {
    @apply absolute flex ml-auto cursor-pointer text-pink-500 top-[17px] right-[17px];
    @media (min-width: 768px) {
        @apply top-[39.25px] right-[38.5px];
    }
}

.lightBox__closeButton {
    @apply w-5 h-5;
}

.lightBox__photo__box {
    @apply relative flex my-0 mx-auto;
}
.lightBox__photo {
    @apply w-16 h-16 md:w-24 md:h-24;
    @apply relative flex flex-col justify-center items-center rounded-2xl my-0 mx-auto;
    transform: translateX(-20px);
}
.lightBox__photo:first-of-type {
    transform: translateX(0px);
}
.lightBox__photo > img {
    @apply absolute object-contain rounded-[inherit];
    width: calc(100% - 10px);
}
.img__bg {
    @apply bg-[#f7f6fe];
}

.lightBox__header {
    @apply flex flex-col items-center;
}

.lightBox__title {
    @apply text-lg mt-3 font-semibold font-serif;
    @apply md:text-xl md:mt-6;
}

.lightBox__intro {
    @apply flex flex-col font-sans text-base font-normal overflow-y-auto mt-3.5;
    @apply md:text-lg md:mt-4 max-h-40 whitespace-pre-line;
}

.lightBox__buttons {
    @apply mx-auto mt-6;
    @apply md:my-6;
}
</style>
