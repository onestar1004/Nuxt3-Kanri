<template>
  <ClickCounter
    v-if="!cardNameEditMode"
    @single-click="$emit('openKanbanModal')"
    @double-click="enableCardEditMode"
  >
    <p>{{ name }}</p>
  </ClickCounter>
  <textarea
    v-else
    ref="cardNameInput"
    v-model="name"
    v-resizable
    type="text"
    maxlength="1000"
    class="bg-elevation-3 h-full w-full rounded-sm focus:outline-none"
    @blur="updateCardName"
    @keypress.enter="updateCardName"
  />
</template>

<script setup lang="ts">
import { Ref } from "vue";
import { Card } from "@/types/kanban-types";

const props =defineProps<{
    card: Card;
    cardIndex: number;
}>();

// eslint-disable-next-line @typescript-eslint/no-unused-vars
const emit = defineEmits(["openKanbanModal", "updateCardName", "disableDragging", "enableDragging"]);

const name = ref(props.card.name);
const cardNameEditMode = ref(false);

const cardNameInput: Ref<HTMLInputElement | null> = ref(null);

watch(props.card, (_, newData) => {
    name.value = newData.name;
})

const enableCardEditMode = () => {
    emit("disableDragging");

    cardNameEditMode.value = true;
    nextTick(() => {
        if (cardNameInput.value == null) return;
        cardNameInput.value.focus();
    });
}

const updateCardName = () => {
    if (name.value == null || !(/\S/.test(name.value))) {
        name.value = props.card.name;
        cardNameEditMode.value = false;
        return;
    }

    emit("updateCardName", props.cardIndex, name.value);
    cardNameEditMode.value = false;
    emit("enableDragging");
}
</script>