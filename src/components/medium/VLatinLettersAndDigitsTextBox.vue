<template>
    <v-text-box
        :modelValue="modelValue"
        v-bind="$attrs"
        @update="
            ($event) => {
                modelValue = $event
            }
        "
        @key-press="isLatinLetterOrNumber"
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
            typePattern: /^[\da-z]+$/i,
        }
    },
    methods: {
        isLatinLetterOrNumber(evt) {
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
