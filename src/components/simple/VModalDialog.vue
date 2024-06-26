<template>
    <DxPopup
        v-bind="$attrs"
        :hide-on-outside-click="true"
        :drag-enabled="true"
        :wrapper-attr="popupAttributes"
        position="center"
        @hiding="onHiding"
    >
        <slot />
        <!-- Default buttons -->
        <slot name="footer">
            <DxToolbarItem
                template="save-button"
                toolbar="bottom"
                location="after"
                :disabled="isSaveDisabled"
            />
            <DxToolbarItem template="cancel-button" toolbar="bottom" location="after" />
        </slot>

        <template #save-button>
            <v-button class="save-btn-default" :text="okText" @click="onOkClick" />
        </template>
        <template #cancel-button>
            <v-button class="cancel-btn-default" :text="cancelText" @click="onCancelClick" />
        </template>
    </DxPopup>
</template>

<script>
import VButton from '@/components/simple/VButton.vue'
import { DxPopup, DxToolbarItem } from 'devextreme-vue/popup'

export default {
    inheritAttrs: false,
    components: {
        DxPopup,
        DxToolbarItem,
        VButton,
    },
    props: {
        isSaveDisabled: Boolean,
        okText: String,
        cancelText: String,
    },
    emits: ['ok', 'cancel', 'hiding'],
    data() {
        return {
            popupAttributes: {
                id: 'popup-base',
            },
        }
    },
    methods: {
        onOkClick() {
            this.$emit('ok')
        },
        onCancelClick() {
            this.$emit('cancel')
        },
        onHiding() {
            this.$emit('hiding')
        },
    },
}
</script>

<style>
#popup-base .dx-popup-normal {
    border-radius: 20px;
}
#popup-base .dx-texteditor-input {
    border: none;
    border-style: none;
    color: #8ca8cc;
    font-family: HelveticaNeueCyr, Helvetica, Arial, sans-serif;
    font-size: 14px;
}
#popup-base .dx-popup-title {
    border-bottom: 0px;
    font-weight: bold;
    font-size: 22px;
    color: #0d356c;
}
#popup-base .dx-label {
    color: #0d356c;
}
.save-btn-default {
    background-color: #0d356c;
    border: 1px solid #0d356c;
    color: #fff;
}
.cancel-btn-default {
    border: 1px solid #0d356c;
    color: #0d356c;
}
.save-btn-default:hover {
    background-color: #0d356c;
    border: 1px solid red;
    color: #fff;
}
.cancel-btn-default:hover {
    background-color: white;
    border: 1px solid red;
    color: #0d356c;
}
</style>
