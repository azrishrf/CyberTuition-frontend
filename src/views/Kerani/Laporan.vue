<template>
    <div class="bg-slate-50 w-full min-h-screen flex">
        <!-- Side Bar -->
        <SidebarDashboard linkActive="laporan" />
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

            <!-- Breadcrumbs -->
            <h1 class="mt-3 mb-2 font-semibold text-xl">LAPORAN</h1>
            <p class="font-semibold text-xs inline mb-4">
                Dashboard &nbsp;
                <span class="font-semibold text-xs inline text-red">
                    > &nbsp; Laporan</span
                >
            </p>

            <!-- Laporan Bulanan -->
            <!-- <h1 class="mt-3 mb-2 font-semibold text-lg">Laporan Bulanan</h1> -->
            <div class="shadow-login bg-white py-4 px-5 rounded-2xl my-5">
                <div class="flex gap-20">
                    <div>
                        <!-- Bulan-->
                        <p class="text-sm mb-3">Bulan</p>
                        <select
                            placeholder="Bulan"
                            name="Bulan"
                            v-model="month"
                            class="border-2 border-slate-grey rounded-md py-3 px-4 block mb-3 text-sm w-80"
                        >
                            <option disabled value="">-- Pilih Bulan --</option>
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
                            <option disabled value="">-- Pilih Tahun --</option>
                            <option
                                v-for="yearOption in yearOptions"
                                :value="yearOption.value"
                                :key="yearOption.value"
                            >
                                {{ yearOption.text }}
                            </option>
                        </select>
                    </div>
                    <!-- Loading -->
                    <div
                        class="fixed inset-0 flex items-center justify-center z-50"
                        v-if="loading"
                    >
                        <Loading />
                    </div>
                    <div class="my-auto mt-9">
                        <SubmitButton
                            type="button"
                            txt="Sahkan"
                            class="px-8 bg-slate-700 hover:bg-slate-800"
                            @click="confirmForm()"
                        />
                    </div>
                </div>
                <p
                    class="error-message text-xs text-red grow mb-3 font-semibold"
                >
                    {{ errorMessage }}
                </p>
            </div>

            <!-- Data Laporan -->
            <div
                class="shadow-login bg-white py-7 px-20 rounded-2xl my-5"
                v-if="showReport"
                id="data-laporan"
            >
                <div class="text-center">
                    <img src="/LogoCyberTuition.png" class="w-60 m-auto" />
                    <h1 class="font-semibold">LAPORAN BULANAN</h1>
                    <h1 class="font-semibold mb-5">
                        {{ monthName.toUpperCase() }} {{ year }}
                    </h1>
                </div>

                <div class="flex pl-10 gap-4">
                    <!-- 1 -->
                    <div class="grow">
                        <!-- Pelajar -->
                        <div class="flex flex-col w-72 mb-5">
                            <h1
                                class="font-semibold my-2 bg-gray-600 text-sm text-white py-2 px-2 rounded-2xl text-center w-full"
                            >
                                Pelajar
                            </h1>
                            <table class="text-sm bg-slate-100 rounded-2xl">
                                <tr>
                                    <td class="font-semibold py-3 pl-4">
                                        Jumlah Pelajar:
                                    </td>
                                    <td
                                        class="font-semibold py-3 px-4 text-center text-fontgrey"
                                    >
                                        {{ studentsData.totalStudents }}
                                    </td>
                                </tr>
                                <tr>
                                    <td class="font-semibold pb-3 pl-4">
                                        Jumlah Pelajar Baru Berdaftar:
                                    </td>
                                    <td
                                        class="font-semibold pb-3 px-4 text-center text-fontgrey"
                                    >
                                        {{
                                            studentsData.totalStudentsNewRegistered
                                        }}
                                    </td>
                                </tr>
                                <tr>
                                    <td class="font-semibold pb-3 pl-4">
                                        Tingkatan 4:
                                    </td>
                                    <td
                                        class="font-semibold pb-3 px-4 text-center text-fontgrey"
                                    >
                                        {{ studentsData.totalForm4Students }}
                                    </td>
                                </tr>
                                <tr>
                                    <td class="font-semibold pb-3 pl-4">
                                        Tingkatan 5:
                                    </td>
                                    <td
                                        class="font-semibold pb-3 px-4 text-center text-fontgrey"
                                    >
                                        {{ studentsData.totalForm5Students }}
                                    </td>
                                </tr>
                            </table>
                        </div>
                        <!-- Guru -->
                        <div class="flex flex-col w-72 mb-5">
                            <h1
                                class="font-semibold my-2 bg-gray-600 text-sm text-white py-2 px-2 rounded-2xl text-center w-full"
                            >
                                Guru
                            </h1>
                            <table class="text-sm bg-slate-100 rounded-2xl">
                                <tr>
                                    <td class="font-semibold py-3 pl-4">
                                        Jumlah Guru:
                                    </td>
                                    <td
                                        class="font-semibold py-3 px-4 text-center text-fontgrey"
                                    >
                                        {{ teachersData.totalTeachers }}
                                    </td>
                                </tr>
                                <tr>
                                    <td class="font-semibold pb-3 pl-4">
                                        Jumlah Guru Baru Berdaftar:
                                    </td>
                                    <td
                                        class="font-semibold pb-3 px-4 text-center text-fontgrey"
                                    >
                                        {{
                                            teachersData.totalTeachersNewRegistered
                                        }}
                                    </td>
                                </tr>
                            </table>
                        </div>
                        <!-- Kelas -->
                        <div class="flex flex-col w-72 mb-5">
                            <h1
                                class="font-semibold my-2 bg-gray-600 text-sm text-white py-2 px-2 rounded-2xl text-center w-full"
                            >
                                Subjek
                            </h1>
                            <table class="text-sm bg-slate-100 rounded-2xl">
                                <tr>
                                    <td class="font-semibold py-3 pl-4">
                                        Jumlah Subjek:
                                    </td>
                                    <td
                                        class="font-semibold py-3 px-4 text-center text-fontgrey"
                                    >
                                        {{ subjectsData.totalClasses }}
                                    </td>
                                </tr>
                            </table>
                        </div>
                    </div>
                    <!-- 2 -->
                    <div class="grow">
                        <!-- Kedatangan Pelajar -->
                        <div class="flex flex-col w-72 mb-5">
                            <h1
                                class="font-semibold my-2 bg-gray-600 text-sm text-white py-2 px-2 rounded-2xl text-center w-full"
                            >
                                Kedatangan Pelajar
                            </h1>
                            <table class="text-sm bg-slate-100 rounded-2xl">
                                <tr>
                                    <td class="font-semibold py-3 pl-4">
                                        Jumlah Kelas Bulan Ini:
                                    </td>
                                    <td
                                        class="font-semibold py-3 px-4 text-center text-fontgrey"
                                    >
                                        {{ attendanceData.totalAttendance }}
                                    </td>
                                </tr>
                                <tr>
                                    <td class="font-semibold pb-3 pl-4">
                                        Pelajar Hadir:
                                    </td>
                                    <td
                                        class="font-semibold pb-3 px-4 text-center text-fontgrey"
                                    >
                                        {{ attendanceData.totalStudentAttends }}
                                        /
                                        {{
                                            attendanceData.totalStudentAttendanceAll
                                        }}
                                    </td>
                                </tr>
                                <tr>
                                    <td class="font-semibold pb-3 pl-4">
                                        Peratus Kehadiran:
                                    </td>
                                    <td
                                        class="font-semibold pb-3 px-4 text-center text-fontgrey"
                                    >
                                        {{ attendanceData.percentageAttends }}
                                    </td>
                                </tr>
                                <tr>
                                    <td class="font-semibold pb-3 pl-4">
                                        Pelajar Tidak Hadir
                                    </td>
                                    <td
                                        class="font-semibold pb-3 px-4 text-center text-fontgrey"
                                    >
                                        {{
                                            attendanceData.totalStudentNotAttends
                                        }}
                                        /
                                        {{
                                            attendanceData.totalStudentAttendanceAll
                                        }}
                                    </td>
                                </tr>
                                <tr>
                                    <td class="font-semibold pb-3 pl-4">
                                        Peratus Ketidakhadiran:
                                    </td>
                                    <td
                                        class="font-semibold pb-3 px-4 text-center text-fontgrey"
                                    >
                                        {{
                                            attendanceData.percentageNotAttends
                                        }}
                                    </td>
                                </tr>
                            </table>
                        </div>
                        <!-- Yuran -->
                        <div class="flex flex-col w-72 mb-5">
                            <h1
                                class="font-semibold my-2 bg-gray-600 text-sm text-white py-2 px-2 rounded-2xl text-center w-full"
                            >
                                Yuran
                            </h1>
                            <table class="text-sm bg-slate-100 rounded-2xl">
                                <tr>
                                    <td class="font-semibold py-3 pl-4">
                                        Jumlah Yuran:
                                    </td>
                                    <td
                                        class="font-semibold py-3 px-4 text-center text-fontgrey"
                                    >
                                        RM
                                        {{ tuitionFeesData.totalTuitionFees }}
                                    </td>
                                </tr>
                                <tr>
                                    <td class="font-semibold pb-3 pl-4">
                                        Yuran Sudah Dibayar:
                                    </td>
                                    <td
                                        class="font-semibold pb-3 px-4 text-center text-fontgrey"
                                    >
                                        RM
                                        {{
                                            tuitionFeesData.totalTuitionFeesPaid
                                        }}
                                    </td>
                                </tr>
                                <tr>
                                    <td class="font-semibold pb-3 pl-4">
                                        Yuran Belum Dibayar:
                                    </td>
                                    <td
                                        class="font-semibold pb-3 px-4 text-center text-fontgrey"
                                    >
                                        RM
                                        {{
                                            tuitionFeesData.totalTuitionFeesUnpaid
                                        }}
                                    </td>
                                </tr>
                            </table>
                        </div>
                    </div>
                    <!-- 3 -->
                    <div class="grow">
                        <!-- Jumlah Pelajar Mengikut Kelas -->
                        <div class="flex flex-col w-72 mb-5">
                            <h1
                                class="font-semibold my-2 bg-gray-600 text-sm text-white py-2 px-2 rounded-2xl text-center w-full"
                            >
                                Jumlah Pelajar Mengikut Subjek
                            </h1>
                            <table class="text-sm bg-slate-100 rounded-2xl">
                                <tr v-for="subjectData in subjectStudentCounts">
                                    <td class="font-semibold py-2 pl-4">
                                        {{ subjectData.subjectName }}
                                    </td>
                                    <td
                                        class="font-semibold py-2 px-4 text-center text-fontgrey"
                                    >
                                        {{ subjectData.totalStudents }}
                                    </td>
                                </tr>
                            </table>
                        </div>
                    </div>
                </div>
            </div>
            <!-- Generate pdf button -->
            <div class="flex flex-row-reverse" v-if="showReport">
                <button
                    @click="generatePDF"
                    class="flex flex-col items-center py-4 px-5 bg-gray-500 hover:bg-slate-700 shadow-login rounded-2xl"
                >
                    <i class="fa-solid fa-print text-2xl text-white"></i>
                    <p class="font-semibold text-white text-sm">Cetak</p>
                </button>
            </div>
        </div>
    </div>
