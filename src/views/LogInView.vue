<template>
    <div class="login-page">
        <div class="card">
            <h1>Welcome, please login!</h1>
            <form @submit.prevent="login">
                <div>
                    <input v-model="username" type="text" placeholder="username" />
                    <input v-model="password" type="password" placeholder="password" />
                    <button>LOGIN</button>
                </div>
            </form>
            <a href="\">Go to home page!</a>
        </div>
    </div>
</template>
<script>
/* eslint-disable */
export default {
    name: "Login",
    data() {
        return {
            username: '',
            password: '',
        }
    },
    methods: {
        login() {
            this.$axios.post('/api/users/login', {
                username: this.username,
                password: this.password,
            }).then(response => {
                if (response.status === 200) {
                    let user = this.parseJwt(response.data.jwt)
                    if (user.status !== "active") {
                        alert("Korisnik deaktiviran!")
                        return;
                    }
                }  
                
                localStorage.setItem('jwt', response.data.jwt)
                
                this.$router.push({ name: 'home' });
            }).catch(err => {
                alert("Username or password are incorrent!")
                confirm.log(err)
            })
        },
        parseJwt(token) {
            var base64Url = token.split('.')[1];
            var base64 = base64Url.replace(/-/g, '+').replace(/_/g, '/');
            var jsonPayload = decodeURIComponent(window.atob(base64).split('').map(function (c) {
                return '%' + ('00' + c.charCodeAt(0).toString(16)).slice(-2);
            }).join(''));
            return JSON.parse(jsonPayload);
        }
    },
}
</script>
<style scoped>



        @import url(https://fonts.googleapis.com/css?family=Roboto:300);
    
        .login-page {
            
            width: 420px;
            padding: 8% 0 0;
            margin: auto;
        }
    
        .card {
            position: relative;
            z-index: 1;
            background: #f7f7f7;
            max-width: 360px;
            margin: 0 auto 100px;
            padding: 45px;
            text-align: center;
            /*box-shadow: 0 0 20px 0 rgba(0, 0, 0, 0.2), 0 5px 5px 0 rgba(0, 0, 0, 0.24);*/
            box-shadow: 13px 15px 0px -6px rgba(0, 0, 0, 0.62);
        }
    
         input {
            
            outline: 0;
            background: #dfdfdf;
            width: 100%;
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
            width: 100%;
            border: 0;
            padding: 15px;
            color: #FFFFFF;
            font-size: 14px;
            -webkit-transition: all 0.3 ease;
            transition: all 0.3 ease;
            cursor: pointer;
            margin-bottom: 20px;
        }
        a{
            color: grey;
        }
    
         button:hover,
         button:active,
         button:focus {
            background: #3a5170;
        }
    
        .message {
            margin: 15px 0 0;
            color: #b3b3b3;
            font-size: 12px;
        }
    
         .message a {
            color: #3d7fca;
            text-decoration: none;
        }
    
         .register-form {
            display: none;
        }
    
        .container {
            position: relative;
            z-index: 1;
            max-width: 300px;
            margin: 0 auto;
        }
    
        .container:before,
        .container:after {
            content: "";
            display: block;
            clear: both;
        }
    
        .container .info {
            margin: 50px auto;
            text-align: center;
        }
    
        .container .info h1 {
            margin: 0 0 15px;
            padding: 0;
            font-size: 36px;
            font-weight: 300;
            color: #1a1a1a;
        }
    
        .container .info span {
            color: #4d4d4d;
            font-size: 12px;
        }
    
        .container .info span a {
            color: #000000;
            text-decoration: none;
        }
    
        .container .info span .fa {
            color: #EF3B3A;
        }
</style>