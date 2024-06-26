<template>
    <DxTextArea
        class="back-color"
        :value="value"
        @value-changed="$emit('update:value', $event.value)"
        v-bind="$attrs"
    >
        <slot />
    </DxTextArea>
</template>

<script>
import { DxTextArea } from 'devextreme-vue/text-area'

export default {
    inheritAttrs: false,
    components: {
        DxTextArea,
    },
    props: {
        value: {
            type: String,
        },
    },
    emits: ['update:value'],
}
</script>

<style scoped>
.back-color {
    background-color: azure;
}
</style>
