<script>
import axios from "axios";
import { useToast } from "vue-toastification";
import { baseAPI } from "../stores";
import SubmitButton from "../components/SubmitButton.vue";
import router from "../router";
import Loading from "../components/Loading.vue";

export default {
    data() {
        return {
            email: "",
            password: "",
            toast: useToast(),
            shouldValidate: false,
            loading: false,
        };
    },
    mounted() {
        document.title = "Log Masuk";
    },
    components: {
        SubmitButton,
        Loading,
    },
    methods: {
        validateInput(input) {
            if (input === "") {
                return "rgb(200 61 40)"; // Example: Set border color to red for empty input
            }
        },
        // check and send input to database to login
        async login() {
            this.shouldValidate = true;
            if (!this.email || !this.password) {
                this.toast.error("E-mel dan kata laluan diperlukan!", {
                    timeout: 3000,
                });
            } else {
                this.loading = true;
                axios
                    .post(baseAPI + "/api/login", {
                        email: this.email,
                        password: this.password,
                    })
                    .then(async (response) => {
                        // Handle successful login
                        sessionStorage.setItem(
                            "idUser",
                            JSON.stringify(response.data)
                        );
                        const responseUser = await axios.get(
                            baseAPI + `/api/user/${response.data}`
                        );

                        if (
                            responseUser.data.role === "Student" &&
                            responseUser.data.student.isRegistered === true
                        ) {
                            this.toast.success("Log Masuk Berjaya", {
                                timeout: 3000,
                            });
                            router.push("/pelajar/dashboard");
                            this.loading = false;
                        } else if (responseUser.data.role === "Teacher") {
                            this.toast.success("Log Masuk Berjaya", {
                                timeout: 3000,
                            });
                            router.push("/guru/dashboard");
                            this.loading = false;
                        } else if (responseUser.data.role === "Clerk") {
                            this.toast.success("Log Masuk Berjaya", {
                                timeout: 3000,
                            });
                            router.push("/kerani/dashboard");
                            this.loading = false;
                        } else if (
                            responseUser.data.student.isRegistered === false
                        ) {
                            this.toast.warning(
                                "Akaun anda masih belum mendapatkan pengesahan",
                                { timeout: 3000 }
                            );
                            this.loading = false;
                        }
                    })
                    .catch((error) => {
                        // Handle error
                        if (error.response && error.response.status === 401) {
                            const errorMessage = error.response.data.error;
                            this.toast.warning(errorMessage, { timeout: 3000 });
                        }
                        this.loading = false;
                    });
            }
        },
    },
};
</script>

<template>
    <div
        class="bg-slate-50 flex justify-center items-center h-screen overflow-hidden"
    >
        <!-- Center div -->
        <div
            class="m-auto bg-white rounded-2xl pt-6 pb-12 w-11/12 md:w-7/12 md:p-16 lg:w-4/12 shadow-login"
        >
            <img src="/LogoCyberTuition.png" class="w-52 md:w-60 m-auto mb-8" />
            <!-- Login Form -->
            <form
                v-on:submit.prevent="login()"
                class="flex flex-col items-center"
            >
                <!-- Email -->
                <div
                    class="border-2 shadow-login rounded-2xl w-11/12 md:w-96 py-3 mb-5 text-sm md:text-base pl-4 flex"
                    :style="{
                        borderColor: shouldValidate ? validateInput(email) : '',
                    }"
                >
                    <i class="bi bi-person inline mr-3 text-grey text-lg"></i>
                    <input
                        class="focus:outline-none w-full input"
                        type="email"
                        placeholder="E-Mel"
                        name="email"
                        v-model="email"
                    />
                </div>
                <!-- Password -->
                <div
                    class="border-2 shadow-login rounded-2xl w-11/12 md:w-96 py-3 text-sm md:text-base pl-4 flex"
                    :style="{
                        borderColor: shouldValidate
                            ? validateInput(password)
                            : '',
                    }"
                >
                    <i class="bi bi-lock inline mr-3 text-grey text-base"></i>
                    <input
                        class="focus:outline-none w-full input"
                        type="password"
                        placeholder="Kata Laluan"
                        name="password"
                        v-model="password"
                    />
                </div>
                <!-- Login Button -->
                <SubmitButton
                    type="submit"
                    txt="Log Masuk"
                    class="mt-8 md:mt-16 mb-5 flex m-auto"
                />
                <!-- Loading -->
                <div
                    class="fixed inset-0 flex items-center justify-center z-50"
                    v-if="loading"
                >
                    <Loading />
                </div>
            </form>

            <!-- Redirect to Sign Up Page -->
            <p class="text-xs text-center font-semibold text-grey2">
                <i class="bi bi-question-circle-fill text-slate-600 mr-1"></i>
                Belum mendaftar lagi?
                <router-link
                    to="/signup"
                    class="py-2 px-3 rounded-full ml-1 hover:bg-slate-700 hover:text-white text-black font-semibold transition underline hover:no-underline removeblue"
                    >Daftar Sekarang</router-link
                >
            </p>
        </div>
    </div>
</template>

<style>
.input:-webkit-autofill,
.input:-webkit-autofill:hover,
.input:-webkit-autofill:focus,
.input:-webkit-autofill:active {
    -webkit-box-shadow: 0 0 0 30px white inset !important;
    -webkit-text-fill-color: black !important;
}

.removeblue {
    -webkit-tap-highlight-color: transparent;
}
</style>
