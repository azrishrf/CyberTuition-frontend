<template>
    <div class="bg-slate-50 w-full min-h-screen flex">
        <!-- Side Bar -->
        <SidebarDashboard linkActive="pelajar2" />
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
            <!-- Loading -->
            <div
                class="fixed inset-0 flex items-center justify-center z-50"
                v-if="loading"
            >
                <Loading />
            </div>
            <!-- Breadcrumbs -->
            <h1 class="mt-3 mb-2 font-semibold text-xl">
                PENGESAHAN PENDAFTARAN PELAJAR BARU
            </h1>
            <p class="font-semibold text-xs inline mb-4">
                Dashboard &nbsp; > &nbsp; Pelajar &nbsp;
                <span class="font-semibold text-xs inline text-red"
                    >> &nbsp; Pengesahan</span
                >
            </p>
            <!-- Content -->
            <div
                class="bg-white my-6 rounded-2xl py-5 px-5 shadow-login"
                v-if="students.length > 0"
            >
                <!-- Search bar -->
                <!-- <div
                    class="font-semibold border-2 border-grey text-sm px-8 py-2 mb-6 rounded-xl w-11/12 flex justify-between items-center"
                >
                    <input
                        type="text"
                        placeholder="Carian Nama Pelajar..."
                        class="w-full focus:outline-none"
                    />
                    <i class="fa-solid fa-magnifying-glass text-grey"></i>
                </div> -->

                <!-- Jadual Senarai Pelajar -->
                <table class="w-11/12 text-center">
                    <tr class="bg-red text-sm text-white">
                        <th class="font-semibold py-2 px-2 rounded-l-2xl">
                            No
                        </th>
                        <th class="font-semibold">Nama Penuh</th>
                        <th class="font-semibold">E-Mel</th>
                        <th class="font-semibold">Tingkatan</th>
                        <th class="font-semibold">No Kad Pengenalan</th>
                        <th class="font-semibold">Tarikh Lahir</th>
                        <th class="font-semibold rounded-r-2xl">Tindakan</th>
                    </tr>

                    <tr
                        class="text-fontgrey text-sm border-b-2"
                        v-for="studentData in students"
                    >
                        <td class="py-3 text-center">
                            {{ students.indexOf(studentData) + 1 }}
                        </td>
                        <td class="font-semibold">
                            {{ studentData.nameStudent }}
                        </td>
                        <td class="font-semibold">
                            {{ studentData.user.email }}
                        </td>
                        <td class="font-semibold">
                            {{ studentData.form }}
                        </td>
                        <td class="font-semibold">
                            {{ studentData.noICStudent }}
                        </td>
                        <td class="font-semibold">
                            {{ formatDate(studentData.dateOfBirth) }}
                        </td>
                        <td
                            class="font-semibold py-3 flex items-center justify-center"
                        >
                            <router-link
                                class="material-symbols-outlined text-black mx-1 cursor-pointer hover:text-red"
                                v-bind:to="
                                    `/kerani/pelajar/pengesahan/maklumatpengesahan/` +
                                    studentData.idStudent
                                "
                            >
                                quick_reference_all
                            </router-link>
                        </td>
                    </tr>
                </table>
            </div>
            <!-- Display text if student not available -->
            <div
                class="bg-white my-6 rounded-2xl py-5 px-5 shadow-login"
                v-else
            >
                <p class="text-center text-red font-semibold">
                    Tiada sebarang pengesahan pelajar pada masa ini
                </p>
            </div>
        </div>
    </div>
</template>

<script>
import axios from "axios";
import { baseAPI } from "../../stores";
import SidebarDashboard from "../../components/SidebarDashboard.vue";
import router from "../../router";
import { pengesahan } from "../../stores/index";
import Loading from "../../components/Loading.vue";

export default {
    components: {
        SidebarDashboard,
        Loading,
    },
    data() {
        return {
            students: [],
            isOpen: false,
            selectedStudent: null,
            loading: false,
        };
    },
    async mounted() {
        document.title = "Pengesahan | Kerani";
        this.loading = true;

        const response = await axios.get(
            baseAPI + `/api/students_notregistered`
        );
        this.students = response.data;
        this.loading = false;
    },
    methods: {
        async lihatButiran(id) {
            const response = await axios.get(baseAPI + `/api/student/${id}`);
            pengesahan.value = response;
            router.push("/kerani/pelajar/pengesahan/maklumatpengesahan");
        },
        // format date
        formatDate(dateString) {
            const parts = dateString.split("-"); // Split the date string by "-"
            const day = parts[2];
            const month = parts[1];
            const year = parts[0];

            return `${day}/${month}/${year}`;
        },
    },
};
</script>
