<template>
    <div>
        <Suspense>
            <AsyncCity />
            <template #fallback>
                <CityViewSkeleton />
            </template>
        </Suspense>
    </div>
</template>

<script setup>
    import AsyncCity from './AsyncCity.vue';
    import CityViewSkeleton from '../components/CityViewSkeleton.vue';
</script>