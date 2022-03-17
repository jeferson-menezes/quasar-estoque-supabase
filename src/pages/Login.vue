<template>
  <q-page padding>
    <q-form @submit.prevent="handleLogin" class="row justify-center">
      <p class="col-12 text-h5 text-center">Login</p>

      <div class="col-md-4 col-sm-6 col-xs-10 q-gutter-y-md">
        <q-input v-model="form.email" type="text" label="Email" />
        <q-input v-model="form.password" type="text" label="Senha" />

        <div class="full-width q-pt-md">
          <q-btn
            type="submit"
            class="full-width q-mt-md"
            color="primary"
            label="Login"
            outline
            rounded
          />
        </div>

        <div class="full-width q-gutter-y-sm">
          <q-btn
            class="full-width q-mt-md"
            color="primary"
            label="Register"
            flat
            rounded
            :to="{ name: 'register' }"
            size="sm"
          />

          <q-btn
            class="full-width q-mt-md"
            color="primary"
            label="Forgot Password ?"
            flat
            rounded
            :to="{ name: 'forgot-password' }"
            size="sm"
          />
        </div>
      </div>
    </q-form>
  </q-page>
</template>

<script>
import { defineComponent, onMounted, ref } from "vue";
import useAuthUser from "src/composables/useAuthUser";
import { useRouter } from "vue-router";
import useNotify from "src/composables/useNotify";

export default defineComponent({
  name: "Login",

  setup() {
    const router = useRouter();
    const { login, isLoggedIn } = useAuthUser();

    const { notifyError, notifySuccess } = useNotify();

    const form = ref({
      email: "",
      password: "",
    });

    onMounted(() => {
      if (isLoggedIn()) {
        router.push({ name: "me" });
      }
    });

    const handleLogin = async () => {
      try {
        await login(form.value);
        notifySuccess("Login successful");
        router.replace({ name: "me" });
      } catch (error) {
        notifyError(error.message);
      }
    };

    return {
      form,
      handleLogin,
    };
  },
});
</script>
