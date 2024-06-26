<template>
    <div>
        <v-text-box
            v-if="ctrlType === false"
            :value="content"
            @update="
                ($event) => {
                    content = $event
                }
            "
            v-bind="$attrs"
        >
            <slot />
        </v-text-box>

        <fieldset v-else class="link-block-style">
            <legend class="legend-style">Text link</legend>
            <a class="anchor-link-style" :href="content" target="_blank">
                <label class="button-link-style">{{ content }}</label>
            </a>
            <i class="clear-link-style dx-icon-close" @click="close()" />
        </fieldset>
    </div>
</template>

<script>
import VTextBox from '@/components/simple/VTextBox.vue'

function initState() {
    return false
}

export default {
    inheritAttrs: false,
    components: {
        VTextBox,
    },
    props: {
        value: {
            type: String,
        },
        controlType: {
            type: Boolean,
            default: false,
        },
    },
    data() {
        return {
            ctrlType: initState(),
            content: '',
        }
    },
    watch: {
        controlType(newControlType) {
            this.ctrlType = newControlType
        },
        value(newVal) {
            this.content = newVal
        },
    },
    methods: {
        close() {
            this.ctrlType = initState()
            this.content = ''
        },
    },
}
</script>

<style scoped>
.legend-style {
    padding-left: 0px;
    color: #999;
    font-size: 12px;
}
.link-block-style {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    background-color: azure;
    border: solid 1px gainsboro;
    border-radius: 4px;
    padding-top: 0px;
    padding-bottom: 0px;
    padding-right: 0px;
    margin: 0;
}
.anchor-link-style {
    display: flex;
    margin-left: -2px;
    margin-top: -3px;
    background-color: azure;
    color: #8ca8cc;
    width: auto;
    height: 28px;
    font-family:
        Helvetica Neue,
        Segoe UI,
        helvetica,
        verdana,
        sans-serif;
}
.button-link-style {
    border: none;
    background-color: azure;
    color: #8ca8cc;
    cursor: pointer;
    font-family:
        Helvetica Neue,
        Segoe UI,
        helvetica,
        verdana,
        sans-serif;
}
.clear-link-style {
    background-color: azure;
    display: flex;
    align-items: center;
    justify-content: center;
    margin-top: -5px;
    margin-right: 8px;
    cursor: pointer;
}
</style>
