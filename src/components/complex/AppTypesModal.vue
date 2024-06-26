<template>
    <v-modal-dialog
        :visible="isModalVisible"
        title="App Types Dialog"
        okText="Save"
        cancelText="Cancel"
        :isSaveDisabled="isDisabled"
        :width="550"
        :height="550"
        @ok="onOk"
        @cancel="onCancel"
        @hiding="onHiding"
    >
        <DxForm
            :form-data="selectedItem"
            label-mode="static"
            validation-group="selectedData"
            :scrollingEnabled="isScrollingEnabled"
        >
            <DxItem template="list-item" :is-required="true" />
            <template #list-item>
                <v-latin-letters-and-digits-text-box
                    v-bind:value="selectedItem.listItem"
                    v-on:update:value="
                        ($event) => {
                            selectedItem.listItem = $event
                        }
                    "
                    label="List item"
                    label-mode="static"
                >
                    <DxValidator validation-group="selectedData">
                        <DxRequiredRule message="Required field: List item." />
                    </DxValidator>
                </v-latin-letters-and-digits-text-box>
            </template>

            <DxItem template="types" :is-required="true" />
            <template #types>
                <v-select-box
                    :modelValue="selectedItem.type"
                    :data-source="typeList"
                    @update:value="
                        ($event) => {
                            selectedItem.type = $event
                        }
                    "
                    label="Type"
                    label-mode="static"
                    value-expr="typeListItem"
                    display-expr="typeListItem"
                    :show-clear-button="true"
                >
                    <DxValidator validation-group="selectedData">
                        <DxRequiredRule message="Required field: Type." />
                    </DxValidator>
                </v-select-box>
            </template>

            <DxItem template="text-link" />
            <template #text-link>
                <v-text-link
                    v-bind:value="selectedItem.link"
                    v-on:update:value="
                        ($event) => {
                            selectedItem.link = $event
                        }
                    "
                    label="Text link"
                    label-mode="static"
                    :controlType="isValidLink"
                    @focusIn="onFocusIn"
                    @focusOut="onFocusOut"
                />
            </template>

            <DxItem template="text-digits" />
            <template #text-digits>
                <v-digits-text-box
                    v-bind:value="selectedItem.digits"
                    v-on:update:value="
                        ($event) => {
                            selectedItem.digits = $event
                        }
                    "
                    label="Digits only"
                    label-mode="static"
                />
            </template>

            <DxItem template="single-text" />
            <template #single-text>
                <v-text-box
                    v-bind:value="selectedItem.singleLineText"
                    v-on:update:value="
                        ($event) => {
                            selectedItem.singleLineText = $event
                        }
                    "
                    label="Single line"
                    label-mode="static"
                />
            </template>

            <DxItem template="multi-text" />
            <template #multi-text>
                <v-text-area
                    :value="selectedItem.multiLineText"
                    v-on:update:value="
                        ($event) => {
                            selectedItem.multiLineText = $event
                        }
                    "
                    label="Multi line"
                    label-mode="static"
                    :height="90"
                />
            </template>
        </DxForm>

        <!-- Uncomment the following block to display non-default buttons -->
        <!-- <template #footer>
            <DxToolbarItem toolbar="bottom" location="after">
                <v-button class="button1-style" text="Button1" @click="onButton1" />
            </DxToolbarItem>
            <DxToolbarItem toolbar="bottom" location="after">
                <v-button text="Button2" @click="onButton2" />
            </DxToolbarItem>
            <DxToolbarItem toolbar="bottom" location="after">
                <v-button text="Button3" @click="onButton3" />
            </DxToolbarItem>
            <DxToolbarItem toolbar="bottom" location="after">
                <v-button text="Button4" @click="onButton4" />
            </DxToolbarItem>
        </template> -->
    </v-modal-dialog>
</template>

