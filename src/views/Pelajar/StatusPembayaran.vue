<template>
    <SideBarPelajar linkActive="yuran">
        <template v-slot:content>
            <!-- Loading -->
            <div
                class="fixed inset-0 flex items-center justify-center z-50"
                v-if="loading"
            >
                <Loading />
            </div>
            <!-- Conditionally display div based on statusId -->
            <div v-if="statusId == 1 && !loading">
                <div class="flex justify-center">
                    <div
                        class="shadow-login bg-white rounded-2xl my-6 py-8 lg:w-6/12"
                    >
                        <!-- <p class="text-green-600">Success</p> -->
                        <div class="flex justify-center w-full">
                            <img
                                src="/PaymentSuccessful.jpg"
                                alt="Payment Successful"
                                class="w-72 h-full"
                            />
                        </div>

                        <p class="text-center font-bold text-xl text-green">
                            PEMBAYARAN BERJAYA
                        </p>
                        <p
                            class="text-center font-semibold text-fontgrey text-xs py-2"
                        >
                            Bayaran anda telah diproses! <br />
                            Butiran transaksi disertakan di bawah
                        </p>
                        <p
                            class="text-center font-semibold text-blue-400 text-xs"
                        >
                            ID Transaksi: {{ this.transactionId }}
                        </p>
                        <!-- Details -->
                        <div>
                            <div class="flex justify-center py-5 px-7 gap-3">
                                <div class="flex flex-col gap-2 text-xs">
                                    <p class="font-semibold text-black">
                                        Yuran:
                                    </p>
                                    <p class="font-semibold text-black">
                                        Emel:
                                    </p>
                                    <p class="font-semibold text-black">
                                        Jenis Pembayaran:
                                    </p>
                                    <p class="font-semibold text-black">
                                        Jumlah Pembayaran:
                                    </p>
                                    <p class="font-semibold text-black">
                                        Tarikh Transaksi:
                                    </p>
                                </div>
                                <div
                                    class="flex flex-col gap-2 font-medium text-xs"
                                >
                                    <p class="text-fontgrey">
                                        Bulan {{ this.tuitionFeeData.month }},
                                        {{ this.tuitionFeeData.year }}
                                    </p>
                                    <p class="text-fontgrey">
                                        {{ this.userData.email }}
                                    </p>
                                    <p class="text-fontgrey">Payment Gateway</p>
                                    <p class="text-fontgrey">
                                        RM {{ this.tuitionFeeData.amount }}
                                    </p>
                                    <p class="text-fontgrey">
                                        {{ this.currentDateTime }}
                                    </p>
                                </div>
                            </div>
                        </div>

                        <div class="flex justify-center my-2">
                            <SubmitButton
                                type="submit"
                                txt="Dashboard"
                                class="px-3 py-1 text-sm font-semibold"
                                @click="redirectDashboard()"
                            />
                        </div>
                    </div>
                </div>
            </div>
            <div v-else-if="statusId == 2" class="mt-4">
                <div class="flex justify-center">
                    <div
                        class="shadow-login bg-white rounded-2xl my-6 py-8 lg:w-6/12"
                    >
                        <!-- <p class="text-green-600">Success</p> -->
                        <div class="flex justify-center w-full">
                            <img
                                src="/PaymentPending.jpg"
                                alt="Payment Pending"
                                class="w-72 h-full mb-5"
                            />
                        </div>

                        <p
                            class="text-center font-bold text-xl text-yellow-500 px-7"
                        >
                            PAYMENT PENDING
                        </p>
                        <p
                            class="text-center font-bold text-fontgrey text-xs py-4 px-8"
                        >
                            Pembayaran anda masih menunggu pengesahan daripada
                            payment gateway.
                        </p>

                        <div class="flex justify-center my-2">
                            <SubmitButton
                                type="submit"
                                txt="Yuran"
                                class="px-8 py-1 text-sm font-semibold"
                                @click="redirectYuran()"
                            />
                        </div>
                    </div>
                </div>
            </div>
            <div v-else-if="statusId == 3" class="mt-4">
                <div class="flex justify-center">
                    <div
                        class="shadow-login bg-white rounded-2xl my-6 py-8 lg:w-6/12"
                    >
                        <!-- <p class="text-green-600">Success</p> -->
                        <div class="flex justify-center w-full">
                            <img
                                src="/PaymentUnsuccessful.png"
                                alt="Payment Successful"
                                class="w-72 h-full mb-5"
                            />
                        </div>

                        <p class="text-center font-bold text-xl text-red px-7">
                            PEMBAYARAN TIDAK BERJAYA
                        </p>
                        <p
                            class="text-center font-bold text-fontgrey text-xs py-4 px-14"
                        >
                            Bayaran anda telah ditolak. <br />
                            Sila cuba lagi kemudian atau gunakan kaedah
                            pembayaran lain.
                        </p>

                        <div class="flex justify-center my-2">
                            <SubmitButton
                                type="submit"
                                txt="Yuran"
                                class="px-8 py-1 text-sm font-semibold"
                                @click="redirectYuran()"
                            />
                        </div>
                    </div>
                </div>
            </div>
        </template>
    </SideBarPelajar>
