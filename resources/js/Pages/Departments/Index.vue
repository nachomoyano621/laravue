<script setup>
import AuthenticatedLayout from '@/Layouts/AuthenticatedLayout.vue';
import DangerButton from '@/Components/DangerButton.vue';
import WarningButton from '@/Components/WarningButton.vue';
import { Head,Link,useForm } from '@inertiajs/vue3';
import Swal from 'sweetalert2';

const props = defineProps({
    departments: {type:Object}
});
const form = useForm({
    id:''
});
const deleteDepartment = (id,name) =>{
    const alerta = Swal.mixin({
        buttonsStyling:true
    });
    alerta.fire({
        title:'Esta seguro de eliminar '+name+' ?',
        icon:'question', showCancelButton:true,
        confirmButtonText:'<i class="fa-solid fa-check"></i> Si, eliminar',
        cancelButtonText:'<i class="fa-solid fa-ban"></i> Cancelar'
    }).then((result) => {
        if(result.isConfirmed) {
            form.delete(route('departments.destroy',id));
        }
    });
}
</script>

<template>
    <Head title="Departments" />

    <AuthenticatedLayout>
        <template #header>
            <h1 class="font-semibold text-xl text-gray-800 leading-tight text-center">Departamentos</h1>
        </template>

        <div class="py-12">
            <div class="bg-white grid v-screen place-items-center">
                <div class="mt-3 mb-3 flex">
                    <Link :href="route('departments.create')"
                    :class="'inline-flex items-center px-4 py-2 bg-gray-800 border border-transparent rounded-md font-semibold text-xs text-white uppercase tracking-widest hover:bg-gray-700 focus:bg-gray-700 active:bg-gray-900 focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:ring-offset-2 transition ease-in-out duration-150'">
                    <i class="fa-solid fa-plus-circle"></i> Agregar
                    </Link>
                </div>
            </div>
            <div class="bg-white grid v-screen place-items-center">
                <table class="table-auto border border-gray-400">
                    <thead>
                        <tr class="bg-gray-100">
                            <th class="px-4 py-4">NRO.</th>
                            <th class="px-4 py-4">DEPARTAMENTOS</th>
                            <th class="px-4 py-4"> </th>
                            <th class="px-4 py-4"> </th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="dep, i in departments" :key="dep.id">
                        <td class="border border-gray-400 px-4 py-4">{{ (i+1) }}</td>
                        <td class="border border-gray-400 px-4 py-4">{{ dep.name }}</td>
                        <td class="border border-gray-400 px-4 py-4">
                            <WarningButton
                            :href="route('departments.edit',dep.id)">
                            <i class="fa-solid fa-edit"></i>

                        </WarningButton>
                        </td>
                        <td class="border border-gray-400 px-4 py-4">
                            <DangerButton @click="deleteDepartment(dep.id,dep.name)">
                                <i class="fa-solid fa-trash"></i>
                            </DangerButton>
                        </td>
                        </tr>
                    </tbody>
                </table>
            </div>
        </div>
    </AuthenticatedLayout>
</template>
