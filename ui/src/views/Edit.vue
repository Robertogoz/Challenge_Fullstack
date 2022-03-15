<template>
    <div>
        <v-container fluid>
            <div v-if="error != undefined">
                <v-alert type="error">{{error}}</v-alert>
            </div>
            <h2>Student Edit</h2><br>
            <v-text-field v-model="ra" label="RA" disabled hint='12334554' ></v-text-field>
            <v-text-field v-model="name" label="Student Name" clearable ></v-text-field>
            <v-text-field v-model="email" label="Student Email" clearable hint="email@gmail/hotmail/outlook.com" ></v-text-field>
            <v-text-field v-model="cpf" label="Student CPF" disabled hint="000.000.000-00"></v-text-field><br>
            <v-btn color='secondary' small @click="update">Edit Student</v-btn> |
            <router-link :to="{name: 'User'}"><v-btn color="secondary" small>Cancel</v-btn></router-link>
        </v-container>
    </div>
</template>

<script>
//import RegisterComp from '../components/RegisterComp.vue'
import axios from 'axios';
export default {
    name: 'EditView',
    created() {
        axios.get("https://localhost:7082/api/Students/"+ this.$route.params.id).then(res => {
            console.log(res);
            this.ra = res.data.ra;
            this.name = res.data.name;
            this.email = res.data.email;
            this.cpf = res.data.cpf;
        }).catch(err => {
            console.log(err.response);
            this.$router.push({name: 'User'});
        })
    },
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
        update() {
            axios.put("https://localhost:7082/api/Students/"+this.ra,{
                ra: Number(this.ra),
                name: this.name,
                email: this.email,
                cpf: this.cpf
            }).then(res => {
                console.log(res);
                this.$router.push({name: "User"});
                alert("Student edited successfully");
            }).catch(err => {
                let errMsg = err.response.data;
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