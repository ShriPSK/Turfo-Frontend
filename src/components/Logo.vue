<template>
    <button class="logo" @click="goToHome">
        <span class="white">TURF</span>
        <span class="green">O</span>
    </button>
</template>

<script>
export default {
    methods: {
        goToHome() {
            this.$router.push({ name: 'HomeView' })
        }
    }
}
</script>
<style scoped>
.logo {
    font-family: 'Hysteria', sans-serif;
    font-weight: 600;
    font-size: 24px;
}
</style>