<template>
    <div>
        <v-container fluid>
            <h2>Students List</h2>
            <router-link :to="{name: 'Register'}"><v-btn color="secondary" small>Create Student</v-btn></router-link>
            <v-simple-table>
                <template v-slot:default>
                    <thead>
                        <tr >
                            <th class="text-left">RA(Registro do Aluno)</th>
                            <th class="text-left">Name</th>
                            <th class="text-left">Email</th>
                            <th class="text-left">CPF</th>
                            <th class="text-left">Actions</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="student in students" :key="student.ra">
                            <td>{{student.ra}}</td>
                            <td>{{student.name}}</td>
                            <td>{{student.email }}</td>
                            <td>{{student.cpf}}</td>
                            <td>
                                <router-link :to="{name: 'Edit', params: {id: student.ra}}"><v-btn color="warning" small>Edit</v-btn></router-link> |
                                <v-btn color="error" small @click="deleteUser(student.ra)">Delete</v-btn>
                            </td>
                        </tr>
                    </tbody>
                </template>
            </v-simple-table>
        </v-container>
    </div>
</template>

<script>
import axios from 'axios';
export default {
    name: 'UserViews',
    created() {
        axios.get('https://localhost:7082/api/Students').then(res => {
            this.students = res.data;
        }).catch(err => {
            console.log(err);
        })
    },
    data() 
    {
        return{
            search: '',
            headers: [
                {text: 'RA',value: 'ra'},
                { text: 'Name', value: 'name' },
                { text: 'Email', value: 'email' },
                { text: 'CFP', value: 'cpf' },
                { text: 'actions', value: 'actions' }
            ],
            students:[],
            deleteUserId: -1
        }
    },
    methods: {
        deleteUser(ra) {
            this.deleteUserId = ra;

            axios.delete('https://localhost:7082/api/Students/'+this.deleteUserId).then(res => {
                console.log(res);
                this.students = this.students.filter(s => s.ra != this.deleteUserId);
            }).catch(err => {
                console.log(err);
            })
        }
    }
}
</script>

<style scoped>

</style>