<template>
  <div class="home">
    <HomeNavBar />
    <h1>Izdvajamo:</h1>
    <div class="vesti">
      <div v-for="vest in vesti" :key="vest.id" class="kartica">
        <p class="Naslov">{{ vest.naslov }}</p>
        <p class="Tekst">{{ vest.tekst | shortText}}</p>
        <p class="br">Broj pregleda: {{ vest.brPoseta }}</p>
        <p class="Datum">Datum: {{ new Date(vest.vremeKreiranja).toLocaleString() }}</p>

        <button class="btn1" @click="selectedVest = vest; openVest(vest.id)">Procitaj</button>
        <button class="btn1" id="btn2" v-if="user.role === 'admin' || user.role === 'creator'"
          @click="selectedVest = vest; editVest(vest.id)">Izmeni</button>
        <button class="btn1" id="btn3" v-if="user.role === 'admin' || user.role === 'creator'"
          @click="selectedVest = vest; deleteVest(vest.id)">Obrisi</button>

      </div>
      <div>
        <vest v-if="selectedVest" :vest="selectedVest"></vest>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import HomeNavBar from '@/components/HomeNavBar.vue';
import VestItem from "@/components/Vest";

export default {
  name: 'HomeView',
  // eslint-disable-next-line 
  components:{ HomeNavBar, VestItem},
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
      user: "",
      selectedVest: null,
      vesti: []
    }
  },
  created() {
    this.$axios.get('/api/vesti/top').then((response) => {
      this.vesti = response.data;
    });
    
  }, methods: {
    openVest(id) {
      this.$router.push('/vesti/' + id);
    },
    deleteVest(id){
      this.$axios.delete(`/api/vesti/${id}`, {
        headers: {
          Authorization: "Bearer " + localStorage.getItem("jwt")
        }
      })
        .then(() => {
          this.vesti = this.vesti.filter(vest => vest.id !== id)
        })
        .catch((err) => {
          console.log(err);
        })
    },
    editVest(id){
      this.$router.push('/izmeni/' + id);
    }
  },
  mounted() {
    if (localStorage.getItem("jwt") != null) {
      this.user = JSON.parse(atob(localStorage.getItem("jwt").split(".")[1]));
    }
  },
  
}
</script>
<style scoped>
h1{
  font-size: 36pt;
  margin: 50px;
  
}
p {
  text-align: justify;
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
.btn1:focus
 {
  background: #384f70;
}
#btn2{
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
