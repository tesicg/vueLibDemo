<template>
    <DxButton v-bind="$attrs">
        <slot />
    </DxButton>
</template>

<script>
import DxButton from 'devextreme-vue/button'

export default {
    inheritAttrs: false,
    components: {
        DxButton,
    },
}
</script>
