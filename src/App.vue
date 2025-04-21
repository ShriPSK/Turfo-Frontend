<template>
    <v-app>
        <v-main>
            <router-view />
        </v-main>
    </v-app>
</template>

<script>

export default {
    name: 'App',

    data: () => ({
        //
    }),
}
</script>

<style>
:root {
    --primary-color: #1DB954;
    --bg-color: #1DB954;
    --text-color: #FFFFFF;
}

@font-face {
    font-family: 'Helvetica';
    src: url('@/assets/fonts/Helvetica.ttf') format('truetype');
}

/* @font-face {
    font-family: 'Recoleta';
    src: url('@/assets/fonts/Recoleta Regular.otf') format('opentype');
} */

html {
    overflow-y: auto !important;
    transition: scroll-behavior 0.5s;
    scroll-behavior: smooth;
}

#app {
    width: 100vw;
    height: 100vh;
    font-family: 'Nunito', sans-serif;
}

.global-table {
    overflow: auto;
    height: 100%;
}

.white {
    color: white;
}

.green {
    color: var(--primary-color);
}

.gray {
    color: #B3B3B3;
}
</style>
