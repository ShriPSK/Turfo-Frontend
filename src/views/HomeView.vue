<template>
    <v-navigation-drawer location="right" v-model="showNavOverlay" @close="toggleNavOverlay" bg-color="black" temporary>
        <div class="drawer-container">check</div>
    </v-navigation-drawer>

    <div class="page-container">
        <div class="nav-header">
            <div class="logo">
                <span class="white">TURF</span>
                <span class="green">O</span>
            </div>
            <div class="nav-right web">
                <a href="#home" class="nav-item">Home</a>
                <a href="#turfDetails" class="nav-item">Turf Details</a>
                <a href="#booking" class="nav-item">Booking</a>
                <a href="#testimonials" class="nav-item">Testimonials</a>
                <a href="#contact" class="nav-item">Contact</a>
                <a href="#login" class="nav-item">Login</a>
            </div>
            <button class="mobile nav-toggle-icon" @click="toggleNavOverlay"><v-icon>{{ showNavOverlay ? 'mdi-close' :
                'mdi-menu' }}</v-icon></button>
        </div>
        <div class="overlay"></div>
        <section class="hero-section-wrapper" id="home">
            <div class="hero-section">
                <div class="hero-header white">Premium Turf Experience</div>
                <div class="hero-header green">For Every Player</div>
                <div class="hero-header-sub gray">Book your next game on our professional-grade turf. State-of-the-art
                    facilities designed for the perfect sporting experience.</div>
                <div class="hero-btn-container">
                    <button class="btn outlined">Book Now</button>
                    <button class="btn contained"><a href="#turfDetails">Explore Turf</a></button>
                </div>
            </div>
        </section>
        <section class="premium-turf-container" id="turfDetails">
            <div class="premium-turf-header">
                <div class="title">
                    <span class="white">Our </span>
                    <span class="green">Premium Turf</span>
                </div>
                <div class="subtitle gray">
                    Experience top-quality sports surface designed for performance and safety. Perfect for casual games
                    and
                    competitive matches alike.
                </div>
            </div>
            <div class="turf-details-container">
                <div class="glide">
                    <div class="glide__track" data-glide-el="track">
                        <ul class="glide__slides">
                            <li class="glide__slide"><img src="../assets/images/slider1.jpeg" alt="turf"
                                    class="glider-img"></li>
                            <li class="glide__slide"><img src="../assets/images/slider2.jpeg" alt="turf"
                                    class="glider-img"></li>
                            <li class="glide__slide"><img src="../assets/images/slider3.jpeg" alt="turf"
                                    class="glider-img"></li>
                        </ul>
                    </div>

                    <div class="glide__arrows" data-glide-el="controls">
                        <button class="glide__arrow glide__arrow--left" data-glide-dir="<"><Svg :src="ChevronLeft"
                                stroke="white"></Svg></button>
                        <button class="glide__arrow glide__arrow--right" data-glide-dir=">"><Svg :src="ChevronRight"
                                stroke="white"></Svg></button>
                    </div>
                    <div class="glide__bullets" data-glide-el="controls[nav]">
                        <button class="glide__bullet" data-glide-dir="=0"></button>
                        <button class="glide__bullet" data-glide-dir="=1"></button>
                        <button class="glide__bullet" data-glide-dir="=2"></button>
                    </div>

                </div>
                <div class="facility-right">
                    <div class="facilities-container">
                        <div class="facility">
                            <Svg :src="clock" stroke="var(--primary-color)"></Svg>
                            <div class="facility-details">
                                <div class="facility-title">Operating Hours</div>
                                <div class="facility-subtitle gray">Open daily from 6 AM to 11 PM</div>
                            </div>
                        </div>
                        <div class="facility">
                            <Svg :src="capacity" stroke="var(--primary-color)"></Svg>
                            <div class="facility-details">
                                <div class="facility-title">Capacity</div>
                                <div class="facility-subtitle gray">Up to 14 players (7v7 format)</div>
                            </div>
                        </div>
                        <div class="facility">
                            <Svg :src="field_size" stroke="var(--primary-color)"></Svg>
                            <div class="facility-details">
                                <div class="facility-title">Field Size</div>
                                <div class="facility-subtitle gray">60m x 40m FIFA approved dimensions</div>
                            </div>
                        </div>
                        <div class="facility">
                            <Svg :src="sports" stroke="var(--primary-color)"></Svg>
                            <div class="facility-details">
                                <div class="facility-title">Sports</div>
                                <div class="facility-subtitle gray">Football, Rugby, Cricket</div>
                            </div>
                        </div>
                        <div class="facility">
                            <Svg :src="quality" stroke="var(--primary-color)"></Svg>
                            <div class="facility-details">
                                <div class="facility-title">Quality</div>
                                <div class="facility-subtitle gray">Professional-grade turf surface</div>
                            </div>
                        </div>
                        <div class="facility">
                            <Svg :src="facilities" stroke="var(--primary-color)"></Svg>
                            <div class="facility-details">
                                <div class="facility-title">Facilities</div>
                                <div class="facility-subtitle gray">Changing rooms, Lighting, Parking</div>
                            </div>
                        </div>
                        <div class="facility">
                            <Svg :src="maintenance" stroke="var(--primary-color)"></Svg>
                            <div class="facility-details">
                                <div class="facility-title">Maintenance</div>
                                <div class="facility-subtitle gray">Daily cleaning and weekly deep maintenance</div>
                            </div>
                        </div>
                        <div class="facility">
                            <Svg :src="bookings" stroke="var(--primary-color)"></Svg>
                            <div class="facility-details">
                                <div class="facility-title">Bookings</div>
                                <div class="facility-subtitle gray">1-hour slots, 24h advance booking</div>
                            </div>
                        </div>
                    </div>
                    <div class="check-btn-container">
                        <button class="btn outlined">
                            <span>Check Availability</span>
                            <v-icon>mdi-calendar-blank</v-icon>
                        </button>
                    </div>
                </div>
            </div>
        </section>
    </div>
