<!-- SPDX-FileCopyrightText: Copyright (c) 2022-2023 trobonox <hello@trobo.tech> -->
<!-- -->
<!-- SPDX-License-Identifier: Apache-2.0 -->

<template>
  <div
    class="bg-elevation-2 flex aspect-video h-32 flex-row gap-4 overflow-hidden rounded-t-md p-2"
  >
    <div
      v-for="column in board.columns"
      :key="column.id"
      class="bg-accent-no-hover flex h-min w-10 shrink-0 flex-col gap-0.5 rounded-sm p-1"
    >
      <div
        v-for="card in column.cards"
        :key="card.id"
        class="bg-elevation-2 mb-1 rounded-sm p-2"
      />
    </div>
  </div>
</template>

<script setup lang="ts">
import type { Board } from "@/types/kanban-types";

defineProps<{
    board: Board;
}>();
</script>