</template>

<script setup></script>

<script>
import axios from "axios";
const user = JSON.parse(sessionStorage.getItem("idUser"));
import { baseAPI } from "../../stores";
import SideBarPelajar from "../../components/SideBarPelajar.vue";
import SubmitButton from "../../components/SubmitButton.vue";
import router from "../../router";
import Loading from "../../components/Loading.vue";

export default {
    components: {
        SideBarPelajar,
        SubmitButton,
        Loading,
    },
    data() {
        return {
            statusId: "",
            transactionId: "",
            billCode: "",
            tuitionFeeData: "",
            userData: "",
            currentDateTime: "",
            loading: false,
        };
    },

    async mounted() {
        document.title = "Status Pembayaran | Pelajar";
        this.loading = true;
        // Get data user
        await axios.get(baseAPI + `/api/user/${user}`).then((response) => {
            this.userData = response.data;
        });
        // Get id Tuition Fee
        const idTuitionFee = JSON.parse(sessionStorage.getItem("idTuitionFee"));
        // Get id Tuition Fee
        const paymentGatewayId = JSON.parse(
            sessionStorage.getItem("paymentGatewayId")
        );

        // Get data from param URL
        this.statusId = this.$route.query.status_id;
        this.transactionId = this.$route.query.transaction_id;

        // Create data payment gateway
        const updatePaymentGatewayData = {
            transactionBill: this.transactionId,
            paymentGatewayId: paymentGatewayId,
        };

        await axios
            .put(baseAPI + "/api/paymentgateway", updatePaymentGatewayData)
            // .then((response) => {})
            .catch((error) => {
                const errorMessage = error.response.data.error;
                console.log(errorMessage);
            });

        if (this.statusId == 1) {
            this.date();
            await axios
                .put(baseAPI + `/api/tuitionfee/${idTuitionFee}`)
                .then((response) => {
                    this.tuitionFeeData = response.data;
                });
        }
        this.loading = false;
    },
    methods: {
        date() {
            const options = {
                day: "numeric",
                month: "long",
                year: "numeric",
                hour: "numeric",
                minute: "numeric",
                hour12: true,
                timeZone: "Asia/Kuala_Lumpur",
            };
            const formatter = new Intl.DateTimeFormat("ms-MY", options);
            this.currentDateTime = formatter
                .format(new Date())
                .replace("pada", ",")
                .replace("PG", "AM")
                .replace("PTG", "PM");
        },
        redirectDashboard() {
            router.push("/pelajar/dashboard");
        },
        redirectYuran() {
            router.push("/pelajar/yuran");
        },
    },
};
</script>
