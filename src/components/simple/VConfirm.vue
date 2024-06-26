<template>
    <v-modal-dialog
        :visible="isModalVisible"
        :title="titleText"
        :okText="yesText"
        :cancelText="noText"
        :width="width"
        :height="height"
        @ok="onOk"
        @cancel="onCancel"
        @hiding="onHiding"
    >
        <DxForm>
            <DxItem template="message-template" />
            <template #message-template>{{ messageText }}</template>
        </DxForm>
    </v-modal-dialog>
</template>

<script>
import { DxForm, DxItem } from 'devextreme-vue/form'
import VModalDialog from '@/components/simple/VModalDialog.vue'

export default {
    components: {
        VModalDialog,
        DxForm,
        DxItem,
    },
    props: {
        titleText: {
            type: String,
            default: 'Confirmation',
        },
        messageText: {
            type: String,
            default: 'Are you sure?',
        },
        yesText: {
            type: String,
            default: 'Yes',
        },
        noText: {
            type: String,
            default: 'No',
        },
        width: {
            type: Number,
            default: 300,
        },
        height: {
            type: Number,
            default: 200,
        },
    },
    emits: ['ok', 'cancel'],
    data() {
        return {
            isModalVisible: false,
        }
    },
    methods: {
        showModal() {
            this.isModalVisible = true
        },
        onOk() {
            this.$emit('ok')
            this.isModalVisible = false
        },
        onCancel() {
            this.$emit('cancel')
            this.isModalVisible = false
        },
        onHiding() {
            this.isModalVisible = false
        },
    },
}
</script>
