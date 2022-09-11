<template>
    <div class="str">
        <HomeNavBar />
        <div class="single">
            <p class="naslov">{{vest.naslov}}</p>
            <p class="tekst">{{vest.tekst}}</p>
            <p class="pregledi">Broj pregleda: {{vest.brPoseta}}</p>
            <br>
            <p class="datum">Autor: {{vest.autor}}</p>
            <p class="datum">Datum: {{ new Date(vest.vremeKreiranja).toLocaleString() }}</p>
        </div>
        <br>
        <div class="komentari">
            <hr>
            <h1>Dodaj komentar:</h1>
            <form @submit.prevent="onSubmit">
                <div>
                    <input v-model="form.ime" type="text" placeholder="Ime" class="ime" id="formIme"/>
                    <input v-model="form.tekst" type="text" placeholder="Komentar" class="text" id="formTekst"/>
                    <button>POSTAVI</button>
                </div>
            </form>
            <h1>Komentari:</h1>
            <div>
            </div>
            <div class="kom">
                <div v-for="komentar in vestKomentari" :key="komentar.id">
                    <p class="autor">{{ komentar.ime }}: <span>{{komentar.tekst}}</span></p>
                    <p class="datum">Vreme: {{ new Date(komentar.datumKreiranja).toLocaleString() }}</p>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
import VestItem from "../components/Vest";
import HomeNavBar from "@/components/HomeNavBar.vue";
export default {
  name: "SingleVest",
  // eslint-disable-next-line
  components: {VestItem, HomeNavBar},
    data() {
        return {
            //user: JSON.parse(atob(localStorage.getItem('jwt').split('.')[1])),
            vest: {}, 
            vestKomentari: [],
            form: {
                vestId: this.$route.params.id,
                ime: null,
                tekst: null
            },count : 0,
        }
    },
    created() {
        this.$axios.get(`/api/vesti/${this.$route.params.id}`).then((response) => {
            this.vest = response.data;
        });
        this.$axios.get(`/api/vesti/${this.$route.params.id}/komentari`)
            .then(response => {
                this.vestKomentari = response.data
            });
    },methods: {
        onSubmit(event) {
            event.preventDefault()
            this.$axios.post('/api/komentari', this.form)
                .then((response) => {
                    if (response.status === 200) {
                        this.vestKomentari.push(response.data)
                        this.form.ime = null
                        this.form.tekst = null
                    }
                })
                .catch((err) => {
                    console.log(err)
                })
        }
    }, mounted(){
        if(this.count==0){
            this.$axios.post(`/api/vesti/poseta/${this.$route.params.id}`);
            this.count = 1;
        }
        
    }
}

</script>

<style scoped>


.text{
    width: 100%;
    height:50px;
}
.kom{
    padding-bottom: 10px;
    box-shadow: 13px 15px 0px -6px rgba(0, 0, 0, 0.62);
}
.single{
    margin-left: 21%;
    margin-right: 21%;
    text-align: justify;
}
.naslov{
    font-weight: bold;
    font-size: 36pt;
}
.tekst{
    font-size: 24pt;

}
.datum{
    font-style: italic;
}
.komentari{
    margin-left: 21%;
    margin-right: 21%;
    text-align: justify;
    
}
.autor{
    font-weight: bold;
}
span{
    font-weight: normal;
}

input {
    display: block;
    outline: 0;
    background: #dfdfdf;
    width: 20%;
    border: 0;
    margin: 0 0 15px;
    padding: 15px;
    box-sizing: border-box;
    font-size: 14px;
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