<template>
    <DxDataGrid
        v-bind="$attrs"
        :auto-navigate-to-focused-row="true"
        :focused-row-enabled="true"
        :hover-state-enabled="true"
        :show-borders="false"
        :show-column-lines="true"
    >
        <template v-for="(_, slot) in $slots" #[slot]="props">
            <slot :name="slot" v-bind="props || {}" />
        </template>

        <slot name="tools" />

        <DxLoadPanel :enabled="true" />
        <DxFilterRow :visible="isFilterRowEnabled" />
        <DxSelection mode="single" />
        <DxScrolling v-if="isVirtualScrollingEnabled" mode="virtual" />
        <DxPaging v-else :page-size="pageSize" :enabled="isPageSizeEnabled" />
        <DxPager />
    </DxDataGrid>
</template>

<script>
import {
    DxDataGrid,
    DxScrolling,
    DxSelection,
    DxPaging,
    DxFilterRow,
    DxPager,
    DxLoadPanel,
    DxToolbar,
    DxItem,
} from 'devextreme-vue/data-grid'
import VButton from '@/components/simple/VButton.vue'

export default {
    inheritAttrs: false,
    components: {
        DxDataGrid,
        DxScrolling,
        DxSelection,
        DxFilterRow,
        DxPaging,
        DxPager,
        DxLoadPanel,
        DxToolbar,
        DxItem,
        VButton,
    },
    props: {
        isLoading: {
            type: Boolean,
        },
        pageSize: {
            type: Number,
            default: 2,
        },
        isPageSizeEnabled: {
            type: Boolean,
            default: false,
        },
        isFilterRowEnabled: {
            type: Boolean,
            default: false,
        },
        isVirtualScrollingEnabled: {
            type: Boolean,
            default: true,
        },
    },
}
</script>

<style scoped>
.button-border {
    border: 0px;
}
.add-button {
    border: 0px;
}
:deep(.dx-button-content) {
    padding: inherit;
}
:deep(.dx-icon) {
    border: 1px solid #8ca8cc;
    border-radius: 16px;
    padding: 4px;
}
:deep(.dx-datagrid-text-content) {
    color: #0d356c;
    font-size: 14px;
    font-weight: 700;
    font-family: HelveticaNeueCyr, Helvetica, Arial, sans-serif;
}
.loading-indicator {
    text-align: center;
    padding-top: 110px;
}
</style>
