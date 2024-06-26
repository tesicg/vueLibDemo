<template>
    <v-text-box
        :modelValue="modelValue"
        v-bind="$attrs"
        @update="
            ($event) => {
                modelValue = $event
            }
        "
        @key-press="isNumber"
    >
        <slot />
    </v-text-box>
</template>

<script>
import VTextBox from '@/components/simple/VTextBox.vue'

export default {
    inheritAttrs: false,
    components: {
        VTextBox,
    },
    props: {
        modelValue: {
            type: String,
        },
    },
    data() {
        return {
            typePattern: /^\d+$/,
        }
    },
    methods: {
        isNumber(evt) {
            const verified = String.fromCharCode(evt.event.which).match(this.typePattern)
            if (!verified) {
                evt.event.preventDefault()
            } else {
                return true
            }
        },
    },
}
</script>
