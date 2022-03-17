<template>
  <q-page padding>
    <q-form class="row justify-center" @submit.prevent="handlePassswordReset">
      <p class="col-12 text-h5 text-center">Reset Password</p>

      <div class="col-md-4 col-sm-6 col-xs-10 q-gutter-y-md">
        <q-input label="New Password" v-model="password" lazy-rules />

        <div class="full-width q-pt-md q-gutter-y-sm">
          <q-btn
            label="Send New Password"
            color="primary"
            class="full-width"
            outline
            rounded
            type="submit"
          />
        </div>
      </div>
    </q-form>
  </q-page>
</template>

<script>
import useAuthUser from "src/composables/useAuthUser";
import useNotify from "src/composables/useNotify";
import { defineComponent, ref } from "vue";
import { useRoute, useRouter } from "vue-router";

export default defineComponent({
  name: "PageResetPassword",

  setup() {
    const { resetPassword } = useAuthUser();
    const router = useRouter();
    const route = useRoute();
    const { notifyError, notifySuccess } = useNotify();
    const password = ref("");

    const token = route.params.token;

    const handlePassswordReset = async () => {
      try {
        await resetPassword(token, password.value);
        notifySuccess("New Password Sent");
        router.push({ name: "login" });
      } catch (error) {
        notifyError(error.message);
      }
    };

    return {
      password,
      handlePassswordReset,
    };
  },
});
</script>
