<template>
    <input 
    :value="value" 
    @input="$emit('input', $event.target.value)" 
    class="border-b border-b-slate-800 rounded-none px-4 font-montserrat text-xl text-slate-900 h-fit py-4 w-full lg:w-[400px] outline-none"
    placeholder="Search..."/>
</template>
<script>
export default {
    props: {
        value: {
            type: String,
            default: ''
        }
    }
}
</script>