<template>
  <v-layout class="rounded rounded-md">
    <v-app-bar title="Web Socket Chat"></v-app-bar>

    <v-navigation-drawer>
      <v-list>
        <v-list-item title="Users In Room"></v-list-item>
        <ChatMemberComponent />
      </v-list>
    </v-navigation-drawer>

    <v-main class="d-flex align-center justify-center" style="min-height: 300px;">
        <NicknameModalComponent />
    </v-main>
  </v-layout>
</template>

<script lang="ts" setup>
    import NicknameModalComponent from '@/components/NicknameModalComponent.vue';
    import ChatMemberComponent from '@/components/ChatMemberComponent.vue';
</script>



