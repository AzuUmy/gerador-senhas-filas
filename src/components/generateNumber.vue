<template>
    <section>
        <div><h1>{{ time }}</h1></div>
        <div>
         
            <h1>Senha {{ senha }}</h1>
            <h1>Preferencial {{ preferencial }}</h1>
           <button @click="generateNumberAndPushData" >Gerar Senha</button>
           <button @click="generatePreferencialAndPush">Gerar Preferencial</button>
        </div>
    </section>
</template>


<script>
import axios from 'axios';


export default {
    data(){
        return {
            senha: 0,
            time: '',
            preferencial: 0,
            indetifyer: 'someInfo'
        }
    },

    methods: {

        updateTime(){
            const now = new Date();
            this.time = now.toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' });
        },

        generateNumber(){
            this.senha +=1;
            this.indetifyer = "Regular"
        },

        generatePreferencial(){
            this.preferencial +=1;
            this.indetifyer = "Preferencial";
            //console.log(this.indetifyer);
        },

        async pushQueryToData(){
            try {
                const response = await axios.post('http://localhost:8080/senha', {
                    senha: this.senha,
                    time: this.time,
                    info: this.indetifyer
        });
            } catch (error) {
                console.error('Error:', error.response ? error.response.data : error.message);
            }
        },

        async pushPrefToDate(){
            try {
                const response = await axios.post('http://localhost:8080/prefe', {
                    senha: this.preferencial,
                    time: this.time,
                    info: this.indetifyer
                });
            } catch (error){
                console.error('Error:', error.response ? error.response.data : error.message);
            }
        },

        generateNumberAndPushData(){
            this.generateNumber();
            this.pushQueryToData();
        },

        generatePreferencialAndPush(){
            this.generatePreferencial();
            this.pushPrefToDate();
        }
    },



    mounted() {
        this.updateTime;
        setInterval(this.updateTime, 1000);
    },

}
</script>