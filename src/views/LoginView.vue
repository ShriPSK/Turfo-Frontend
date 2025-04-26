<template>
    <div class="login-container">
        <div class="sub-text">
            <span class="title">Sign In to TURFO</span>
            <span class="gray">Enter your email to receive a verification code</span>
        </div>
        <v-form v-model="isEmailFormValid" @submit.prevent="sendOtp" class="form-container"
            v-if="!isContinueBtnClicked">
            <div class="gray">Email Address</div>
            <v-text-field v-model="email" :rules="emailRules" hide-details variant="outlined" density="comfortable"
                placeholder="your.email@example.com" prepend-inner-icon="mdi-email-outline"
                class="email-input"></v-text-field>
            <button class="btn outlined" :disabled="!isEmailFormValid || sendBtnLoader">
                <div v-if="sendBtnLoader" class="btn-text">
                    <v-progress-circular v-if="sendBtnLoader" indeterminate size="16" width="3"></v-progress-circular>
                    <span>Sending...</span>
                </div>
                <div v-else class="btn-text">
                    <span>Continue with Email</span>
                    <v-icon color="black" size="small">mdi-arrow-right</v-icon>
                </div>
            </button>
            <button class="back-home-btn" @click="goHome">
                <v-icon size="small">mdi-arrow-left</v-icon>
                <span>Back to Home</span>
            </button>
        </v-form>
        <v-form @submit.prevent="verifyOtp" class="form-container" v-if="isContinueBtnClicked">
            <div class="gray">Verification Code</div>
            <v-otp-input v-model="otp" :length="6" class="otp-input"></v-otp-input>
            <button class="btn outlined" :disabled="!otp || otp.length < 6 || verifyBtnLoader">
                <div class="btn-text" v-if="verifyBtnLoader">
                    <v-progress-circular v-if="verifyBtnLoader" indeterminate size="16" width="3"></v-progress-circular>
                    <span>Verifying...</span>
                </div>
                <div v-else class="btn-text">
                    <span>Verify Code</span>
                    <v-icon color="black" size="small">mdi-arrow-right</v-icon>
                </div>
            </button>
        </v-form>
    </div>
</template>

<script>
export default {
    data() {
        return {
            email: null,
            emailRules: [
                v => !!v || 'Email is required',
                v => /.+@.+\..+/.test(v) || 'Email must be valid',
            ],
            isEmailFormValid: false,
            isContinueBtnClicked: false,
            isOtpFormValid: false,
            otp: null,
            sendBtnLoader: false,
            verifyBtnLoader: false,
        }
    },
    methods: {
        sendOtp() {
            this.sendBtnLoader = true;
            setTimeout(() => {
                this.sendBtnLoader = false;
                this.isContinueBtnClicked = true;
            }, 1000)
        },
        verifyOtp() {
            this.verifyBtnLoader = true;
            setTimeout(() => {
                this.verifyBtnLoader = false;
            }, 1000)
        },
        goHome() {
            this.$router.push({ name: 'HomeView' })
        }
    }
}
</script>

<style scoped>
.login-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    height: 90vh;
    gap: 48px;
}

.title {
    font-size: 32px;
    font-weight: 800;
}

.sub-text {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 8px;
}

.form-container {
    display: flex;
    flex-direction: column;
    gap: 32px;
    width: 100%;
    max-width: 400px;
    background: #1a1a1a;
    padding: 24px;
    border-radius: 16px;
}

.back-home-btn {
    display: flex;
    align-items: center;
    justify-content: flex-start;
    gap: 16px;
    padding: 8px 16px;
    border-radius: 12px;
    width: fit-content;
}

.back-home-btn:hover {
    background-color: #313131;
}

.email-input {
    height: auto;
}

.email-input :deep(.v-field--focused .v-field__overlay) {
    border: 1px solid var(--primary-color) !important;
}

.btn-text {
    display: flex;
    align-items: center;
    gap: 8px;
}
</style>