</template>

<script>
import axios from "axios";
import html2pdf from "html2pdf.js";
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
            month: "",
            year: "",
            monthName: "",
            malayMonthNames: "",
            monthOptions: "",
            studentsData: "",
            teachersData: "",
            subjectsData: "",
            subjectStudentCounts: "",
            attendanceData: "",
            tuitionFeesData: "",
            showReport: false,
            loading: false,
        };
    },
    async mounted() {
        document.title = "Laporan | Kerani";
        // Create an array of month names in Malay
        this.malayMonthNames = [
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

        for (let i = currentMonthIndex; i >= 0; i--) {
            const monthValue = i + 1;
            const monthName = this.malayMonthNames[i];
            monthOptions.push({ value: monthValue, text: monthName });
        }

        // Set the options array in your Vue.js component's data
        this.monthOptions = monthOptions.reverse();

        // Set the default value of the month select element to the current month
        this.month = currentMonthIndex + 1;

        const currentYear = new Date().getFullYear();
        const yearOptions = [];

        for (let i = 0; i < 3; i++) {
            const yearValue = currentYear - i;
            yearOptions.push({ value: yearValue, text: yearValue.toString() });
        }
        this.yearOptions = yearOptions.reverse();
        this.year = currentYear;
        this.confirmForm();
    },

    methods: {
        confirmForm() {
            this.studentsReport();
            this.teachersReport();
            this.subjectsReport();
            this.attendanceReport();
            this.tuitionFeesReport();
            // this.showReport = true;
            this.monthName = this.malayMonthNames[this.month - 1];
        },

        async studentsReport() {
            this.loading = true;
            // Make the API request
            try {
                this.loading = true;
                const response = await axios.post(
                    baseAPI + "/api/report/students",
                    {
                        month: this.month,
                        year: this.year,
                    }
                );
                this.studentsData = response.data;
            } catch (error) {
                console.error("Error:", error);
            }
            this.loading = false;
            this.showReport = true;
        },
        async teachersReport() {
            // Make the API request
            this.loading = true;

            try {
                const response = await axios.post(
                    baseAPI + "/api/report/teachers",
                    {
                        month: this.month,
                        year: this.year,
                    }
                );
                this.teachersData = response.data;
            } catch (error) {
                console.error("Error:", error);
            }
            this.loading = false;
        },
        async subjectsReport() {
            this.loading = true;

            // Make the API request
            try {
                const response = await axios.post(
                    baseAPI + "/api/report/subjects",
                    {
                        month: this.month,
                        year: this.year,
                    }
                );
                this.subjectsData = response.data;
                this.subjectStudentCounts =
                    this.subjectsData.subjectStudentCounts;
            } catch (error) {
                console.error("Error:", error);
            }
            this.loading = false;
        },
        async attendanceReport() {
            this.loading = true;

            // Make the API request
            try {
                const response = await axios.post(
                    baseAPI + "/api/report/attendance",
                    {
                        month: this.month,
                        year: this.year,
                    }
                );
                this.attendanceData = response.data;
            } catch (error) {
                console.error("Error:", error);
            }
            this.loading = false;
        },
        async tuitionFeesReport() {
            this.loading = true;

            // Make the API request
            try {
                const response = await axios.post(
                    baseAPI + "/api/report/tuitionfees",
                    {
                        month: this.month,
                        year: this.year,
                    }
                );
                this.tuitionFeesData = response.data;
            } catch (error) {
                console.error("Error:", error);
            }
            this.loading = false;
        },
        // Generate pdf
        generatePDF() {
            this.loading = true;

            const element = document.getElementById("data-laporan");

            html2pdf()
                .set({
                    html2canvas: {
                        scale: 2, // Adjust the scale value to make the PDF smaller (e.g., 2 for half the size)
                    },
                    pagebreak: { mode: "avoid-all" }, // Optional: Set page break mode to avoid splitting content across pages
                    filename: `Laporan Bulanan ${this.monthName} ${this.year}.pdf`, // Optional: Set the desired file name
                    jsPDF: {
                        orientation: "landscape", // Set the page orientation to landscape
                    },
                })
                .from(element)
                .save();
            this.loading = false;
        },
    },
};
</script>
