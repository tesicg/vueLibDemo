<template>
    <div>
        <v-data-table
            :data-source="store"
            :columns="columns"
            :isFilterRowEnabled="false"
            :isLoading="isLoading"
            @selection-changed="selectEmployee"
            @row-dbl-click="rowDblClick"
        >
            <template #tools>
                <DxToolbar>
                    <DxItem location="before" template="app-title" />
                    <DxItem location="after" template="add-button" />
                    <DxItem location="after" />
                    <DxItem location="after" />
                    <DxItem location="after" template="remove-button" :disabled="isDisabled" />
                </DxToolbar>
            </template>

            <template #app-title>
                <div><strong>APPLICATION TYPES</strong></div>
            </template>

            <template #add-button>
                <v-button
                    class="button-border"
                    icon="add"
                    text="Add Application Type"
                    @click="addApplicationType()"
                />
            </template>
            <template #remove-button>
                <v-button
                    class="button-border"
                    icon="remove"
                    text="Remove Application Type"
                    @click="removeApplicationType()"
                />
            </template>

            <template #link-to-modal="{ data }">
                <a href="#" @click="openConnectionModal(data)">
                    {{ data.data.type }}
                </a>
            </template>
        </v-data-table>

        <app-types-modal ref="appTypesModal" @ok="onOk" />
        <v-confirm ref="confirm" :width="400" :height="200" @ok="onOkConfirm" />
    </div>
</template>

<script>
import ArrayStore from 'devextreme/data/array_store'
import { testData } from '../data.js'
import { DxToolbar } from 'devextreme-vue/data-grid'
import { DxItem } from 'devextreme-vue/form'
import VDataTable from '@/components/simple/VDataTable.vue'
import AppTypesModal from '@/components/complex/AppTypesModal.vue'
import VButton from '@/components/simple/VButton.vue'
import VConfirm from '@/components/simple/VConfirm.vue'

export default {
    components: {
        VDataTable,
        AppTypesModal,
        DxToolbar,
        DxItem,
        VButton,
        VConfirm,
    },
    data() {
        return {
            isLoading: false,
            testData,
            store: [],
            columns: [
                {
                    dataField: 'id',
                    caption: 'ID',
                    dataType: 'string',
                    visible: false,
                },
                {
                    dataField: 'listItem',
                    caption: 'List Item',
                    dataType: 'string',
                },
                {
                    dataField: 'type',
                    caption: 'Type',
                    dataType: 'string',
                    cellTemplate: 'link-to-modal',
                },
                {
                    dataField: 'link',
                    caption: 'Link',
                    dataType: 'string',
                },
                {
                    dataField: 'digits',
                    caption: 'Digits',
                    dataType: 'string',
                },
                {
                    dataField: 'singleLineText',
                    caption: 'Single Line',
                    dataType: 'string',
                },
                {
                    dataField: 'multiLineText',
                    caption: 'Multi Line',
                    dataType: 'string',
                },
            ],
            selectedEmployee: undefined,
        }
    },
    mounted() {
        this.store = new ArrayStore({
            key: 'id',
            data: testData,
        })
    },
    computed: {
        isDisabled() {
            return this.selectedEmployee ? false : true
        },
    },
    methods: {
        selectEmployee(e) {
            e.component.byKey(e.currentSelectedRowKeys[0]).done((employee) => {
                if (employee) {
                    this.selectedEmployee = employee
                }
            })
        },
        rowDblClick(e) {
            this.showModal(e.data)
        },
        openConnectionModal(data) {
            this.showModal(data.data)
        },
        showModal(selectedReqtype) {
            this.$refs.appTypesModal.showModal(selectedReqtype)
        },
        onOk(obj) {
            if (!obj.id) {
                const idsArray = this.store._array.map((item) => Number(item.id))
                const maxId = Math.max(...idsArray)
                const newItem = {
                    id: maxId + 1,
                    listItem: obj.listItem,
                    type: obj.type,
                    link: obj.link,
                    digits: obj.digits,
                    singleLineText: obj.singleLineText,
                    multiLineText: obj.multiLineText,
                }
                this.store.push([{ type: 'insert', data: newItem, index: maxId }])
            } else {
                this.store.push([{ type: 'update', data: obj, key: this.store.keyOf(obj) }])
            }
        },
        addApplicationType() {
            const emptyItem = {
                id: '',
                listItem: '',
                type: '',
                link: '',
                digits: '',
                singleLineText: '',
                multiLineText: '',
            }
            this.showModal(emptyItem)
        },
        removeApplicationType() {
            this.$refs.confirm.showModal()
        },
        onOkConfirm() {
            this.store.push([{ type: 'remove', key: this.store.keyOf(this.selectedEmployee) }])
            this.selectedEmployee = undefined
        },
    },
}
</script>

<style scoped>
.button-border {
    border: 0px;
}
</style>
