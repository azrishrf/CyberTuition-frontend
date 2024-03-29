<template>
    <div class="bg-slate-50 w-full min-h-screen flex">
        <!-- Side Bar -->
        <SidebarDashboard linkActive="guru1" />
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
            <h1 class="mt-3 mb-2 font-semibold text-xl">SENARAI GURU</h1>
            <p class="font-semibold text-xs inline mb-4">
                Dashboard &nbsp; > &nbsp; Guru &nbsp;
                <span class="font-semibold text-xs inline text-red"
                    >> &nbsp; Senarai Guru</span
                >
            </p>
            <!-- Content -->
            <div
                class="bg-white my-6 rounded-2xl py-5 px-5 shadow-login"
                v-if="teachers.length > 0"
            >
                <!-- Jadual Senarai Guru -->
                <table class="w-11/12 text-center">
                    <tr class="bg-red text-sm text-white">
                        <th class="font-semibold py-2 px-2 rounded-l-2xl">
                            No
                        </th>
                        <th class="font-semibold">Nama Penuh</th>
                        <th class="font-semibold">E-Mel</th>
                        <th class="font-semibold">Umur</th>
                        <th class="font-semibold">No Kad Pengenalan</th>
                        <th class="font-semibold">No Telefon</th>
                        <th class="font-semibold rounded-r-2xl">Tindakan</th>
                    </tr>

                    <tr
                        class="text-fontgrey text-sm border-b-2"
                        v-for="teacherData in teachers"
                    >
                        <td class="py-3 text-center">
                            {{ teachers.indexOf(teacherData) + 1 }}
                        </td>
                        <td class="font-semibold">
                            {{ teacherData.nameTeacher }}
                        </td>
                        <td class="font-semibold">
                            {{ teacherData.user.email }}
                        </td>
                        <td class="font-semibold">
                            {{ teacherData.ageTeacher }}
                        </td>
                        <td class="font-semibold">
                            {{ teacherData.noICTeacher }}
                        </td>
                        <td class="font-semibold">
                            {{ teacherData.noPhoneTeacher }}
                        </td>
                        <td class="font-semibold">
                            <router-link
                                class="fa-solid fa-pen-to-square text-lg text-black mx-2 cursor-pointer hover:text-red"
                                v-bind:to="
                                    `/kerani/guru/senaraiguru/kemaskiniguru/` +
                                    teacherData.idTeacher
                                "
                            ></router-link>
                            <button
                                type="button"
                                class="fa-regular fa-trash-can text-lg text-black mx-2 cursor-pointer hover:text-red"
                                @click="
                                    toggleConfirmDelete(teacherData.idTeacher)
                                "
                            ></button>
                        </td>
                        <!-- Dialog delete student -->
                        <div
                            v-if="isOpen"
                            class="fixed inset-0 flex items-center justify-center z-50 bg-gray-800 bg-opacity-10"
                        >
                            <div
                                class="w-2/6 bg-white px-3 py-4 top-1/3 rounded-xl"
                            >
                                <div>
                                    <i
                                        class="bi bi-exclamation-circle text-red text-4xl float-left mr-3"
                                    ></i>

                                    <h1
                                        class="font-semibold text-base text-left"
                                    >
                                        Memadam Data Guru
                                    </h1>
                                    <p class="font-normal text-xs text-left">
                                        Adakah anda pasti mahu memadamkan data
                                        guru ini?
                                    </p>

                                    <button
                                        @click="deleteData()"
                                        class="bg-red hover:bg-darkred text-white py-2 px-5 rounded-xl float-right mr-1 ml-3 mt-5 font-semibold text-xs"
                                    >
                                        Sahkan
                                    </button>
                                    <button
                                        @click="isOpen = false"
                                        class="text-black hover:bg-slate-300 py-2 px-4 rounded-xl border-2 border-grey float-right mt-5 font-semibold text-xs"
                                    >
                                        Batalkan
                                    </button>
                                </div>
                            </div>
                        </div>
                    </tr>
                </table>
            </div>
        </div>
    </div>
</template>

<script>
import axios from "axios";
import { baseAPI } from "../../stores";
import SidebarDashboard from "../../components/SidebarDashboard.vue";
import Loading from "../../components/Loading.vue";
import { useToast } from "vue-toastification";

export default {
    components: {
        SidebarDashboard,
        Loading,
    },
    data() {
        return {
            teachers: [],
            isOpen: false,
            selectedTeacher: null,
            loading: false,
            toast: useToast(),
        };
    },
    async mounted() {
        document.title = "Senarai Guru | Kerani";
        this.loading = true;

        const response = await axios.get(baseAPI + `/api/teachers`);
        this.teachers = response.data;
        this.loading = false;
    },
    methods: {
        toggleConfirmDelete(idTeacher) {
            this.selectedTeacher = idTeacher;

            this.isOpen = !this.isOpen;
        },

        async deleteData() {
            try {
                this.isOpen = false;
                this.loading = true;
                await axios
                    .delete(baseAPI + `/api/teacher/${this.selectedTeacher}`)
                    .then((response) => {
                        this.teachers = this.teachers.filter(
                            (teacher) =>
                                teacher.idTeacher !== this.selectedTeacher
                        );
                        this.loading = false;
                        this.toast.warning("Data Guru Telah Dipadam", {
                            timeout: 3000,
                        });
                    })
                    .catch((error) => console.log(error));
            } catch (error) {
                console.error("Error:", error);
            }
        },
    },
};
</script>
