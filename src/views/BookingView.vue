<template>
    <div class="booking-container">
        <div class="booking-header">
            <button class="back-btn" @click="goBack">
                <v-icon color="white">mdi-arrow-left</v-icon>
            </button>
            <span class="title">{{ pageTitle }}</span>
        </div>
        <Stepper :stepperNumber="currentStep" :totalSteps="totalSteps" />
        <div class="booking-body-wrapper">
            <div class="booking-body" v-if="currentStep === 1">
                <div class="select-date-container">
                    <div class="date-menu-title">
                        <v-menu v-model="dateMenu" offset-y transition="scale-transition"
                            :close-on-content-click="false">
                            <template #activator="{ props }">
                                <button v-bind="props">
                                    <v-icon color="var(--bg-color)">mdi-calendar</v-icon>
                                </button>
                            </template>
                            <v-date-picker v-model="datePickerValue" @update:modelValue="updateDate"
                                color="var(--primary-color)"></v-date-picker>
                        </v-menu>
                        <span class="title">Select Date</span>
                    </div>
                    <div class="dateList-container" :key="refresh">
                        <div v-for="(date, index) in dateList" :key="index" class="date-item"
                            :class="{ 'active': selectedDate.dateValue === date.dateValue }"
                            @click="selectedDate = date">
                            <div class="day">{{ date.day }}</div>
                            <div class="date">{{ date.date }}</div>
                            <div class="month">{{ date.month }}</div>
                        </div>
                    </div>
                </div>
                <div class="select-time-container">
                    <div class="title">Select One or More Time Slots</div>
                    <div class="time-duration-container">
                        <button v-for="(time, index) in timeList" :key="index" class="time-grid" @click="addSlots(time)"
                            :class="{
                                'active': selectedSlots.includes(time),
                                'blocked': blockedTimes.includes(time.slotStartTime)
                            }" :disabled="blockedTimes.includes(time.slotStartTime)">
                            {{ time.title }}
                        </button>
                    </div>
                </div>
                <div class="booking-summary-container" v-if="selectedDate && selectedSlots.length > 0">
                    <div class="title">Booking Summary</div>
                    <div class="summary-item">
                        <div class="summary-label">Date: </div>
                        <div class="summary-value">{{ selectedDate.dateValue }}</div>
                    </div>
                    <div class="summary-item">
                        <div class="summary-label">Slot: </div>
                        <div class="summary-value">{{ slotDuration }}</div>
                    </div>
                    <div class="summary-item">
                        <div class="summary-label">Price: </div>
                        <div class="summary-value">{{ '₹ ' + finalAmount }}</div>
                    </div>
                </div>
                <div class="continue-btn-container">
                    <button class="btn contained" :disabled="!selectedDate || selectedSlots.length === 0"
                        @click="goToNextStep">Continue
                        to Player Details</button>
                </div>
            </div>
            <div class="player-details-container" v-if="currentStep === 2">
                <div class="title">
                    <v-icon color="var(--bg-color)">mdi-account-multiple-outline</v-icon>
                    Player Details
                </div>
                <div class="player-count-input-container">
                    <div class="summary-label">Number of Players</div>
                    <div class="count-input">
                        <button class="update-count-btn" :disabled="playerCount === 1"
                            @click="updatePlayerCount(-1)"><v-icon>mdi-chevron-left</v-icon></button>
                        <div class="count-value">{{ playerCount }}</div>
                        <button class="update-count-btn" :disabled="playerCount === 14"
                            @click="updatePlayerCount(1)"><v-icon>mdi-chevron-right</v-icon></button>
                    </div>
                </div>
                <div class="player-booking-summary-container" v-if="selectedDate && selectedSlots.length > 0">
                    <div class="title">Booking Summary</div>
                    <div class="summary-item">
                        <div class="summary-label">Date: </div>
                        <div class="summary-value">{{ selectedDate.dateValue }}</div>
                    </div>
                    <div class="summary-item">
                        <div class="summary-label">Slot: </div>
                        <div class="summary-value">{{ slotDuration }}</div>
                    </div>
                    <div class="summary-item">
                        <div class="summary-label">Price: </div>
                        <div class="summary-value">{{ '₹ ' + finalAmount }}</div>
                    </div>
                    <v-divider></v-divider>
                    <div class="price-item">
                        <div class="summary-label">Turf Charge:</div>
                        <div class="white bold">{{ '₹ ' + finalAmount }}</div>
                    </div>
                    <div class="price-item">
                        <div class="summary-label">Platform Fee:</div>
                        <div class="platform-fee gray">
                            <span class="strike-out">₹ 20</span>
                            <span class="green">FREE</span>
                        </div>
                    </div>
                    <div class="price-item">
                        <div class="summary-label">Total:</div>
                        <div class="green bold">{{ '₹ ' + finalAmount }}</div>
                    </div>
                </div>
                <div class="continue-payment-btn-container">
                    <button class="btn contained" @click="goToNextStep">Continue to Payment</button>
                </div>
            </div>
            <div class="payment-details-container" v-if="currentStep === 3">
                <div class="title">Payment Options</div>
                <v-radio-group v-model="paymentOption" class="radio-group">
                    <v-radio color="var(--bg-color)" value="card" class="radio-item"
                        :class="{ active: paymentOption === 'creditcard' }">
                        <template #label>
                            <div>
                                <div class="radio-label">Credit/Debit Card</div>
                                <span class="gray">Pay securely with your card</span>
                            </div>
                        </template>
                    </v-radio>
                    <v-radio color="var(--bg-color)" label="UPI" value="upi" class="radio-item"
                        :class="{ active: paymentOption === 'upi' }">
                        <template #label>
                            <div>
                                <div class="radio-label">UPI Payment</div>
                                <span class="gray">Pay using Google Pay, PhonePe, etc.</span>
                            </div>
                        </template>
                    </v-radio>
                </v-radio-group>

                <div class="payment-details-container" v-if="paymentOption === 'card'">
                    <div class="input-field">
                        <span class="gray">Card Number</span>
                        <v-text-field v-model="cardDetails.cardNumber" placeholder="1234 5678 9012 3456"
                            variant="contained" density="comfortable" bg-color="#0D0D0D"></v-text-field>
                    </div>
                    <div class="input-row">
                        <div class="input-field">
                            <span class="gray">Expiry Date</span>
                            <v-text-field v-model="cardExpiry" placeholder="MM/YY" variant="contained"
                                density="comfortable" bg-color="#0D0D0D" @input="updateCardExpiry()"></v-text-field>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import moment from 'moment-timezone';

