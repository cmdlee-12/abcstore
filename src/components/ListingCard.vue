<template>
    <div class="w-full lg:max-w-[calc(100%/3-16px)] lg:[&:not(:nth-child(3n))]:mr-6 border border-slate-600 rounded-md mb-10 pt-4">
        <picture>
            <img :src="data.image" :alt="data.title" class="object-contain w-72 h-72 mx-auto">
        </picture>
        <div class="py-6 px-4">
            <span class="font-montserrat text-white bg-blue-900 px-2 py-2 tracking-wide uppercase text-xs mb-2 block w-fit">
                {{ data.category }}
            </span>
            <h3 class="font-montserrat text-black text-lg tracking-tight font-medium">
                {{ data.title }}
            </h3>
            <h4 class="font-montserrat text-slate-900 text-2xl font-bold tracking-wide" v-html="price"></h4>
            <div 
            v-if="data.rating.rate !== ''"
            class="mt-2 flex">
                <svg v-for="rating in totalRatings" :key="rating" class="mr-2" height="16px" width="16px" viewBox="0 0 53.867 53.867" xml:space="preserve">
                    <polygon stroke="#facc15" :fill="setRating(rating)" points="26.934,1.318 35.256,18.182 53.867,20.887 40.4,34.013 43.579,52.549 26.934,43.798 10.288,52.549 13.467,34.013 0,20.887 18.611,18.182 "/>
                </svg>
            </div>
        </div>
    </div>
</template>
<script>
export default {
    props: {
        data: {
            type: Object,
            default() {
                return {}
            }
        }
    },
    data: () => ({
        totalRatings: [1, 2, 3, 4, 5]
    }),
    computed: {
        /**
         * Adds currency sign on the price
         */
        price() {
            return `$${this.data.price}`
        }
    },
    methods: {
        /**
         * Rounds the rating to the nearest integer
         */
        rating(rate) {
            return Math.round(rate);
        },
        setRating(index) {
            return index <= this.rating(this.data.rating.rate) ? '#facc15' : 'transparent';
        }
    }
}
</script>