</template>

<script>
import Glide from '@glidejs/glide'
import ChevronRight from '@/assets/icons/chevron_right.svg'
import ChevronLeft from '@/assets/icons/chevron_left.svg'

import clock from '@/assets/icons/clock.svg'
import bookings from '@/assets/icons/bookings.svg'
import capacity from '@/assets/icons/capacity.svg'
import facilities from '@/assets/icons/facilities.svg'
import field_size from '@/assets/icons/field_size.svg'
import maintenance from '@/assets/icons/maintenance.svg'
import quality from '@/assets/icons/quality.svg'
import sports from '@/assets/icons/sports.svg'
import turf from '@/assets/icons/turf.svg'

export default {
    data() {
        return {
            ChevronLeft, ChevronRight, clock, bookings, capacity, facilities, field_size, maintenance, quality, sports, turf,
            showNavOverlay: false,
        }
    },
    mounted() {
        const glide = new Glide('.glide', {
            type: 'carousel',
            perView: 1,
            focusAt: 'center',
            gap: 10,
        }).mount()
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
    color: white;
    background: #121212;
}

.nav-header {
    position: fixed;
    top: 0;
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 16px 24px;
    width: 100%;
    z-index: 10000;
}

.logo {
    font-family: 'Hysteria', sans-serif;
    font-weight: 600;
    font-size: 24px;
}

.nav-right {
    display: flex;
    gap: 20px;
}

.nav-item {
    color: white;
    text-decoration: none;
    font-size: 16px;
    font-weight: 500;
}

.hero-section-wrapper {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    text-align: center;
    height: 100vh;
    background: rgba(0, 0, 0, 0.3);
}

.overlay {
    position: absolute;
    top: 0;
    height: 100vh;
    background: url('../assets/images/hero_section_bg.jpg') no-repeat center center;
}

.hero-section {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    width: 50%;

    @media (max-width: 900px) {
        width: 90%;
        gap: 16px;
    }
}

.hero-header {
    font-size: 60px;

    @media (max-width: 900px) {
        font-size: 40px;
    }
}

.hero-header-sub {
    font-size: 20px;
}

.hero-btn-container {
    display: flex;
    gap: 20px;
    margin-top: 20px;
}

.check-btn-container {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100%;
    padding: 20px 0;
}

.btn {
    display: flex;
    align-items: center;
    gap: 12px;
    padding: 12px 20px;
    cursor: pointer;
    border-radius: 10px;
    font-weight: 600;

    @media (max-width: 900px) {
        padding: 8px 24px;
    }
}

.btn a {
    text-decoration: none;
    color: inherit;
}

.btn.outlined {
    background: var(--primary-color);
    color: black;
    animation: pulse 3s ease-in-out infinite;
}

.btn.contained {
    color: var(--primary-color);
    border: 2px solid var(--primary-color);
    transition: background 0.5s ease, color 0.3s ease;
}

.btn.contained:hover {
    background: var(--primary-color);
    color: black;
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

.premium-turf-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    text-align: center;
    gap: 16px;

    @media (max-width: 900px) {
        padding: 24px;
    }
}

.premium-turf-header {
    transition: translate 0.5s ease;
}

.premium-turf-header .title {
    font-size: 48px;
    font-weight: 600;
}

.turf-details-container {
    display: flex;
    width: 100%;
    padding: 20px;
    gap: 20px;
}

.glide {
    width: 45%;
}

.glide__arrow {
    background: transparent;
    opacity: 1;
    color: black;
    border-radius: 50%;
}

.glide__bullet {
    width: 15px;
    height: 15px;
}

.glide__bullet--active {
    background: var(--primary-color);
}

.glide__slide {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100%;
    height: 100%;
}

.glider-img {
    width: 100%;
    height: 100%;
    border-radius: 20px;
    object-fit: contain;
}

.facility-right {
    width: 55%;
    display: flex;
    flex-direction: column;
    gap: 16px;
}

.facilities-container {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    grid-template-rows: repeat(4, auto);
    gap: 16px;
}

.facility {
    display: flex;
    align-items: center;
    gap: 16px;
    padding: 16px;
    background: #1A1A1A;
    border-radius: 20px;
}

.facility-details {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
}

.facility-title {
    font-weight: 600;
}

.facility-subtitle {
    text-align: left;
}

.nav-toggle-icon {
    z-index: 10000;
}

.drawer-container {
    background: #121212;
    height: 100vh;
}

.v-navigation-drawer--active {
    width: 100vw !important;
    border: none !important;
}

@media (max-width: 900px) {
    .web {
        display: none;
    }

    .turf-details-container {
        flex-direction: column;
    }

    .glide,
    .facility-right {
        width: 100%;
    }

    .glide {
        height: 100%;
    }
}

@media (min-width: 900px) {
    .mobile {
        display: none;
    }
}
</style>