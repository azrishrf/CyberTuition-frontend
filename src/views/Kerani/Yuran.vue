<template>
    <div class="bg-slate-50 w-full min-h-screen flex">
        <!-- Side Bar -->
        <SidebarDashboard linkActive="yuran" />
        <!-- Page Content -->
        <div class="w-full px-12 py-4">
            <!-- Top Bar -->
            <div
                class="flex justify-end bg-white shadow-login rounded-2xl px-6 py-2"
            >
                <div
                    class="flex items-center justify-center my-auto bg-red text-white w-28 rounded-2xl py-2 h-8"
                >
                    <i class="fa-solid fa-user text-xs"></i>
                    <span class="text-xs font-semibold ml-2">Kerani</span>
                </div>
            </div>
            <!-- Loading all components-->
            <div
                class="fixed inset-0 flex items-center justify-center z-50"
                v-if="loading1"
            >
                <Loading />
            </div>
            <!-- Loading status yuran-->
            <div
                class="fixed inset-0 flex items-center justify-center z-50"
                v-if="loading2"
            >
                <Loading />
            </div>
            <!-- Breadcrumbs -->
            <h1 class="mt-3 mb-2 font-semibold text-xl">YURAN</h1>
            <p class="font-semibold text-xs inline mb-4">
                Dashboard &nbsp;
                <span class="font-semibold text-xs inline text-red"
                    >> &nbsp; Yuran</span
                >
            </p>
            <div v-if="!loading1 && tuitionFees">
                <!-- Content -->
                <div class="bg-white my-6 rounded-2xl py-5 px-11 shadow-login">
                    <h1 class="mb-4 font-semibold text-lg">
                        Pengesahan Resit Bank
                    </h1>

                    <!-- Jadual Senarai Pelajar -->
                    <table
                        class="w-11/12 text-center"
                        v-if="receiptsBank.length > 0"
                    >
                        <tr class="bg-red text-sm text-white">
                            <th class="font-semibold py-2 px-2 rounded-l-2xl">
                                No
                            </th>
                            <th class="font-semibold" style="width: 25rem">
                                Nama Penuh
                            </th>
                            <th class="font-semibold">Tingkatan</th>
                            <th class="font-semibold">Tarikh Pembayaran</th>
                            <th class="font-semibold">Bulan</th>
                            <th class="font-semibold rounded-r-2xl">
                                Tindakan
                            </th>
                        </tr>
                        <tr
                            class="text-fontgrey text-sm border-b-2"
                            v-for="receipt in receiptsBank"
                            :key="receiptsBank.receiptBankId"
                        >
                            <td class="py-3 text-center">
                                {{ receiptsBank.indexOf(receipt) + 1 }}
                            </td>
                            <td class="font-semibold">
                                {{ receipt.tuitionFee.student.nameStudent }}
                            </td>
                            <td class="font-semibold">
                                {{ receipt.tuitionFee.student.form }}
                            </td>
                            <td class="font-semibold">
                                {{ formatDate(receipt.createdAt) }}
                            </td>
                            <td class="font-semibold">
                                {{ malayMonths[receipt.tuitionFee.month - 1] }}
                            </td>
                            <td class="font-semibold">
                                <router-link
                                    v-bind:to="
                                        `/kerani/yuran/pengesahan/` +
                                        receipt.receiptBankId
                                    "
                                    class="material-symbols-outlined text-black mx-1 cursor-pointer hover:text-red"
                                >
                                    quick_reference_all
                                </router-link>
                            </td>
                        </tr>
                    </table>
                    <div v-else>
                        <p class="text-center text-red font-semibold pb-2">
                            Tiada data resit bank yang memerlukan pengesahan
                            buat masa ini
                        </p>
                    </div>
                </div>

                <div class="bg-white my-6 rounded-2xl py-5 px-11 shadow-login">
                    <h1 class="mb-4 font-semibold text-lg">
                        Status Pembayaran Yuran
                    </h1>

                    <div class="flex gap-20 pb-4">
                        <div>
                            <!-- Bulan-->
                            <p class="text-sm mb-3">Bulan</p>
                            <select
                                placeholder="Bulan"
                                name="Bulan"
                                v-model="month"
                                class="border-2 border-slate-grey rounded-md py-3 px-4 block mb-3 text-sm w-80"
                            >
                                <option disabled value="">
                                    -- Pilih Bulan --
                                </option>
                                <option
                                    v-for="option in monthOptions"
                                    :value="option.value"
                                >
                                    {{ option.text }}
                                </option>
                            </select>
                        </div>
                        <div>
                            <!-- Tahun -->
                            <p class="text-sm mb-3">Tahun</p>
                            <select
                                placeholder="Tahun"
                                name="Tahun"
                                v-model="year"
                                class="border-2 border-slate-grey rounded-md py-3 px-4 block mb-3 text-sm w-80"
                            >
                                <option disabled value="">
                                    -- Pilih Tahun --
                                </option>
                                <option
                                    v-for="yearOption in yearOptions"
                                    :value="yearOption.value"
                                    :key="yearOption.value"
                                >
                                    {{ yearOption.text }}
                                </option>
                            </select>
                        </div>
                        <div class="my-auto mt-9">
                            <SubmitButton
                                type="button"
                                txt="Sahkan"
                                class="px-8 bg-slate-700 hover:bg-slate-800"
                                @click="submit()"
                            />
                        </div>
                    </div>
                    <div v-if="tuitionFees.length > 0">
                        <!-- Search bar -->
                        <div class="relative">
                            <input
                                type="text"
                                placeholder="Carian Nama Pelajar..."
                                class="font-semibold border-2 border-slate-grey text-sm px-8 py-2 mb-6 rounded-xl w-11/12 flex justify-between items-center"
                                v-model="searchQuery"
                            />
                            <i
                                class="fa-solid fa-magnifying-glass text-grey absolute right-40 top-3"
                            ></i>
                        </div>

                        <!-- Jadual Senarai Pelajar -->
                        <table class="w-11/12 text-center">
                            <tr class="bg-red text-sm text-white">
                                <th
                                    class="font-semibold py-2 px-2 rounded-l-2xl"
                                >
                                    No
                                </th>
                                <th class="font-semibold" style="width: 25rem">
                                    Nama Penuh
                                </th>
                                <th class="font-semibold">Tingkatan</th>
                                <th class="font-semibold">Status Pembayaran</th>
                                <th class="font-semibold rounded-r-2xl">
                                    Tindakan
                                </th>
                            </tr>

                            <tr
                                class="text-fontgrey text-sm border-b-2"
                                v-for="(
                                    studentData, index
                                ) in displayedStudents"
                            >
                                <td class="py-3 text-center">
                                    {{ tuitionFees.indexOf(studentData) + 1 }}
                                </td>
                                <td class="font-semibold">
                                    {{ studentData.student.nameStudent }}
                                </td>
                                <td class="font-semibold">
                                    {{ studentData.student.form }}
                                </td>
                                <td class="font-semibold">
                                    <p
                                        class="bg-green text-white px-8 py-1 rounded-xl text-xs inline-block"
                                        v-if="
                                            studentData.statusPayment ===
                                            'Telah Dibayar'
                                        "
                                    >
                                        Telah Dibayar
                                    </p>
                                    <p
                                        class="bg-darkred text-white px-8 py-1 rounded-xl text-xs inline-block"
                                        v-if="
                                            studentData.statusPayment ===
                                            'Belum Dibayar'
                                        "
                                    >
                                        Belum Dibayar
                                    </p>
                                    <p
                                        class="bg-yellow-400 text-white px-3 py-1 rounded-xl text-xs inline-block"
                                        v-if="
                                            studentData.statusPayment ===
                                            'Menunggu Pengesahan'
                                        "
                                    >
                                        Menunggu Pengesahan
                                    </p>
                                </td>
                                <td class="font-semibold">
                                    <router-link
                                        v-bind:to="
                                            `/kerani/yuran/penyatakewanganpelajar/` +
                                            studentData.idTuitionFee
                                        "
                                        class="material-symbols-outlined text-black mx-1 cursor-pointer hover:text-red"
                                    >
                                        quick_reference_all
                                    </router-link>
                                </td>
                            </tr>
                        </table>
                    </div>
                    <div v-else>
                        <p class="text-center text-red font-semibold py-10">
                            Tiada data yuran pelajar pada bulan yang dipilih
                        </p>
                    </div>
                    <!-- Pagination controls -->
                    <div
                        class="flex justify-center items-center text-sm mt-4 font-semibold text-fontgrey"
                    >
                        <button
                            class="px-2 py-1 rounded-full w-7 bg-red mr-2 text-white hover:bg-darkred"
                            @click="previousPage"
                            :disabled="currentPage === 1"
                        >
                            <i
                                class="fa-solid fa-angle-left"
                                style="color: #ffffff"
                            ></i>
                        </button>
                        <span class="px-2 py-1 rounded bg-gray-200">
                            Page {{ currentPage }} of {{ totalPages }}
                        </span>
                        <button
                            class="px-2 rounded-full py-1 w-7 bg-red ml-2 text-white hover:bg-darkred"
                            @click="nextPage"
                            :disabled="currentPage === totalPages"
                        >
                            <i
                                class="fa-solid fa-angle-right"
                                style="color: #ffffff"
                            ></i>
                        </button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from "axios";
