<template>
    <SideBarPelajar linkActive="profil">
        <template v-slot:content>
            <!-- Breadcrumbs -->
            <h1 class="mt-5 font-semibold text-base md:text-xl">
                KEMASKINI PROFIL DIRI
            </h1>

            <p class="mb-5 font-semibold text-xs md:text-xs text-black">
                Dashboard &nbsp; > &nbsp; Profil Diri &nbsp;
                <span class="font-semibold text-xs inline text-red"
                    >> &nbsp; Kemaskini Profil Diri</span
                >
            </p>
            <!-- Loading -->
            <div
                class="fixed inset-0 flex items-center justify-center z-50"
                v-if="loading"
            >
                <Loading />
            </div>
            <div class="bg-white my-6 rounded-2xl py-2 shadow-login">
                <form
                    class="bg-white m-auto pt-1 pb-6 px-5"
                    v-on:submit.prevent="updateProfile()"
                >
                    <!-- Maklumat Diri Pelajar -->
                    <h4 class="text-base md:text-lg font-semibold mt-2 mb-4">
                        Maklumat Diri Pelajar
                    </h4>

                    <div class="flex max-md:flex-col">
                        <div class="grow">
                            <!-- Full Name -->
                            <p class="text-sm mb-2">Nama Penuh</p>
                            <input
                                type="text"
                                placeholder="Nama Penuh"
                                name="fullname"
                                v-model="studentData.nameStudent"
                                :style="{
                                    borderColor: shouldValidate
                                        ? validateInput(studentData.nameStudent)
                                        : '',
                                }"
                                class="border-2 border-slate-grey rounded-md w-11/12 py-3 px-4 block mb-3 md:mb-5 text-sm"
                            />
                            <!-- Phone Number -->
                            <p class="text-sm mb-2">No Telefon</p>
                            <input
                                type="text"
                                placeholder="No Telefon"
                                name="noPhone"
                                v-model="studentData.noPhoneStudent"
                                :style="{
                                    borderColor: shouldValidate
                                        ? validateInput(
                                              studentData.noPhoneStudent
                                          )
                                        : '',
                                }"
                                class="border-2 border-slate-grey rounded-md w-11/12 py-3 px-4 block mb-3 md:mb-5 text-sm"
                            />
                        </div>
                        <div class="grow">
                            <!-- Birth Date -->
                            <p class="text-sm mb-2">Tarikh Lahir</p>
                            <input
                                type="date"
                                placeholder="Tarikh Lahir"
                                name="birthdate"
                                v-model="studentData.dateOfBirth"
                                :style="{
                                    borderColor: shouldValidate
                                        ? validateInput(studentData.dateOfBirth)
                                        : '',
                                }"
                                class="border-2 border-slate-grey rounded-md w-11/12 py-3 px-4 block mb-3 md:mb-5 text-sm"
                            />
                            <!-- Form -->
                            <p class="text-sm mb-3">Tingkatan</p>
                            <select
                                name="tingkatan"
                                v-model="studentData.form"
                                :style="{
                                    borderColor: shouldValidate
                                        ? validateInput(studentData.form)
                                        : '',
                                }"
                                class="border-2 border-slate-grey rounded-md w-11/12 py-3 px-4 block mb-3 md:mb-5 text-sm"
                            >
                                <option value="4">4</option>
                                <option value="5">5</option>
                            </select>
                        </div>
                    </div>
                    <!-- Address -->
                    <div>
                        <p class="text-sm mb-3">Alamat Rumah</p>
                        <input
                            type="text"
                            placeholder="Alamat Rumah"
                            name="address"
                            v-model="studentData.address"
                            :style="{
                                borderColor: shouldValidate
                                    ? validateInput(studentData.address)
                                    : '',
                            }"
                            class="border-2 border-slate-grey rounded-md w-11/12 py-3 px-4 block mb-3 md:mb-5 text-sm h-16"
                        />
                    </div>

                    <!-- Maklumat Ibu Bapa -->

                    <h4 class="text-base md:text-lg font-semibold mt-6 mb-4">
                        Maklumat Ibu Bapa
                    </h4>

                    <div class="flex max-md:flex-col">
                        <div class="grow">
                            <!-- FullName -->
                            <p class="text-sm mb-3">Nama Penuh Ibu Bapa</p>
                            <input
                                type="text"
                                placeholder="Nama Penuh Ibu Bapa"
                                name="fullnameParents"
                                v-model="studentData.nameParent"
                                :style="{
                                    borderColor: shouldValidate
                                        ? validateInput(studentData.nameParent)
                                        : '',
                                }"
                                class="border-2 border-slate-grey rounded-md w-11/12 py-3 px-4 block mb-3 md:mb-5 text-sm"
                            />
                            <!-- No KP -->
                            <p class="text-sm mb-3">No Kad Pengenalan</p>
                            <input
                                type="text"
                                placeholder="No Kad Pengenalan"
                                name="noICParents"
                                v-model="studentData.noICParent"
                                :style="{
                                    borderColor: shouldValidate
                                        ? validateInput(studentData.noICParent)
                                        : '',
                                }"
                                class="border-2 border-slate-grey rounded-md w-11/12 py-3 px-4 block mb-3 md:mb-5 text-sm"
                            />
                        </div>
                        <div class="grow">
                            <!-- FullName -->
                            <p class="text-sm mb-3">No Telefon</p>
                            <input
                                type="text"
                                placeholder="No Telefon"
                                name="noPhoneParents"
                                v-model="studentData.noPhoneParent"
                                :style="{
                                    borderColor: shouldValidate
                                        ? validateInput(
                                              studentData.noPhoneParent
                                          )
                                        : '',
                                }"
                                class="border-2 border-slate-grey rounded-md w-11/12 py-3 px-4 block mb-3 md:mb-5 text-sm"
                            />
                        </div>
                    </div>
                    <div class="flex gap-4 mt-3">
                        <button
                            type="submit"
                            class="bg-red text-white px-5 py-2 rounded-2xl hover:bg-darkred text-xs lg:text-sm font-bold"
                        >
                            Sahkan
                        </button>
                        <button
                            type="button"
                            class="bg-gray-200 text-black px-5 rounded-2xl hover:bg-slate-300 text-xs lg:text-sm font-bold"
                            @click="cancel()"
                        >
                            Batalkan
                        </button>
                    </div>
                </form>
            </div>
        </template>
    </SideBarPelajar>
