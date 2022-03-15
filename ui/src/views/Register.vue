<template>
    <div>
        <v-container fluid>
            <div v-if="error != undefined">
                <v-alert type="error">{{error}}</v-alert>
            </div>
            <h2>Student Register</h2><br>
            <v-text-field v-model="ra" label="RA" hint="12334554" clearable ></v-text-field>
            <v-text-field v-model="name" label="Student Name" clearable ></v-text-field>
            <v-text-field v-model="email" label="Student Email" hint="email@gmail/hotmail/outlook.com" clearable ></v-text-field>
            <v-text-field v-model="cpf" label="Student CPF" hint="000.000.000-00" clearable></v-text-field><br>
            <v-btn color='secondary' small @click="register">Create Student</v-btn> |
            <router-link :to="{name: 'User'}"><v-btn color="secondary" small>Cancel</v-btn></router-link>
        </v-container>
    </div>
</template>

<script>
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
                alert("Student created successfully");
            }).catch(err => {
                let errMsg = err.response.data;
                this.error = `Error: ${errMsg}`;
            })
        }
    }
}
</script>

<style scoped>

</style>