import Stepper from '../components/Stepper.vue';
import clock from '../assets/icons/clock.svg';

export default {
    components: {
        Stepper
    },
    data() {
        return {
            clock,
            pageTitle: null,
            currentStep: 1,
            totalSteps: 3,
            dateList: [],
            selectedDate: {},
            selectedSlots: [],
            slotDuration: null,
            baseAmount: 200,
            finalAmount: null,
            timeList: [],
            blockedTimes: [14, 15, 16],
            dateMenu: false,
            datePickerValue: null,
            refresh: 0,
            playerCount: 1,
            paymentOption: 'card',
            cardExpiry: null,
            cardDetails: {
                cardNumber: null,
                expiryDate: null,
                cvv: null,
                cardName: null
            },
            firstSelectedSlot: null,
        }
    },
    watch: {
        selectedDate() {
            if (this.selectedDate) {
                this.selectedDate.dateValue = moment(`${this.selectedDate.date} ${this.selectedDate.month} ${moment().year()}`, 'DD MMM YYYY').format('dddd, MMMM D, YYYY');
                this.datePickerValue = new Date(this.selectedDate.dateValue);
                this.refresh += 1;
            }
        },

        'cardDetails.cardNumber'() {
            let realNumber = this.cardDetails.cardNumber.replace(/-/gi, '')
            let dashedNumber = realNumber.match(/.{1,4}/g)
            this.cardDetails.cardNumber = dashedNumber.join('-')
        },
    },
    methods: {
        hourToString(hour) {
            return moment(hour, 'HH').format('h:mm A');
        },
        updatePageTitle() {
            if (this.currentStep === 1) {
                this.pageTitle = 'Select Date & Time';
            } else if (this.currentStep === 2) {
                this.pageTitle = 'Booking Summary';
            } else {
                this.pageTitle = 'Payment';
            }
        },
        goBack() {
            if (this.currentStep > 1) {
                this.currentStep -= 1;
                this.updatePageTitle();
            } else {
                this.$router.push({ name: 'HomeView' });
            }
        },

        updatePlayerCount(value) {
            this.playerCount += value;
        },
        updateDate(date) {
            this.selectedDate = {
                date: moment(date).date(),
                day: moment(date).format('ddd'),
                month: moment(date).format('MMM'),
                dateValue: moment(date).format('dddd, MMMM D, YYYY')
            };
            if(!this.dateList.includes(this.selectedDate)) this.dateList[0] = this.selectedDate;

            this.refresh += 1;
            this.dateMenu = false;
        },
        updateCardExpiry() {
            let expiryDate = this.cardExpiry.replace(/[^0-9]/g, '');
            if (expiryDate && (expiryDate.length > 4)) {
                this.cardDetails.expiryDate = expiryDate.slice(0, 4);
                this.cardExpiry = expiryDate.slice(0, 4);
                return;
            }

            if (expiryDate && expiryDate[0] > 1) {
                expiryDate = `0${expiryDate}`
            }
            let realNumber = expiryDate.replace(/-/gi, '')
            let dashedNumber = realNumber.match(/.{1,2}/g)

            this.cardDetails.expiryDate = dashedNumber.join('/')
            this.cardExpiry = dashedNumber.join('/');

        },
        goToNextStep() {
            this.currentStep += 1;
            if (this.currentStep === 3) {
                this.paymentOption = 'card';
            }
            this.updatePageTitle();
            window.scrollTo(0, 0);
        },
        addSlots(time) {
            if (this.selectedSlots.length === 0 || this.selectedSlots.length > 1) {
                this.selectedSlots = [time];
                this.finalAmount = this.baseAmount;
                this.slotDuration = this.hourToString(time.slotStartTime) + ' - ' + this.hourToString(time.slotEndTime);
            } else {
                let slotStartTime = Math.min(this.selectedSlots[0].slotStartTime, time.slotStartTime);
                let slotEndTime = Math.max(this.selectedSlots[this.selectedSlots.length - 1].slotEndTime, time.slotEndTime);
                this.selectedSlots = this.timeList.filter(time => time.slotStartTime >= slotStartTime && time.slotEndTime <= slotEndTime && !this.blockedTimes.includes(time.slotStartTime));
                this.finalAmount = this.baseAmount * this.selectedSlots.length;
                this.slotDuration = this.hourToString(slotStartTime) + ' - ' + this.hourToString(slotEndTime);
            }

        }
    },
    mounted() {
        this.updatePageTitle();

        // Get next 13 days
        for (let i = 0; i < 13; i++) {
            let day = moment().add(i, 'days');
            this.dateList.push({
                date: day.date(),
                day: day.format('ddd'),
                month: day.format('MMM'),
                dateValue: day.format('dddd, MMMM D, YYYY')
            });
        }
        this.selectedDate = this.dateList[0];

        // Generate time slots
        for (let i = 5; i <= 23; i++) {
            let startHour = i > 12 ? i - 12 : i;
            let startAmpm = i >= 12 ? 'PM' : 'AM';

            let nextHour = (i + 1) > 23 ? 0 : (i + 1); // 23 + 1 = 0 (Midnight)
            let endHour = nextHour === 0 ? 12 : (nextHour > 12 ? nextHour - 12 : nextHour);
            let endAmpm = nextHour >= 12 ? 'PM' : 'AM';

            this.timeList.push({
                title: `${startHour}:00 ${startAmpm} - ${endHour}:00 ${endAmpm}`,
                slotStartTime: i,
                slotEndTime: nextHour
            });
        }

    }
}
</script>

