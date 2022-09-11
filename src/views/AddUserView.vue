<template>

    <div v-if="user.role === 'admin'">

        <div class="str">
            <HomeNavBar />
            <div class="single">
                <form @submit.prevent="addUser">
                    <br>
                    <div>

                        <p>Username:</p>
                        <input v-model="form.username" type="text" class="ime" id="formIme" />
                        <p>Email:</p>
                        <input v-model="form.email" type="text" class="text" id="formBrTekst" />
                        <p>Password:</p>
                        <input v-model="form.password" type="password" class="ime" id="formIme" />
                        <p>Role:</p>
                        <select v-model="form.role" class="text" id="kategorija">
                            <option value="user">user</option>
                            <option value="creator">creator</option>
                            <option value="admin">admin</option>
                        </select>
                        <p>Status:</p>
                        <select v-model="form.status" class="text" id="kategorija">
                            <option value="active">active</option>
                            <option value="deactivated">deactivated</option>
                        </select>
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
                username: null,
                password: null,
                email: null,
                role: 'user',
                status: 'active',
            }
        }
    },
    created() {
        
    }, methods: {

        addUser() {
            this.$axios.post('/api/users', {
                username: this.form.username,
                email: this.form.email,
                role: this.form.role,
                status: this.form.status,
                hashedPassword: this.form.password
            }).then(response => {
                if (response.status === 200) {
                    alert('Korisnik uspesno dodat!')
                    this.$router.push('/korisnici')
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