</template>

<script>
import axios from "axios";
const user = JSON.parse(sessionStorage.getItem("idUser"));
import { useToast } from "vue-toastification";
import { baseAPI } from "../../stores";
import SideBarPelajar from "../../components/SideBarPelajar.vue";
import router from "../../router";
import Loading from "../../components/Loading.vue";

export default {
    components: {
        SideBarPelajar,
        Loading,
    },
    data() {
        return {
            studentData: "",
            userEmail: "",
            studentId: "",
            toast: useToast(),
            shouldValidate: false,
            loading: false,
        };
    },

    async mounted() {
        document.title = "Kemaskini Profil | Pelajar";
        this.loading = true;

        // Get Student Data
        await axios
            .get(baseAPI + `/api/user/${user}`)
            .then((response) => {
                this.studentId = response.data.student.idStudent;
                axios
                    .get(baseAPI + `/api/student/${this.studentId}`)
                    .then((response) => {
                        this.studentData = response.data;
                        this.loading = false;
                    });
            })
            .catch((error) => {
                console.error(error);
            });

        // Get Student email from user table
        await axios
            .get(baseAPI + `/api/user/${user}`)
            .then((response) => {
                this.userEmail = response.data.email;
                this.loading = false;
            })
            .catch((error) => {
                console.error(error);
            });
    },
    methods: {
        validateInput(input) {
            if (input === "") {
                return "rgb(200 61 40)";
            }
        },
        // update profile
        async updateProfile() {
            this.shouldValidate = true;
            if (
                !this.studentData.nameStudent ||
                !this.studentData.dateOfBirth ||
                !this.studentData.noPhoneStudent ||
                !this.studentData.form ||
                !this.studentData.address ||
                !this.studentData.nameParent ||
                !this.studentData.noICParent ||
                !this.studentData.noPhoneParent
            ) {
                this.toast.error("Sila isi semua maklumat!", {
                    timeout: 3000,
                });
                window.scrollTo({
                    top: window.innerHeight / 3,
                    behavior: "smooth", // Use 'smooth' for smooth scrolling effect
                });
            } else {
                this.loading = true;
                const updatedStudent = {
                    nameStudent: this.studentData.nameStudent,
                    dateOfBirth: this.studentData.dateOfBirth,
                    noPhoneStudent: this.studentData.noPhoneStudent,
                    form: parseInt(this.studentData.form),
                    address: this.studentData.address,
                    nameParent: this.studentData.nameParent,
                    noICParent: this.studentData.noICParent,
                    noPhoneParent: this.studentData.noPhoneParent,
                };

                await axios
                    .put(
                        baseAPI + `/api/student/${this.studentData.idStudent}`,
                        updatedStudent
                    )
                    .then((response) => {
                        this.studentData = response.data;
                    })
                    .catch((error) => {});
                this.loading = false;
                this.toast.success("Kemaskini Profil Diri Berjaya", {
                    timeout: 3000,
                });
                router.push("/pelajar/profil");
            }
        },
        // cancel
        cancel() {
            router.push("/pelajar/profil");
        },
    },
};
</script>