import { baseAPI } from "../../stores";
import SidebarDashboard from "../../components/SidebarDashboard.vue";
import SubmitButton from "../../components/SubmitButton.vue";
import Loading from "../../components/Loading.vue";

export default {
    components: {
        SubmitButton,
        SidebarDashboard,
        Loading,
    },
    data() {
        return {
            receiptsBank: "",
            month: "",
            year: "",
            monthName: "",
            malayMonths: "",
            monthOptions: "",
            tuitionFees: "",
            filteredStudents: [],
            currentPage: 1,
            pageSize: 10,
            searchQuery: "",
            loading1: false,
            loading2: false,
        };
    },

    async mounted() {
        document.title = "Yuran | Kerani";

        this.loading1 = true;
        const response = await axios.get(baseAPI + `/api/receiptbank`);
        this.receiptsBank = response.data;

        this.malayMonths = [
            "Januari",
            "Februari",
            "Mac",
            "April",
            "Mei",
            "Jun",
            "Julai",
            "Ogos",
            "September",
            "Oktober",
            "November",
            "Disember",
        ];

        const currentMonthIndex = new Date().getMonth();
        const monthOptions = [];

        // // Set the default value of the month select element to the current month
        this.month = currentMonthIndex + 1;

        for (let i = 0; i < 12; i++) {
            const monthValue = i + 1;
            const monthName = this.malayMonths[i];
            monthOptions.push({ value: monthValue, text: monthName });
        }

        this.monthOptions = monthOptions;

        const currentYear = new Date().getFullYear();
        const yearOptions = [];

        for (let i = 0; i < 3; i++) {
            const yearValue = currentYear - i;
            yearOptions.push({ value: yearValue, text: yearValue.toString() });
        }
        this.yearOptions = yearOptions.reverse();
        this.year = currentYear;

        this.submit();
        this.filteredStudents = this.tuitionFees;
        this.currentPage = 1;
        this.loading1 = false;
    },
    computed: {
        displayedStudents() {
            const startIndex = (this.currentPage - 1) * this.pageSize;
            const endIndex = startIndex + this.pageSize;
            return this.filteredStudents.slice(startIndex, endIndex);
            // return this.filteredStudents.slice(startIndex, endIndex);
        },
        totalPages() {
            return Math.ceil(this.tuitionFees.length / this.pageSize);
        },
    },
    watch: {
        searchQuery: function (newQuery) {
            this.filterStudents();
        },
    },
    methods: {
        formatDate(date) {
            const dateObj = new Date(date);
            const day = dateObj.getDate();
            const month = dateObj.getMonth() + 1;
            const year = dateObj.getFullYear();

            return `${day < 10 ? "0" + day : day}/${
                month < 10 ? "0" + month : month
            }/${year}`;
        },
        async submit() {
            this.isDataAvailable = true;
            try {
                this.loading2 = true;

                const response = await axios.get(
                    baseAPI +
                        `/api/tuitionfee/monthyear/${this.month}/${this.year}`
                );
                this.tuitionFees = response.data;
                this.filteredStudents = response.data;
                this.currentPage = 1;
                this.loading2 = false;
            } catch (error) {
                console.error("Error:", error);
            }
        },
        goToPage(pageNumber) {
            if (pageNumber >= 1 && pageNumber <= this.totalPages) {
                this.currentPage = pageNumber;
            }
        },
        nextPage() {
            if (this.currentPage < this.totalPages) {
                this.currentPage++;
            }
        },
        previousPage() {
            if (this.currentPage > 1) {
                this.currentPage--;
            }
        },
        filterStudents() {
            if (this.searchQuery === "") {
                this.filteredStudents = this.tuitionFees;
            } else {
                const searchQuery = this.searchQuery.toLowerCase();
                this.filteredStudents = this.tuitionFees.filter(
                    (studentData) => {
                        const studentName =
                            studentData.student.nameStudent.toLowerCase();
                        return studentName.includes(searchQuery);
                    }
                );
                // this.currentPage = 1;
            }
        },
    },
};
</script>
