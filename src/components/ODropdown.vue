<template>
    <div class="w-full max-w-full mt-4 lg:mt-0 lg:max-w-[115px]">
        <span class="block mb-1 text-lg font-normal text-black font-montserrat">{{ label }}</span>
        <div
            @mouseout="isDropdownOpen = false" 
            @mouseover="isDropdownOpen = !isDropdownOpen" 
            :class="[isDropdownOpen ? 'before:rotate-0 z-8' : 'before:rotate-180']"
            class="relative w-full px-4 py-2 border rounded border-neutral-500 before:absolute before:top-[10px] before:content-arrow-down-black before:w-6 before:h-6 before:right-4 before:transition before:duration-300 before:ease-in-out min-h-43">
            
            <span ref="label" class="block mr-4 mr-6 overflow-hidden font-normal capitalize font-montserrat text-black text-lg whitespace-nowrap text-ellipsis">{{activeDropdownChoice}}</span>
            
            <div :class="[isDropdownOpen ? 'opacity-1 visible' : 'opacity-0 invisible']"
            class="absolute overflow-hidden z-2 transition-all top-[45px] ease-in-out delay-75 lg:top-[45px] left-0 bg-white min-h-[90px] max-h-[90px] w-full shadow-dropdown rounded-bl-md rounded-br-md">
                <div class="overflow-auto min-h-[90px] max-h-[90px]">
                    <span 
                    v-for="(choice, index) in data" 
                    :key="index" :data-value="choice.value" 
                    @click="setActiveDropdownChoice(choice.label, choice.value)" 
                    :class="activeDropdownChoice == choice.label && 'bg-border-blue-400'"
                    class="block px-4 py-2 text-lg border-b cursor-pointer font-montserrat text-black border-neutral-100">
                    {{ choice.label }}
                    </span>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
export default {
    props: {
        data: {
            type: Array,
            default() {
                return []
            }
        },
        label: {
            type: String,
            default: ''
        }
    },
    data: () => ({
        isDropdownOpen: false,
        activeDropdownChoice: ''
    }),
    methods: {
        /**
         * Sets the active dropdown value
         */
        setActiveDropdownChoice(label, value) {
            this.activeDropdownChoice = label;
            this.isDropdownOpen = false;

            this.$emit('emitDropdownValue', value);
        }
    },
    mounted() {
        this.activeDropdownChoice = this.data[0].label;
    }
}
</script>