<style scoped>
.booking-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 24px;
    gap: 16px;

    @media (max-width: 900px) {
        padding: 24px;
        gap: 24px;
    }
}

.booking-header {
    align-self: flex-start;
}

.booking-header,
.date-menu-title {
    display: flex;
    gap: 24px;
}

.booking-header .title {
    font-size: 30px;
    font-weight: 800;
}

.booking-body-wrapper {
    display: flex;
    justify-content: center;
    width: 100%;
    max-width: 1200px;
}

.booking-body {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 30px;
    overflow: hidden;
}

.select-date-container,
.select-time-container {
    display: flex;
    flex-direction: column;
    gap: 16px;
    width: 100%;
}

.dateList-container {
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 12px;
    width: 100%;

    @media (max-width: 900px) {
        overflow: auto;
    }
}

.date-item {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    padding: 8px 12px;
    border-radius: 12px;
    background-color: #121212;
    color: white;
    cursor: pointer;
    font-weight: 600;
    transition: background-color 0.3s ease;
}

.date-item .date {
    font-size: 20px;
    font-weight: 800;
}

.date-item .day,
.date-item .month {
    font-size: 14px;
}

.date-item.active {
    background-color: var(--primary-color);
}

.date-item:hover:not(.active) {
    background-color: rgb(55 65 81 / 0.6);
}

