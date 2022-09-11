<template>
    <nav>
        <ul>
            <li>
                <router-link to="/" tag="a" class="nav-link"
                    :class="{active: this.$router.currentRoute.name === 'home'}">
                    Home</router-link>
            </li>
            <li>
                <router-link to="/najcitanije" tag="a" class="nav-link"
                    :class="{active: this.$router.currentRoute.name === 'najcitanije'}">
                    Najcitanije</router-link>
            </li>
            <li>
                <div class="menu-item" @click="isOpen = !isOpen">
                    <a href="#">
                        Kategorije
                        <svg viewBox="0 0 1030 638" width="10">
                            <path
                                d="M1017 68L541 626q-11 12-26 12t-26-12L13 68Q-3 49 6 24.5T39 0h952q24 0 33 24.5t-7 43.5z"
                                fill="#FFF"></path>
                        </svg>
                    </a>

                    <transition name="fade" appear>
                        <div class="sub-menu" v-if="isOpen">
                            <div v-for="kat in kategorije" :key="kat.id" class="menu-item">
                                <router-link :to='katPath(kat.id)' tag="a" class="nav-link">{{kat.tip}}
                                </router-link>

                            </div>

                        </div>

                    </transition>
                </div>
            </li>
            <li v-if="this.user.role === 'admin' || this.user.role ==='creator'">
                <router-link to="/vesti" tag="a" class="nav-link"
                    :class="{active: this.$router.currentRoute.name === 'vesti'}">
                    Sve vesti</router-link>
            </li>
            <li v-if="this.user.role === 'admin'">

                <router-link to="/korisnici" tag="a" class="nav-link"
                    :class="{active: this.$router.currentRoute.name === 'korisnici'}">
                    Korisnici</router-link>
            </li>
            <li v-if="this.user.role === 'admin' || user.role === 'creator'">

                <router-link to="/kategorije" tag="a" class="nav-link"
                    :class="{active: this.$router.currentRoute.name === 'korisnici'}">
                    Sve Kategorije</router-link>
            </li>
            <li>
                <input v-model="search" type="text" class="pretraga" placeholder="Search...">
            </li>
            <li>
                <button class="lupa" @click="searchVest">PRETRAZI</button>
            </li>
            <li style="float:right;" v-if="this.user == ''">

                <router-link to="/login" tag="a" class="nav-link"
                    :class="{active: this.$router.currentRoute.name === 'login'}">
                    Login</router-link>
            </li>
            <li class="hello" v-if="this.user.role!=null">
                <p>Hello, {{user.username}}!</p>
            </li>
            <li style="float:right;" v-if="this.user.role!=null">
                <form class="d-flex" @submit.prevent="logout">
                    <button class="btn btn-outline-secondary" type="submit">Logout</button>
                </form>
            </li>
        </ul>
    </nav>
</template>
<script>


export default {
    name: "HomeNavBar",
    data() {
        return {
            user: "",
            kategorije: [],
            isOpen: false,
            search: null,
        };
    },
    mounted() {
        if (localStorage.getItem("jwt") != null) {
            this.user = JSON.parse(atob(localStorage.getItem("jwt").split(".")[1]));
        }
        this.$axios.get("/api/kategorije")
            .then(response => {
            this.kategorije = response.data;
            // console.log(this.categories[1])
        });
    },
    created() {
        this.$axios.get("/api/kategorije")
            .then(response => {
                this.kategorije = response.data;
                // console.log(this.categories[1])
            });
    },
    methods: {
        logout() {
            localStorage.removeItem("jwt");
            this.$router.push({ name: "login" });
        },
        searchVest(){
            this.$router.push('/vesti/search/' + this.search);
        },
        katPath(ime){
            return "/kategorije/" + ime;
        }
    },
    
}


</script>
<style scoped>



.lupa{
    margin-left: 0px;
    padding: 0;
    padding-top: 14px;
    
}

.pretraga{
        outline: 0;
        background: white;
        width: 70%;
        border: 0;
        margin-top: 10px;
        box-sizing: border-box;
        font-size: 21px;
}

.hello{
    float: right;
    color: white;
    font-weight: bold;
    font-size: 11pt;
    
}

 menu-item svg {
     width: 10px;
     margin-left: 10px;
 }

menu-item sub-menu {
     position: absolute;
     background-color: #222;
     top: calc(100% + 18px);
     left: 50%;
     transform: translateX(-50%);
     width: max-content;
     border-radius: 0px 0px 16px 16px;
 }

 fade-enter-active,
 fade-leave-active {
     transition: all .5s ease-out;
 }

 fade-enter,
 fade-leave-to {
     opacity: 0;
 }
    template{
        margin: 0;
    }
        nav{
            
            font-weight: lighter;
        ;
        }

        ul {
            list-style-type: none;
            margin: 0;
            padding: 0;
            padding-left: 200px;
            padding-right: 200px;
            overflow: hidden;
            background-color: #283b55;
        }
    
        li {
            float: left;
        }
    
        li a {
            display: block;
            color: white;
            text-align: center;
            padding: 14px 16px;
            text-decoration: none;
        }
        button{
            display: block;
                color: white;
                text-align: center;
                padding: 14px 16px;
                text-decoration: none;
                background-color: #283b55;
                cursor: pointer;
                border: none;
                font-weight: bold;
                font-family: "Roboto";
        }
        

               
    
        /* Change the link color to #111 (black) on hover */
        li a:hover {
            background-color: #334a69;
        }

        
</style>