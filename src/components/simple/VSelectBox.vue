<template>
    <DxSelectBox
        ref="mySelectbox"
        :value="value"
        :data-source="getSelectBoxData"
        v-bind="$attrs"
        @value-changed="$emit('update:value', $event.value)"
    >
        <template v-for="(_, slot) in $slots" #[slot]="props">
            <slot :name="slot" v-bind="props || {}" />
        </template>
        <slot />
    </DxSelectBox>
</template>

<script>
import DataSource from 'devextreme/data/data_source'
import DxSelectBox from 'devextreme-vue/select-box'

export default {
    inheritAttrs: false,
    components: {
        DxSelectBox,
    },
    props: {
        value: {
            type: String,
        },
        dataMode: {
            type: String,
            default: 'standard',
            validator(value) {
                return ['standard', 'large'].includes(value)
            },
        },
        storeKey: {
            type: String,
        },
    },
    emits: ['update:value'],
    data() {
        return {
            selectBoxData: {},
        }
    },
    computed: {
        getSelectBoxData() {
            const val = Object.values(this.$attrs['data-source'])
            if (this.dataMode === 'standard') return val
            return this.prepareSelectBoxData(val)
        },
    },
    methods: {
        prepareSelectBoxData(val) {
            this.selectBoxData = new DataSource({
                store: {
                    data: val,
                    type: 'array',
                    key: this.storeKey,
                },
                paginate: true,
                pageSize: 10,
            })

            return this.selectBoxData
        },
    },
}
</script>

<style scoped>
:deep(.dx-texteditor-input) {
    background-color: azure;
}
</style>