.align-center {
    display: flex;
    align-items: center;
    justify-content: center;
}

.input {
    display: flex;
    align-items: center;
    gap: 32px;
    width: 100%
}

.value {
    display: flex;
    align-items: center;
    justify-content: center;
    white-space: nowrap;
    gap: 24px;
}

.label,
.value {
    flex: 1;
}

.booking-summary-container {
    display: flex;
    flex-direction: column;
    gap: 12px;
    background: #1A1A1A;
    padding: 24px;
    border-radius: 12px;
    width: 100%;
}

.summary-item {
    display: flex;
    gap: 16px;
}

.summary-label {
    color: #b3b3b3;
}

.platform-fee {
    display: flex;
    gap: 12px;
}

.strike-out {
    text-decoration: line-through;
}

.booking-body .title,
.player-details-container .title {
    font-size: 24px;
    font-weight: 800;
}

.time-duration-container {
    display: grid;
    grid-template-rows: repeat(5, 1fr);
    grid-template-columns: repeat(4, 1fr);
    gap: 16px;
    width: 100%;

    @media ((max-width: 900px) and (min-width: 600px)) {
        grid-template-columns: repeat(3, 1fr);
    }

    @media (max-width: 600px) {
        grid-template-columns: repeat(2, 1fr);
    }
}

.time-grid {
    background: #1A1A1A;
    border-radius: 12px;
    padding: 16px 60px;
    color: white;
    font-weight: 600;
    cursor: pointer;
    transition: background-color 0.3s ease;

    @media (max-width: 900px) {
        padding: 16px 20px;
    }
}

.time-grid:hover,
.time-grid.blocked {
    background-color: rgb(55 65 81 / 0.6);
}

.time-grid.blocked {
    cursor: not-allowed;
}

.time-grid.active {
    background-color: var(--primary-color);
}

.continue-btn-container {
    display: flex;
    justify-content: flex-end;
    width: 100%;
}

.player-details-container,
.payment-details-container {
    display: flex;
    flex-direction: column;
    align-self: center;
    gap: 16px;
    background: #1A1A1A;
    padding: 24px;
    border-radius: 12px;
    width: 100%;
}

.player-count-input-container {
    display: flex;
    flex-direction: column;
    gap: 12px;
}

.count-input {
    display: flex;
    width: max-content;
    background-color: #121212;
    border-radius: 12px;
}

.count-value {
    display: flex;
    align-items: center;
    justify-content: center;
    width: 40px;
}

.update-count-btn {
    padding: 12px;
}

.update-count-btn:hover {
    background: var(--primary-color);
    color: black;
}

.update-count-btn:first-child {
    border-top-left-radius: 12px;
    border-bottom-left-radius: 12px;
}

.update-count-btn:last-child {
    border-top-right-radius: 12px;
    border-bottom-right-radius: 12px;
}

.player-booking-summary-container {
    display: flex;
    flex-direction: column;
    gap: 12px;
    background: #121212;
    padding: 16px;
    border-radius: 12px;
    width: 100%;

    @media (max-width: 900px) {
        width: 100%;
    }
}

.price-item {
    display: flex;
    justify-content: space-between;
}

.continue-payment-btn-container {
    display: flex;
    justify-content: flex-end;
}

.radio-group :deep(.v-selection-control-group) {
    gap: 16px;
}

.radio-item {
    display: flex;
    align-items: flex-start;
    gap: 8px;
    padding: 16px;
    background-color: #121212;
    border: 1px solid #374151;
    border-radius: 12px;
    cursor: pointer;
    transition: border-color 0.3s ease;
}

.radio-item:hover {
    border-color: rgb(55, 65, 81 / 0.6);
}

.radio-item.active:hover {
    border-color: var(--primary-color);
}
</style>