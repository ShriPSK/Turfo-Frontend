<template>
    <div class="html-wrapper">
        <div class="sidebar-container">
            <div class="sidebar-top">
                <img :src="Logo" class="logo" />
                <div class="nav-menu">
                    <div class="sub-nav-container">
                        <router-link class="nav" :to="{ name: 'TurfList' }">
                            <Svg :src="TurfIcon" fill="#FFFFFF" width="20" height="20"></Svg>
                            <span>Turf Management</span>
                        </router-link>
                    </div>
                </div>
            </div>
            <!-- <div class="sidebar-bottom">Logout</div> -->
        </div>
        <div class="right-container">
            <router-view />
        </div>
    </div>
</template>

<script>
import Logo from '@/assets/icons/logo.svg'
import TurfIcon from '@/assets/icons/turf.svg'
export default {
    props: ['adminLevel'],
    data() {
        return {
            Logo, TurfIcon
        }
    },
    computed: {
        currentPath() {
            return this.$route.path
        },
    },
    methods: {
        logout() {
            this.$router.push({ name: 'Login' })
        },
    }
}
</script>
<style scoped>
.html-wrapper {
    display: flex;
    width: 100%;
    height: 100%;
}

.sidebar-container {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    padding: 30px 20px;
    width: 300px;
    height: 100%;
    background: #212121;
    color: #FFFFFF;
}

.sidebar-top {
    display: flex;
    flex-direction: column;
    gap: 20px;
}

.logo {
    width: 100px;
    border-radius: 10px;
}

.nav {
    all: unset;
    display: flex;
    align-items: center;
    gap: 0.5em;
    padding: 12px 16px;
    cursor: pointer;
    white-space: nowrap;
    transition: background 0.7s ease;
}

.nav.router-link-exact-active {
    background: var(--bg-color);
    border-radius: 12px;
    font-weight: 600;
}

.nav:hover {
    background: var(--bg-color);
}

.right-container {
    background: var(--bg-color);
    width: 100%;
    height: 100%;
}
</style>