<template>
    <div class="login-wrapper">
        <!-- <div class="left-wrapper">
            <img :src="require('@/assets/images/login_image.png')" class="login-img"/>
        </div> -->
        <div class="right-wrapper">
            <div class="header-section">
                <img :src="Logo" alt="logo" />
                <p class="get-started">Get Started</p>
                <!-- <p class="description">Please enter your details to continue.</p> -->
            </div>
            <div class="body-section">
                <button style="width: 100%;display:flex;justify-content: center;">
                    <div ref="googleLoginBtn"></div>
                </button>
            </div>

            <div v-if="loginLoader" class="login-loader">
                <v-progress-circular :width="3" :size="30" indeterminate></v-progress-circular>
            </div>
        </div>

        <v-dialog v-model="unauthorisedErrorPopup" persistent width="400">
            <v-card>
                <v-card-title class="text-h5">
                    Ooops ! You are unauthorized !!
                </v-card-title>
                <v-card-text>You are unauthorized! Please login with authorized account</v-card-text>
                <v-card-actions>
                    <v-spacer></v-spacer>
                    <v-btn color="blue darken-1" text @click="unauthorisedErrorPopup = false">
                        Okay
                    </v-btn>
                </v-card-actions>
            </v-card>
        </v-dialog>
    </div>
</template>

<script>
import Cookies from 'js-cookie';
import CryptoJS from 'crypto-js';
import axios from 'axios';
import Logo from '@/assets/icons/logo.svg';

export default {
    name: 'LoginView',
    data() {
        return {
            Logo,
            clientId: import.meta.env.VITE_APP_CLIENT_ID,
            encryptionKey: import.meta.env.VITE_APP_ENCRYPTION_KEY,
            loginLoader: false,
            unauthorisedErrorPopup: false
        }
    },

    async mounted() {
        this.renderSignInButton()
    },

    methods: {
        async onGoogleAuthSuccess(jwtCredentials) {
            this.loginLoader = true;
            const profileData = JSON.parse(atob(jwtCredentials.credential.split('.')[1]
            ));

            var idToken = jwtCredentials.credential;
            const encryptedText = CryptoJS.AES.encrypt(idToken + " " + profileData.email, this.encryptionKey).toString()
            var createAdminTokenData = {
                key: encryptedText
            }
            this.$router.push({
                name: 'TurfList'
            })
            // await axios.post(this.url + "/admin/token", createAdminTokenData)
            //     .then(async (apiResponse) => {
            //         Cookies.set("access-token", apiResponse.data.token);
            //         this.$router.push({
            //             name: "upcomingBookings",
            //         });
            //     })
            //     .catch(err => {
            //         if (err.response?.status === 401) {
            //             return this.unauthorisedErrorPopup = true;
            //         }
            //         console.error("[LOGIN FAILED]", err.message);
            //     })
            //     .finally(() => {
            //         this.loginLoader = false;
            //     })
        },

        renderSignInButton() {
            window.google.accounts.id.initialize({
                client_id: this.clientId,
                callback: this.onGoogleAuthSuccess,
                prompt: 'select_by',
                scope: "email"
            })
            window.google.accounts.id.renderButton(
                this.$refs.googleLoginBtn, {
                text: 'signin',
                shape: 'square',
                size: 'large',
                width: 500,
                theme: 'white',
            }
            )

            window.google.accounts.id.prompt();
        },
    },
};

</script>

<style scoped>
.login-wrapper {
    display: flex;
    width: 100%;
    height: 100vh;
    overflow: hidden;
}

.left-wrapper {
    height: 100%;
    background-size: 100% 100%;
    flex: 2;
}

.login-img {
    width: 100%;
    height: 100%;
    object-fit: cover;
}

.right-wrapper {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    gap: 24px;
    height: 100%;
    flex: 3;
}

.header-section {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 8px;
}

.get-started {
    color: var(--Main-Color-black-500, #212332);
    font-size: 24px;
    font-style: normal;
    font-weight: 600;
    line-height: 150%;
}

.description {
    color: var(--Grey-Light-light-grey-400, #6F717E);
    font-size: 16px;
    font-style: normal;
    font-weight: 400;
    line-height: 174%;
}

.login-loader {
    display: flex;
    width: 100%;
    justify-content: center;
}
</style>
