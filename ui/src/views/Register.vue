<template>
    <div>
        <div v-if="error != undefined">
            <p>{{error}}</p>
        </div>
        <h2>Student Register</h2>
        <hr>
        <v-text-field v-model="ra" label="RA" ></v-text-field>
        <v-text-field v-model="name" label="Student Name" clearable ></v-text-field>
        <v-text-field v-model="email" label="Student Email" clearable ></v-text-field>
        <v-text-field v-model="cpf" label="Student CPF"></v-text-field>
        <v-btn color='secondary' small @click="register">Create Student</v-btn>
    </div>
</template>

<script>
//import RegisterComp from '../components/RegisterComp.vue'
import axios from 'axios';
export default {
    name: 'RegisterView',
    data() {
        return {
            ra: "",
            name: "",
            email: "",
            cpf: "",
            error: undefined
        }
    }, 
    methods: {
        register() {
            axios.post("https://localhost:7082/api/Students",{
                ra: Number(this.ra),
                name: this.name,
                email: this.email,
                cpf: this.cpf
            }).then(res => {
                console.log(res);
                this.$router.push({name: "User"});
            }).catch(err => {
                let errMsg = err.response.data.title;
                this.error = `Error: ${errMsg}`;
            })
        }
    }

    /*components: {
        RegisterComp,
    },*/
}
</script>

<style scoped>

</style>