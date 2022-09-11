<template>

    <div v-if="user.role === 'admin'">
        <HomeNavBar />
        <div class="vesti">
            <h1>Svi Korisnici</h1>

            <button class="btn1" id="btn4" @click="openAdd()">Dodaj Korisnika</button>
            <br><br>
            <div v-for="kor in korisnici" :key="kor.id" class="kartica">
                <h2>{{kor.username}}</h2>
                <h3>Email: {{kor.email}}</h3>
                <h3>Role: {{kor.role}}</h3>
                <button class="btn1" id="btn2" v-if="user.role === 'admin'"
                    @click="selectedUser = kor; editUser(kor.username)">Izmeni</button>
                <button class="btn1" id="btn3" v-if="user.role === 'admin'"
                    @click="selectedUser = kor; deleteUser(kor.id)">Obrisi</button>
            </div>
        </div>
    </div>
    <div v-else-if="user.role === 'user' || user.role === 'creator'">
        <ErrorItem />
    </div>

</template>
<script>


import ErrorItem from "../components/ErrorItem.vue";
import HomeNavBar from "@/components/HomeNavBar.vue";



export default {
    // eslint-disable-next-line
    components: { ErrorItem, HomeNavBar },
    filters: {
        shortText(value) {
            if (value.length < 30) {
                return value;
            }
            return value.slice(0, 30) + '...'
        }
    },
    data() {
        return {
            user: JSON.parse(atob(localStorage.getItem('jwt').split('.')[1])),
            selectedUser: null,
            korisnici: [],
        }
    },
    created() {
        this.$axios.get('/api/users/').then((response) => {
            this.korisnici = response.data;
        });
    }, methods: {
        
        deleteUser(id) {

            var answer = window.confirm("Da li ste sigurni da zelite da obrisete korisnika?");
            if (answer) {
                this.$axios.delete(`/api/users/delete/${id}`, {
                    headers: {
                        Authorization: "Bearer " + localStorage.getItem("jwt")
                    }
                })
                    .then(() => {
                        alert("Korisnik uspesno obrisan!");
                        this.users = this.users.filter(user => user.id !== id)
                        
                    })
                    .catch((err) => {
                        console.log(err);
                    })
            }
            else {
                //some code
            }    
            this.$router.push('/korisnici');
        },
        editUser(username) {
            this.$router.push('/izmeniUser/' + username);
        },openAdd(){
            this.$router.push('/addUser');
        }
    },
}


</script>

<style scoped>
h1 {
    font-size: 36pt;
    margin-right: 100px;
    margin-left: 600px;
    margin-top: 30px;
    text-align: center;
    font-weight: bold;
}

p {
    text-align: justify;
}
#btn4{
    margin-top: 35px;
    height: 50px;
}
.Naslov {
    font-size: 36pt;
    font-weight: bold;
}

.Tekst {
    font-style: italic;
    font-size: 16pt;
}

.Datum {
    font-style: oblique;
}

.vesti {
    margin: 7%;
    margin-top: 0;
    display: flex;
    flex-wrap: wrap;
}

.btn1 {
    font-size: 15px;
    margin: 10px;
    padding: 10px 30px;
    border-width: 0px;
    color: #e9e9e9;
    font-weight: bold;

    cursor: pointer;
    background: #283b55;
}

.btn1:hover,
.btn1:active,
.btn1:focus {
    background: #384f70;
}

#btn2 {
    background-color: rgb(184, 122, 255);
    color: white;
}

#btn3 {
    background-color: rgb(212, 49, 49);
    color: white;
}

.kartica {
    text-align: justify;
    padding: 35px;
    margin: 15px;
    width: 420px;


    /*box-shadow: 10px 10px 73px -53px rgba(0, 0, 0, 0.50);*/
    box-shadow: 13px 15px 0px -6px rgba(0, 0, 0, 0.62);
}
</style>
