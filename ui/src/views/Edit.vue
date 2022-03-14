<template>
    <div>
        <div v-if="error != undefined">
            <p>{{error}}</p>
        </div>
        <h2>Student Edit</h2>
        <hr>
        <v-text-field v-model="ra" label="RA" disabled ></v-text-field>
        <v-text-field v-model="name" label="Student Name" clearable ></v-text-field>
        <v-text-field v-model="email" label="Student Email" clearable ></v-text-field>
        <v-text-field v-model="cpf" label="Student CPF" disabled></v-text-field>
        <v-btn color='secondary' small @click="update">Edit</v-btn>
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