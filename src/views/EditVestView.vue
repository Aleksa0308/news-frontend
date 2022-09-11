<template>

    <div v-if="user.role === 'admin' || user.role === 'creator'">

        <div class="str">
            <HomeNavBar />
            <div class="single" id="single">
                <form @submit.prevent="editVest">
                    <div>
                        <p>Naslov:</p>
                        <input v-model="form.naslov" type="text" class="ime" id="formIme" />
                        <p>Sadrzaj:</p>
                        <input v-model="form.tekst" type="text" class="text" id="formTekst" />
                        <p>Broj poseta:</p>
                        <input v-model="form.brPoseta" type="text" class="text" id="formBrPoseta" />
                        <p>Kategorija:</p>
                        <select v-model="form.kategorija" class="text" id="kategorija">
                            <option v-for="kat in kategorije" :key="kat.id" :value="kat.id">{{kat.tip}}</option>
                        </select>
                        <br>
                        <br>
                        <button>Izmeni</button>
                    </div>
                </form>
            </div>
        </div>
    </div>
    <div v-else-if="this.user.role === 'user'">
        <ErrorItem />
    </div>
</template>
<script>
import VestItem from "../components/Vest";
import HomeNavBar from "@/components/HomeNavBar.vue";
import ErrorItem from "@/components/ErrorItem.vue";
export default {
    name: "EditVest",
    // eslint-disable-next-line
    components: { VestItem, HomeNavBar, ErrorItem },
    data() {
        return {
            user: JSON.parse(atob(localStorage.getItem('jwt').split('.')[1])),
            vest: {},
            vestKomentari: [],
            kategorije: [],
            form: {
                id: this.$route.params.id,
                naslov: null,
                tekst: null,
                brPoseta: null,
                kategorija: null,
                datum: null,
            }
        }
    },
    created() {
        this.$axios.get(`/api/vesti/${this.$route.params.id}`).then((response) => {
            this.vest = response.data;
            this.form.naslov = this.vest.naslov;
            this.form.tekst = this.vest.tekst;
            this.form.brPoseta = this.vest.brPoseta;
            this.form.kategorija = this.vest.kategorijaId;
        });
        this.$axios.get(`/api/kategorije`)
            .then(response => {
                this.kategorije = response.data
            });
    }, methods: {
        
        editVest() {
            this.$axios.put('/api/vesti/', {
                id: this.vest.id,
                kategorijaId: this.form.kategorija,
                naslov: this.form.naslov,
                tekst: this.form.tekst,
                brPoseta: this.form.brPoseta
            }),
            this.$router.push('/vesti/' + this.vest.id);
        }
    }
}

</script>

<style scoped>
p{
    font-weight: bold;
    font-size: 21pt;
}
.text {
    width: 100%;
    height: 50px;
}

.kom {
    padding-bottom: 10px;
    box-shadow: 13px 15px 0px -6px rgba(0, 0, 0, 0.62);
}

.single {
    margin-left: 21%;
    margin-right: 21%;
    text-align: justify;
}

.naslov {
    font-weight: bold;
    font-size: 24pt;
}

.tekst {
    font-size: 14pt;

}

.datum {
    font-style: italic;
}

.autor {
    font-weight: bold;
}

span {
    font-weight: normal;
}
#kategorija{
    font-size: 21px;
    background-color: #dfdfdf;
    width: 15%;
    margin-bottom: 20px;
    text-align: center;

}
input {
    display: block;
    outline: 0;
    background: #dfdfdf;
    width: 50%;
    border: 0;
    margin: 0 0 15px;
    padding: 15px;
    box-sizing: border-box;
    font-size: 21px;
}
#formBrPoseta{
    width: 7%;
    text-align: center;
}

button {
    font-family: "Roboto", sans-serif;
    text-transform: uppercase;
    outline: 0;
    background: #283b55;
    width: 20%;
    border: 0;
    padding: 15px;
    color: #FFFFFF;
    font-size: 14px;
    -webkit-transition: all 0.3 ease;
    transition: all 0.3 ease;
    cursor: pointer;
}

button:hover,
button:active,
button:focus {
    background: #384f70;
}
</style>