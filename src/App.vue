<template>
    <div id="app">
        <list-manager />
    </div>
</template>

<script>
import 'devextreme/dist/css/dx.light.css'
import ListManager from '@/pages/ListManager.vue'

export default {
    name: 'App',
    components: {
        ListManager,
    },
}
</script>

<style scoped>
#app {
    width: 70%;
}
</style>
