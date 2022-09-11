<template>

    <div v-if="user.role === 'admin' || user.role === 'creator'">
        <HomeNavBar />
        <div class="vesti">
            <h1>Sve Kategorije</h1>

            <button class="btn1" id="btn4" @click="openAdd()">Dodaj Kategoriju</button>
            <br><br>
            <div v-for="kat in kategorije" :key="kat.id" class="kartica">
                <h2>{{ kat.tip }}</h2>
                <h3>Opis: {{ kat.opis }}</h3>
                <button class="btn1" id="btn2" v-if="user.role === 'admin' || user.role === 'creator'"
                    @click="selectedKategorija = kat; editKat(kat.id)">Izmeni</button>
                <button class="btn1" id="btn3" v-if="user.role === 'admin' || user.role === 'creator'"
                    @click="selectedKategorija = kat; deleteKat(kat.id)">Obrisi</button>
            </div>
        </div>
    </div>
    <div v-else-if="user.role === 'user'">
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
            selectedKategorija: null,
            kategorije: [],
        }
    },
    created() {
        this.$axios.get('/api/kategorije/').then((response) => {
            this.kategorije = response.data;
        });
    }, methods: {

        deleteKat(id) {

            var answer = window.confirm("Da li ste sigurni da zelite da obrisete kategoriju?");
            if (answer) {
                this.$axios.delete(`/api/kategorije/${id}`, {
                    headers: {
                        Authorization: "Bearer " + localStorage.getItem("jwt")
                    }
                })
                    .then(() => {
                        alert("Kategorija uspesno obrisana!");
                        this.kategorije = this.kategorije.filter(kat => kat.id !== id)

                    })
                    .catch((err) => {
                        console.log(err);
                    })
            }
            else {
                //some code
            }
            this.$router.push('/kategorije');
        },
        editKat(id) {
            this.$router.push('/editKategoriju/' + id);
        }, openAdd() {
            this.$router.push('/addKategoriju');
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

#btn4 {
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
