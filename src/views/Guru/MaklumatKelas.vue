<template>
    <div class="bg-slate-50 w-full min-h-screen flex">
        <!-- Side Bar -->
        <SidebarGuru linkActive="kelas" />
        <!-- Page Content -->
        <div class="w-full px-12 py-4">
            <!-- Loading -->
            <div
                class="fixed inset-0 flex items-center justify-center z-50"
                v-if="loading"
            >
                <Loading />
            </div>
            <!-- Top Bar -->
            <div
                class="flex justify-end bg-white shadow-login rounded-2xl px-6 py-2"
            >
                <div
                    class="flex items-center justify-between bg-red w-28 text-white text-xs px-4 rounded-3xl py-2 font-semibold"
                >
                    <i class="fa-solid fa-user text-xs"></i>Guru
                    <i class="fa-solid fa-angle-down"></i>
                </div>
            </div>
            <!-- Breadcrumbs -->
            <h1 class="my-2 font-semibold text-xl">SUBJEK</h1>
            <p class="font-semibold text-xs inline mb-4">
                Dashboard &nbsp;> &nbsp; Subjek &nbsp;
                <span class="font-semibold text-xs inline text-red"
                    >> &nbsp; Maklumat Subjek</span
                >
            </p>

            <div v-if="!loading">
                <!-- Maklumat Kelas -->
                <div class="shadow-login bg-white py-5 px-11 rounded-2xl my-5">
                    <div>
                        <h1 class="text-lg font-semibold mt-2 mb-4">
                            Maklumat Subjek
                        </h1>
                        <table class="text-sm w-4/5">
                            <tr>
                                <td class="font-semibold pb-3 w-48">
                                    Nama Subjek:
                                </td>
                                <td class="text-fontgrey font-medium pb-3">
                                    {{ subjectData.name }}
                                </td>
                            </tr>
                            <tr>
                                <td class="font-semibold pb-3">Hari:</td>
                                <td class="text-fontgrey font-medium pb-3">
                                    {{ subjectData.day }}
                                </td>
                            </tr>
                            <tr>
                                <td class="font-semibold pb-3">Masa:</td>
                                <td class="text-fontgrey font-medium pb-3">
                                    {{ subjectData.time }}
                                </td>
                            </tr>
                            <tr>
                                <td class="font-semibold pb-3">Nama Guru:</td>
                                <td class="text-fontgrey font-medium pb-3">
                                    {{ teacher.nameTeacher }}
                                </td>
                            </tr>
                        </table>
                    </div>
                </div>

                <!-- Content -->
                <div class="bg-white my-6 rounded-2xl py-5 px-11 shadow-login">
                    <h1 class="text-lg font-semibold mt-2 mb-4">
                        Senarai Pelajar
                    </h1>

                    <!-- Jadual Senarai Pelajar -->
                    <table class="w-11/12 text-center">
                        <tr class="bg-red text-sm text-white">
                            <th class="font-semibold py-2 px-2 rounded-l-2xl">
                                No
                            </th>
                            <th class="font-semibold">Nama Penuh</th>

                            <th class="font-semibold">Tingkatan</th>
                            <th class="font-semibold rounded-r-2xl">
                                No Kad Pengenalan
                            </th>
                        </tr>
                        <tr
                            class="text-fontgrey text-sm border-b-2"
                            v-for="studentData in listStudents"
                        >
                            <td class="py-3 text-center">
                                {{ listStudents.indexOf(studentData) + 1 }}
                            </td>
                            <td class="font-semibold">
                                {{ studentData.student.nameStudent }}
                            </td>

                            <td class="font-semibold">
                                {{ studentData.student.form }}
                            </td>
                            <td class="font-semibold">
                                {{ studentData.student.noICStudent }}
                            </td>
                        </tr>
                    </table>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from "axios";
import { baseAPI } from "../../stores";
import router from "../../router";
import SidebarGuru from "../../components/SidebarGuru.vue";
import Loading from "../../components/Loading.vue";

export default {
    components: {
        SidebarGuru,
        Loading,
    },
    data() {
        return {
            subjectData: {},
            idSubject: router.currentRoute.value.params.id,
            teacher: [],
            listStudents: {},
            loading: false,
        };
    },
    async mounted() {
        document.title = "Maklumat Subjek | Guru";
        this.loading = true;

        const response = await axios.get(
            baseAPI + `/api/subject/${this.idSubject}`
        );
        this.subjectData = response.data;
        this.teacher = this.subjectData.teacher;

        this.listStudents = this.subjectData.student_Subject;

        this.loading = false;
    },
};
</script>
