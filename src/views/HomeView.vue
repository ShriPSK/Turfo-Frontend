<template>
    <div class="home-container">
        <div class="overlay"></div>
        <section class="hero-section-wrapper" id="home">
            <div class="hero-section">
                <div class="hero-header white">Premium Turf Experience</div>
                <div class="hero-header green">For Every Player</div>
                <div class="hero-header-sub gray">Book your next game on our professional-grade turf. State-of-the-art
                    facilities designed for the perfect sporting experience.</div>
                <div class="hero-btn-container">
                    <button class="btn contained" @click="goToBooking">Book Now</button>
                    <!-- <button class="btn contained"><a href="#turfDetails">Explore Turf</a></button> -->
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
                        <button class="glide__arrow glide__arrow--left" data-glide-dir="<"><v-icon color="white">mdi-chevron-left</v-icon></button>
                        <button class="glide__arrow glide__arrow--right" data-glide-dir=">"><v-icon color="white">mdi-chevron-right</v-icon></button>
                    </div>
                    <div class="glide__bullets" data-glide-el="controls[nav]">
                        <button class="glide__bullet" data-glide-dir="=0"></button>
                        <button class="glide__bullet" data-glide-dir="=1"></button>
                        <button class="glide__bullet" data-glide-dir="=2"></button>
                    </div>

                </div>
                <div class="facility-right">
                    <div class="facilities-container">
                        <div class="facility web" v-for="(facility, index) in facilities" :key="index">
                            <Svg :src="facility.icon" stroke="var(--primary-color)"></Svg>
                            <div class="facility-details">
                                <div class="facility-title">{{ facility.title }}</div>
                                <div class="facility-subtitle gray">{{ facility.subtitle }}</div>
                            </div>
                        </div>

                        <div class="facility mobile" v-for="(facility, index) in facilities" :key="index">
                            <div class="facility-row">
                                <Svg :src="facility.icon" stroke="var(--primary-color)"></Svg>
                                <div class="facility-title">{{ facility.title }}</div>
                            </div>
                            <div class="facility-subtitle gray">{{ facility.subtitle }}</div>
                        </div>
                    </div>
                    <div class="check-btn-container">
                        <button class="btn contained" @click="goToBooking">
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

import clock from '../assets/icons/clock.svg'
import bookings from '../assets/icons/bookings.svg'
import capacity from '../assets/icons/capacity.svg'
import facilities from '../assets/icons/facilities.svg'
import field_size from '../assets/icons/field_size.svg'
import maintenance from '../assets/icons/maintenance.svg'
import quality from '../assets/icons/quality.svg'
import sports from '../assets/icons/sports.svg'
import turf from '../assets/icons/turf.svg'

export default {
    data() {
        return {
            clock, bookings, capacity, facilities, field_size, maintenance, quality, sports, turf,
            facilities: []
        }
    },
    watch: {
        $route(to, from) {
            if (to.hash) {
                const element = document.querySelector(to.hash)
                if (element) {
                    element.scrollIntoView({ behavior: 'smooth' })
                }
            }
        },
    },
    methods: {
        goToBooking() {
            this.$router.push({ name: 'BookingView' })
        },
    },
    mounted() {
        const glide = new Glide('.glide', {
            type: 'carousel',
            perView: 1,
            focusAt: 'center',
            gap: 10,
        }).mount()

        this.facilities = [
            {
                title: "Operating Hours",
                subtitle: "Open daily from 6 AM to 11 PM",
                icon: clock
            },
            {
                title: "Capacity",
                subtitle: "Up to 14 players (7v7 format)",
                icon: capacity
            },
            {
                title: "Field Size",
                subtitle: "60m x 40m FIFA approved dimensions",
                icon: field_size
            },
            {
                title: "Sports",
                subtitle: "Football, Rugby, Cricket",
                icon: sports
            },
            {
                title: "Quality",
                subtitle: "Professional-grade turf surface",
                icon: quality
            },
            {
                title: "Facilities",
                subtitle: "Changing rooms, Lighting, Parking",
                icon: facilities
            },
            {
                title: "Turf Type",
                subtitle: "Grass Turf",
                icon: maintenance
            },
            {
                title: "Bookings",
                subtitle: "1-hour slots, 24h advance booking",
                icon: bookings
            }
        ]
    },
}

</script>

<style scoped>

.hero-section-wrapper {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    text-align: center;
    height: 100vh;
    /* background-color: rgba(0, 0, 0, 0.3); */
    /* background: url('../assets/images/hero_section_bg.jpg') ; */
}

.overlay {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100vh;
    z-index: -1;
    background: url('../assets/images/hero_section_bg.jpg');
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

.btn a {
    text-decoration: none;
    color: inherit;
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

    @media (max-width: 900px) {
        flex-direction: column;
        align-items: flex-start;
    }
}

.facility-row {
    display: flex;
    gap: 16px;
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