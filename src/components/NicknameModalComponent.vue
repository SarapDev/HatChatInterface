<template>
    <v-container v-show="show">
        <v-row justify="center">
          <v-col cols="12" sm="6">
            <v-card>
              <v-card-title class="text-center">Введите ваше имя</v-card-title>
              <v-card-text>
                <v-text-field v-model="username" label="Имя"></v-text-field>
                <v-btn @click="openChat" color="primary" block>Подключиться к Чату</v-btn>
              </v-card-text>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
      <component :is="currentComponent" :username="username"></component>
</template>

<script lang="ts">
import ChatComponent from "./ChatComponent.vue";

export default {
  data() {
    return {
      currentComponent: {},
      username: "",
      show: true,
    };
  },
  methods: {
    openChat() {
      this.currentComponent = ChatComponent;
      this.show = false;
    },
  },
};

</script>