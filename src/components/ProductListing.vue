<template>
    <section class="max-w-7xl mx-auto py-10 md:py-20 px-4 md:px-14">
        <div class="flex flex-wrap lg:flex-nowrap justify-between mb-6">
            <h1 class="font-montserrat font-bold text-5xl text-blue-900 w-full mb-6 lg:mb-0">
                {{ heading }}
            </h1>
            <div class="flex w-full flex-wrap lg:flex-nowrap justify-end items-end">
                <OInput
                v-model="search"
                class="lg:mr-6"
                />
                <ODropdown
                :data="dropdownValues"
                label="Filter by:"
                @emitDropdownValue="emitDropdownValue"
                />
            </div>
        </div>
        <div class="flex flex-wrap transition-all duration-500 ease-linear">
            <ListingCard
            v-for="(product, index) in filteredList"
            :key="index"
            :data="product"
            />
        </div>
        <paginate
        v-model="page"
        :page-count="pages.length"
        :page-range="pageRange"
        :click-handler="scrollToTop"
        page-class="flex mr-4 rounded-sm"
        page-link-class="px-2 py-1 text-xs font-normal link text-true-gray-225 font-almarai leading-18"
        active-class="active-link bg-blue-900 text-white"
        next-class="hidden"
        prev-class="hidden"
        :hide-prev-next="true"
        class="inline-flex justify-center w-full xs:mb-8 2lg:mb-0 2lg:mt-4 o-pagination"> 
        </paginate>
    </section>
</template>
<script>
import axios from 'axios';
import Paginate from 'vuejs-paginate';
import ListingCard from './ListingCard.vue';
import OInput from './OInput.vue';
import ODropdown from './ODropdown.vue';
export default {
    components: {
        Paginate,
        ListingCard,
        OInput,
        ODropdown
    },
    data: () => ({
        heading: 'Products',
        dropdownValues: [
            {
                label: 'ASC',
                value: 'asc'
            },
            {
                label: 'DESC',
                value: 'desc'
            }
        ],
        allProducts: [],
        activeFilter: '',
        search: '',

        page: 1,
		perPage: 6,
        pageRange: 5,
		pages: [],
        allFilteredProducts: []
    }),
    computed: {
        filteredList() {
            let _filteredProducts = this.allProducts;
    
            // filters the products based on the search keyword
            if (this.search !== '' && this.search) {
                _filteredProducts = _filteredProducts.filter((item) => {
                return item.title
                    .toUpperCase()
                    .includes(this.search.toUpperCase())
                })
            }

            // filters the products based on sort data
            _filteredProducts = this.sortData(_filteredProducts, this.activeFilter);

            this.setPages(_filteredProducts);
            return this.paginate(_filteredProducts);
        }
    },
    watch: {
        search() {
            //sets the page to 1 whenever the user inputs something
            this.page = 1;
        }
    },
    methods: {
        async getProducts() {
            await axios.get(`https://fakestoreapi.com/products`)
            .then(res => {
                this.allProducts = this.sortData(res.data, 'asc');
            }).catch( err => {
                console.log('Error getting the products: ' +err);
            });
        },
         /**
         * Sets the number of pages
         */
        setPages(items) {
            this.pages = [];

            let numberOfPages = Math.ceil(items.length / this.perPage);
            for (let index = 1; index <= numberOfPages; index++) {
                this.pages.push(index);
            }
        },
        /**
         * Sets the number of items to show per page
         */
        paginate(posts) {
            let page = this.page;
            let perPage = this.perPage;
            let from = page * perPage - perPage;
            let to = page * perPage;

            console.log(page, perPage, from, to)

            return posts.slice(from, to);
		},
        /**
         * Scrolls to top when user clicks on the pagination
         */
        scrollToTop() {
            if(this.$refs.lists){
                window.scrollTo({
                    top: this.$refs.lists.offsetTop, 
                    behavior: "smooth" 
                });
            }
        },
        /**
         * Sorts data in asc or desc order
         */
        sortData(data, order) {
            switch(order) {
                case 'asc': 
                    return data.sort((a, b) => {
                        return b.title < a.title ? 1: -1;
                    });
                case 'desc':
                    return data.sort((a, b) => {
                        return a.title < b.title ? 1: -1;
                    });
                default:
                    return data.sort((a, b) => {
                        return b.title < a.title ? 1: -1;
                    });
            }
        },
        /**
         * Sets the active dropdown value
         */
        emitDropdownValue(data) {
            this.activeFilter = data;
        },
    },
    created() {
        this.getProducts();
    }
}
</script>
