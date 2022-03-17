<template>
  <q-page class="flex flex-center">
    <div v-if="user">Hello {{ user.user_metadata.name }}</div>
  </q-page>
</template>

<script>
import useAuthUser from "src/composables/useAuthUser";
import { defineComponent } from "vue";

export default defineComponent({
  name: "PageMe",

  setup() {
    const { user } = useAuthUser();

    return {
      user,
    };
  },
});
</script>
