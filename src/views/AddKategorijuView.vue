<template>

    <div v-if="user.role === 'admin' || user.role === 'creator'">

        <div class="str">
            <HomeNavBar />
            <div class="single">
                <form @submit.prevent="addUser">
                    <br>
                    <div>
                        <p>Tip:</p>
                        <input v-model="form.tip" type="text" class="ime" id="formIme" />
                        <p>Opis:</p>
                        <input v-model="form.opis" type="text" class="text" id="formBrTekst" />
                        <br>
                        <br>
                        <button>Dodaj</button>
                    </div>
                </form>
            </div>
            <br>
        </div>
    </div>
    <div v-else-if="this.user.role === 'user'">
        <ErrorItem />
    </div>
</template>
<script>

import HomeNavBar from "@/components/HomeNavBar.vue";
import ErrorItem from "@/components/ErrorItem.vue";
export default {
    name: "AddUser",
    // eslint-disable-next-line
    components: { HomeNavBar, ErrorItem },
    data() {
        return {
            user: JSON.parse(atob(localStorage.getItem('jwt').split('.')[1])),
            korisnik: {},
            kategorije: [],
            form: {
                id: null,
                tip: null,
                opis: null,
            }
        }
    },
    created() {

    }, methods: {

        addUser() {
            this.$axios.post('/api/kategorije', {
                tip: this.form.tip,
                opis: this.form.opis,
                
            }).then(response => {
                if (response.status === 200) {
                    alert('Kategorija uspesno dodata!')
                    this.$router.push('/kategorije')
                } else {
                    alert(response.data.message);
                }
            })
        }
    }
}

</script>

<style scoped>
p {
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
    font-size: 36pt;
}

.tekst {
    font-size: 24pt;

}

.datum {
    font-style: italic;
}

.komentari {
    margin-left: 21%;
    margin-right: 21%;
    text-align: justify;

}

.autor {
    font-weight: bold;
}

span {
    font-weight: normal;
}

#kategorija {
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

#formBrPoseta {
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