<script>
import cloneDeep from 'lodash/cloneDeep'
import { DxForm, DxItem, DxLabel, DxRequiredRule as FormDxRequiredRule } from 'devextreme-vue/form'
import DxSelectBox from 'devextreme-vue/select-box'
import DxValidator, { DxRequiredRule, DxPatternRule } from 'devextreme-vue/validator'
// Uncomment next two lines to display non-default buttons
// import { DxToolbarItem } from 'devextreme-vue/popup'
// import VButton from '@/components/simple/VButton.vue'
import VTextLink from '@/components/medium/VTextLink.vue'
import VTextBox from '@/components/simple/VTextBox.vue'
import VTextArea from '@/components/simple/VTextArea.vue'
import VSelectBox from '@/components/simple/VSelectBox.vue'
import VDigitsTextBox from '@/components/medium/VDigitsTextBox.vue'
import VLatinLettersAndDigitsTextBox from '@/components/medium/VLatinLettersAndDigitsTextBox.vue'
import VModalDialog from '@/components/simple/VModalDialog.vue'

export default {
    components: {
        DxForm,
        DxItem,
        DxLabel,
        DxSelectBox,
        DxValidator,
        DxRequiredRule,
        DxPatternRule,
        FormDxRequiredRule,
        // Uncomment next two lines to display non-default buttons
        // DxToolbarItem,
        // VButton,
        VTextLink,
        VTextBox,
        VTextArea,
        VSelectBox,
        VDigitsTextBox,
        VLatinLettersAndDigitsTextBox,
        VModalDialog,
    },
    emits: ['ok'],
    data() {
        return {
            isModalVisible: false,
            isScrollingEnabled: true,
            selectedItem: {},
            linkContent: 'Link to page',
            patternName:
                /^https?:\/\/(?:www\.)?[-a-zA-Z0-9@:%._\+~#=]{1,256}\.[a-zA-Z0-9()]{1,6}\b(?:[-a-zA-Z0-9()@:%_\+.~#?&\/=]*)$/,
            isValidLink: false,
            typeList: [
                { id: 1, typeListItem: 'A' },
                { id: 2, typeListItem: 'B' },
                { id: 3, typeListItem: 'C' },
            ],
        }
    },
    computed: {
        isDisabled() {
            if (!this.selectedItem.listItem || !this.selectedItem.type) {
                return true
            }
            return false
        },
    },
    methods: {
        //  Uncomment the following block to enable non-default buttons' event handlers
        // onButton1() {
        //     console.log('Button1')
        // },
        // onButton2() {
        //     console.log('Button2')
        // },
        // onButton3() {
        //     console.log('Button3')
        // },
        // onButton4() {
        //     console.log('Button4')
        // },
        onFocusIn() {
            this.isValidLink = false
        },
        onFocusOut() {
            const res = this.selectedItem.link.match(this.patternName)
            if (res) {
                this.isValidLink = true
            } else {
                this.isValidLink = false
            }
        },
        async showModal(item) {
            this.selectedItem = cloneDeep(item)
            this.linkContent = 'Link to page'
            this.isModalVisible = true
        },
        async onOk() {
            this.$emit('ok', this.selectedItem)
            this.isModalVisible = false
        },
        onCancel() {
            this.clearFormData()
            this.isModalVisible = false
            this.isValidLink = false
        },
        clearFormData() {
            this.selectedItem = {
                id: '',
                type: null,
                link: '',
                typeName: null,
            }
        },
        onHiding() {
            this.clearFormData()
            this.isModalVisible = false
        },
        closeLink() {
            this.linkContent = ''
        },
    },
}
</script>

<style scoped>
.button1-style {
    background-color: yellow;
    color: red;
    border-color: green;
}
.link_box {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
    border: solid 1px gainsboro;
    height: 34px;
    border-radius: 4px;
}
.link-content {
    margin-left: 8px;
}
.link-clear {
    margin-right: 8px;
    cursor: pointer;
}
.no-border {
    border: none;
}
</style>
