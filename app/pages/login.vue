<script setup>
import Register from '~/components/auth/Register.vue';
import Login from '~/components/auth/Login.vue';
import Google from '~/components/auth/Google.vue';

definePageMeta({
    layout: 'default'
})

const { loggedIn } = useUserSession()

// To show registration component and viceversa
const change = ref(false);

onMounted(async () => {
    if (loggedIn.value) { await navigateTo('/dashboard') }
});

watch(loggedIn, (isIdle) => {
    if (isIdle) {
        if (loggedIn.value) {
            return navigateTo('/dashboard')
        }
    }
});

</script>

<template>
    <div v-if="!loggedIn">
        <div class="flex-1 flex items-center justify-center">
            <div class="flex flex-col items-center justify-center gap-8">
                <!-- Show Login or Register -->
                <component :is="change ? Register : Login" />
                <p class="text-center">
                    <span v-if="change">
                        Already have an account?
                        <ULink @click="change = false" active-class="text-primary" inactive-class="text-primary">
                            Login
                        </ULink>
                        <!-- Google Login -->
                        <Google />
                    </span>
                    <span v-else>
                        Don't have an account?
                        <ULink @click="change = true" active-class="text-primary" inactive-class="text-primary  ">
                            Sign Up
                        </ULink>
                        <!-- Google Login -->
                        <Google />
                    </span>
                </p>
            </div>
        </div>
    </div>
</template>
