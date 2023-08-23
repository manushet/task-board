<template>
    <div 
        class='flex items-start task bg-white p-2 mb-2 rounded shadow-sm max-w-[250px]'
        :title='TaskTitle()'
        @focus='focused = true'
        @blur='focused = false'
        tabindex='0'>
        <DragHandle class='mt-1 pr-2'/>
        <span>
            {{ task.title }}
        </span>
    </div>
</template>

<script setup lang='ts'>
    import type {Task, ID} from '@/types';
    import DragHandle from '@/components/DragHandle.vue';
    import { onKeyStroke } from '@vueuse/core';

    const emit = defineEmits<{
        (e: "delete", payload: ID): void;
    }>();    

    const props = defineProps<{
        task: Task;
    }>();

    const focused = ref(false);

    onKeyStroke("Delete", (e) => {
        if (focused.value) {
            emit("delete", props.task.id);
        }
    });

    function TaskTitle() {
        const d = new Date(props.task.createdAt);
        return d.toLocaleDateString() + ' ' + d.toLocaleTimeString();
    }
</script>

<style scoped>
.sortable-chosen {}

.sortable-drag .task {
    transform: rotate(5deg);
}

.sortable-ghost .task {
    position: relative;
}

.sortable-ghost .task::after {
    content: "";
    @apply absolute top-0 bottom-0 left-0 right-0 bg-slate-300 rounded;
}

.task:focus,
.task:focus-visible {
    @apply outline-gray-400 !important;
    outline: gray auto 1px   
}
</style>