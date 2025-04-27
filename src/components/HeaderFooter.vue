<template>

    <v-navigation-drawer location="right" v-model="showNavOverlay" @close="toggleNavOverlay" mobile bg-color="black"
        class="nav-drawer">
        <button class="close-icon" @click="toggleNavOverlay">
            <v-icon>mdi-close-circle-outline</v-icon>
        </button>
        <div class="drawer-container">
            <div class="drawer-header">
                <Logo />
            </div>
            <div class="drawer-details">
                <a href="/" class="nav-item">Explore Turf</a>
                <a href="#contact" class="nav-item">Contact</a>
            </div>
        </div>
    </v-navigation-drawer>

    <div class="page-container">
        <div class="nav-header" :class="{ 'nav-fixed': $route.name === 'HomeView' }">
            <Logo />
            <div class="nav-right web">
                <a href="/" class="nav-item">Explore Turf</a>
                <a href="#contact" class="nav-item">Contact</a>
                <a href="/login" class="btn contained">Login</a>
            </div>
            <div class="nav-right mobile">
                <button class="nav-toggle-icon" @click="toggleNavOverlay">
                    <v-icon>{{ showNavOverlay ? 'mdi-close' : 'mdi-menu' }}</v-icon>
                </button>
                <div class="btn contained" v-if="!showNavOverlay"><a href="/login">Login</a></div>
            </div>
        </div>
        <router-view />
        <section class="page-footer" id="contact">
            <div class="footer-top">
                <div class="footer-details">
                    <Logo />
                    <div class="desc">Premium turf booking platform providing high-quality sports surfaces for your
                        game.
                        Experience the difference with TURFO.</div>
                </div>
                <div class="footer-details">
                    <div class="title">Quick Links</div>
                    <router-link to="/" class="nav-item">Home</router-link>
                    <router-link to="/#turfDetails" class="nav-item">Turf Details</router-link>
                </div>
                <div class="footer-details">
                    <div class="title">Contact Us</div>
                    <div class="contact-item">
                        <Svg :src="Call" stroke="var(--primary-color)" class="call-icon"></Svg>
                        <a href="tel:1234567890" class="nav-item" target="_blank">+91 123 456 7890</a>
                    </div>
                    <div class="contact-item">
                        <Svg :src="Mail" stroke="var(--primary-color)" class="mail-icon"></Svg>
                        <a href="mailto:turfbooking@example.com" class="nav-item"
                            target="_blank">turfbooking@example.com</a>
                    </div>
                </div>
            </div>
            <div class="footer-bottom">
                <div class="copyright">© 2025 TURFO. All Rights Reserved.</div>
                <div class="copyright">Designed and maintained by <span class="green">TURFO Team</span></div>
            </div>
        </section>
    </div>
</template>

<script>
import Logo from './Logo.vue'
import Location from '../assets/icons/location.svg'
import Call from '../assets/icons/call.svg'
import Mail from '../assets/icons/mail.svg'

export default {
    components: {
        Logo,
    },
    data() {
        return {
            Location, Call, Mail,
            showNavOverlay: false,
        }
    },
    methods: {
        toggleNavOverlay() {
            this.showNavOverlay = !this.showNavOverlay;
        },
    },
}
</script>

<style scoped>
.page-container {
    display: flex;
    flex-direction: column;
    overflow: hidden;
    width: 100%;
    height: 100%;
    color: white;
    background: #121212;
}

.nav-header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 16px 24px;
    width: 100%;
    background: #121212;
}

.nav-fixed {
    position: fixed;
    top: 0;
}

.nav-right {
    display: flex;
    gap: 20px;
}

.nav-item {
    display: flex;
    align-items: center;
    color: white;
    text-decoration: none;
    font-size: 16px;
    font-weight: 500;
}

.page-footer {
    display: flex;
    flex-direction: column;
    padding: 24px;
    background: #1A1A1A;

    @media (max-width: 768px) {
        gap: 16px;
    }
}

.footer-top {
    display: flex;
    gap: 20px;

    @media (max-width: 768px) {
        flex-direction: column;
    }
}

.footer-details {
    display: flex;
    flex-direction: column;
    gap: 8px;
    flex: 1;
}

.footer-details .title {
    font-size: 20px;
    font-weight: 600;
}

.contact-item {
    display: flex;
    align-items: center;
    gap: 8px;
}

.desc,
.footer-details .nav-item,
.contact-item .nav-item {
    color: #B3B3B3;
}

.footer-bottom {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
}

.drawer-container, .drawer-details {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    gap: 16px;
    height: 100%;
    padding: 30px;
}

.close-icon {
    position: absolute;
    top: 30px;
    right: 30px;
}

@media (max-width: 768px) {
    .web {
        display: none;
    }
}

@media (min-width: 768px) {
    .mobile {
        display: none;
    }
}
</style>

<style>
.v-navigation-drawer--active {
    width: 100vw !important;
}
</style>