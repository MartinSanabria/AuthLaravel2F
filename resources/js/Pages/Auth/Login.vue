<script setup>
import { Head, Link, useForm } from '@inertiajs/vue3';
import AuthenticationCard from '@/Components/AuthenticationCard.vue';
import AuthenticationCardLogo from '@/Components/AuthenticationCardLogo.vue';
import Checkbox from '@/Components/Checkbox.vue';
import InputError from '@/Components/InputError.vue';
import InputLabel from '@/Components/InputLabel.vue';
import PrimaryButton from '@/Components/PrimaryButton.vue';
import TextInput from '@/Components/TextInput.vue';

defineProps({
    canResetPassword: Boolean,
    status: String,
});

const form = useForm({
    email: '',
    password: '',
    remember: false,
});

const submit = () => {
    form.transform(data => ({
        ...data,
        remember: form.remember ? 'on' : '',
    })).post(route('login'), {
        onFinish: () => form.reset('password'),
    });
};
</script>

<style scoped>
.login-container {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  background-color: #f7f9fc;
}

.login-card {
  max-width: 400px;
  width: 100%;
  background-color: #ffffff;
  border-radius: 8px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  padding: 30px;
}

.login-form .form-group {
  margin-bottom: 20px;
}

.form-control {
  width: 100%;
  padding: 12px;
  border: 1px solid #e0e0e0;
  border-radius: 4px;
  background-color: #f5f5f5;
}

.link {
  color: #007bff;
  text-decoration: none;
}

.link:hover {
  text-decoration: underline;
}

</style>

<template>
    <AuthenticationCard>
      <form class="login-form" @submit.prevent="submit">
        <div class="form-group">
          <InputLabel for="email" value="Correo electrónico" />
          <TextInput
            id="email"
            v-model="form.email"
            type="email"
            class="form-control"
            required
            autofocus
            autocomplete="username"
          />
          <InputError :message="form.errors.email" />
        </div>
        <div class="form-group">
          <InputLabel for="password" value="Contraseña" />
          <TextInput
            id="password"
            v-model="form.password"
            type="password"
            class="form-control"
            required
            autocomplete="current-password"
          />
          <InputError :message="form.errors.password" />
        </div>
        <div class="form-group">
          <Checkbox v-model:checked="form.remember" name="remember" />
          <span class="text-sm text-gray-600">Recuérdame</span>
        </div>
        <div class="form-group">
          <PrimaryButton :class="{ 'opacity-25': form.processing }" :disabled="form.processing">
            Iniciar Sesión
          </PrimaryButton>
        </div>
        <div class="form-group">
          <Link v-if="canResetPassword" :href="route('password.request')" class="link">¿Olvidaste tu contraseña?</Link>
          <Link v-if="canResetPassword" :href="route('register')" class="link ms-4">Registrarse</Link>
        </div>
      </form>
    </AuthenticationCard>
  </template>