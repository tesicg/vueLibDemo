<template>
    <DxTextBox :value="value" @value-changed="$emit('update:value', $event.value)" v-bind="$attrs">
        <slot />
    </DxTextBox>
</template>

<script>
import { DxTextBox } from 'devextreme-vue/text-box'

export default {
    inheritAttrs: false,
    components: {
        DxTextBox,
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
:deep(.dx-texteditor-input) {
    background-color: azure;
}
</style>
