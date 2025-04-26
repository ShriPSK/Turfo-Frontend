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
    watch: {
        '$route'() {
            window.scrollTo(0, 0);
        }
    },
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
    src: url('./assets/fonts/Helvetica.ttf') format('truetype');
}

/* @font-face {
    font-family: 'Recoleta';
    src: url('./assets/fonts/Recoleta Regular.otf') format('opentype');
} */

html {
    overflow-y: auto !important;
    transition: scroll-behavior 0.5s;
    scroll-behavior: smooth;
}

a {
    text-decoration: none;
    color: inherit;
}

button {
    transition: background-color 0.5s ease, color 0.3s ease;
}

button:disabled {
    opacity: 0.5;
    cursor: not-allowed;
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

.bold {
    font-weight: 800;
}

.btn {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 12px;
    padding: 12px 20px;
    cursor: pointer;
    border-radius: 10px;
    font-weight: 800;

    @media (max-width: 900px) {
        padding: 8px 24px;
    }
}

.btn.outlined {
    background: var(--primary-color);
    color: black;
    animation: pulse 3s ease-in-out infinite;
}

.btn.contained {
    color: var(--primary-color);
    border: 2px solid var(--primary-color);
    transition: background-color 0.5s ease, color 0.3s ease;
}

.btn.contained:hover {
    background: var(--primary-color);
    color: black;
}

.btn:disabled {
    opacity: 0.5;
    cursor: not-allowed;
    animation: none;
}

@keyframes pulse {
    0% {
        opacity: 1;
    }

    70% {
        opacity: 0.7;
    }

    100% {
        opacity: 1;
    }
}

.v-picker-title {
    display: none;   
}

.v-date-picker-header {
    background: #212121 !important;
    border-bottom: 1px solid #121212 !important;
}
</style>
