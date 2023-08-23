<template>
    <div class='flex items-start oveflow-x-auto gap-4'>
        <draggable-component 
            v-model='columns'
            group='columns'
            :animation='150'
            handle='.drag-handle'
            item-key='id'
            class='flex gap-4 items-start'>
            <template #item='{element: column}: {element: Column}'>
                <div class='column bg-gray-200 p-5 rounded min-w-[250px]'>
                    <header class='flex items-center font-bold mb-4'>
                        <drag-handle class='pr-2'/>
                        <span>
                            <input
                                class='bg-transparent focus:bg-white rounded px-1 w-4/5 title-input'
                                @keyup.enter='($event.target as HTMLInputElement).blur()'
                                @keydown.delete='column.title === "" ? columns = columns.filter(el => el.id !== column.id) : null'
                                type='text'
                                v-model='column.title'/>
                        </span>
                    </header>
                    <draggable-component 
                        v-model='column.tasks'
                        :group='{name: "tasks", pull: alt ? "clone" : true}'
                        handle='.drag-handle'
                        :animation=' 150 '
                        item-key='id'>     
                        <template #item='{element: task}: {element: Task}'>
                            <div>
                                <trello-board-task 
                                    :task='task' 
                                    @delete='column.tasks = column.tasks.filter(el => el.id !== $event)'/>
                            </div>
                        </template>          
                    </draggable-component>
                    <footer>
                        <new-task @add='column.tasks.push($event)'/>
                    </footer>                    
                </div>
            </template>
        </draggable-component>
        <button
            @click='createColumn'
            class='bg-gray-200 whitespace-nowrap p-2 rounded opacity-50'>
            + Add Another Column
        </button>
    </div>
</template>

<script setup lang="ts">
import { nanoid } from 'nanoid';
import type { Column, Task } from '@/types';
import draggableComponent from 'vuedraggable';
import DragHandle from '@/components/DragHandle.vue';
import NewTask from '@/components/NewTask.vue';
import { useKeyModifier, useLocalStorage } from '@vueuse/core';

const columns = useLocalStorage<Column[]>("trelloBoard", [
    {
        id: nanoid(),
        title: "Backlog",
        tasks: [
            {
                id: nanoid(),
                title: "Create marketing landing page",
                createdAt: new Date(),
            },
            {
                id: nanoid(),
                title: "Develop cool new feature",
                createdAt: new Date(),
            },
            {
                id: nanoid(),
                title: "Fix page nav bug",
                createdAt: new Date(),
            },                        
        ],
    },
    {
        id: nanoid(),
        title: "Selected for Dev",
        tasks: [],
    },
    {
        id: nanoid(),
        title: "In Progress",
        tasks: [],
    },
    {
        id: nanoid(),
        title: "QA",
        tasks: [],
    },  
    {
        id: nanoid(),
        title: "Complete",
        tasks: [],
    },       
]);

const alt = useKeyModifier("Alt");

watch(columns, () => 
    {
        //ajax requests to update columns on the server side
    }, 
    {
        deep: true
    }
);

function createColumn() {
    const newColumn: Column = {
        id: nanoid(),
        title: "Task Title",
        tasks: [],
    };
    columns.value.push(newColumn);
    nextTick(()=> {
        (document.querySelector(".column:last-of-type .title-input") as HTMLInputElement).focus();
    });
}

</script>

<style>

</style>