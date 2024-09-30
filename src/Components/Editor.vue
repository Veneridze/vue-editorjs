<template>
    <div class="editor">

        <div id="editorjs"></div>
    </div>
</template>
<script setup>
import { onMounted, defineExpose, defineProps, readonly } from 'vue';
import EditorJS from '@editorjs/editorjs';
import Header from '@editorjs/header';
import NestedList from '@editorjs/nested-list';
import underline from '@editorjs/underline';
import hyperlink from 'editorjs-hyperlink';
import table from '@editorjs/table';
import List from '@editorjs/list';
import DragDrop from "editorjs-drag-drop";

const saveData = async function () {
    //alert('tset');
    value.value = await editor.save();
}
const value = defineModel();

const props = defineProps({
    data: {
        type: Object,
        required: true
    },
    readonly: {
        type: Boolean,
        required: false,
        default: false
    }
});

defineExpose({
    saveData
});

let editor;
onMounted(async () => {
    //alert('test');
    //value.value = 'test';
    editor = new EditorJS({
        holder: 'editorjs',
        tools: {
            header: Header,
            list: List,
            NestedList,
            underline, table, hyperlink,
        },
        readOnly: props.readonly,
        data: props.data,
        inlineToolbar: true,
        hideToolbar: false,
    });

    try {
        await editor.isReady;
        //console.log('Editor.js is ready to work!')
        new DragDrop(editor);
        /** Do anything you need after editor initialization */
    } catch (reason) {
        console.log(`Editor.js initialization failed because of ${reason}`)
    }
})
</script>
<style>
.editor:not([readonly="true"]) {
    /*padding: 6px 62px;
    padding-right: 6px; */
    border: 1px solid var(--el-color-primary);
    border-radius: 5px;
    padding: 10px;
    width: 100%;
}

.editor:not([readonly="true"]) .ce-block__content {
    border: 3px solid #dcd9d9 !important;
    /*margin: 10px;*/
    padding: 0 10px;
    margin-bottom: 5px;
}
</style>