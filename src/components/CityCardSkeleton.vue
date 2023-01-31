<template>
    <div class="flex py-6 px-3 bg-weather-secondary rounded-md shadow-md text-white">
        <div class="flex flex-col flex-1 gap-2">
            <AminatePlaceholder class="max-w-[50%]"></AminatePlaceholder>
            <AminatePlaceholder class="max-w-[40%]"></AminatePlaceholder>
        </div>
        <div class="flex flex-col items-end flex-1 gap-2">
            <AminatePlaceholder class="max-w-[50px] w-full"></AminatePlaceholder>
            <AminatePlaceholder class="max-w-[75px] w-full"></AminatePlaceholder>
        </div>
    </div>
</template>

<script setup>
    import AminatePlaceholder from './AminatePlaceholder.vue';
</script>
