<template>
    <div>
        <Message :msg="msg" v-show="msg"/>
        <div>
            <form id="form-burger" @submit="createBurger">
                <div class="input-container">
                    <label for="nome">Nome do Cliente:</label>
                    <input type="text" name="nome" id="nome-cliente" v-model="nome_cliente" placeholder="Digite seu nome">
                </div>
                <div class="input-container">
                    <label for="pao">Escolha seu pão:</label>
                    <select name="pao" id="pao" v-model="pao">
                        <option value="">Selecione seu pão</option>
                        <option v-for="pao in paes" :key="pao.id" :value="pao.tipo">{{pao.tipo}}</option>
                    </select>
                </div>
                <div class="input-container">
                    <label for="carne">Escolha sua carne:</label>
                    <select name="carne" id="carne" v-model="carne">
                        <option value="">Selecione sua Carne</option>
                        <option v-for="carne in carnes" :key="carne.id" :value="carne.tipo">{{carne.tipo}}</option>
                    </select>
                </div>
                <div class="input-container checkbox">
                    <p>Selecione os opcionais</p>
                    <div class="all-checkbox">
                        <div class="checkbox_container" v-for="op in opcionaisdata" :key="op.id">
                            <input type="checkbox" :name="op.tipo" :id="op.tipo" v-model="opcionais" :value="op.tipo">
                            <label :for="op.tipo">{{op.tipo}}</label>                        
                        </div>


                    </div>
                </div>
                <div class="input-container">
                    <input type="submit" value="Criar meu Burger" class="submit-btn">
                </div>
            </form>
        </div>
    </div>
</template>
<script>
import Message from './Message.vue'

export default {
    name:'BurgerForm',
    data(){
        return{
            paes: null,
            carnes: null,
            opcionaisdata: null,
            nome_cliente: null,
            pao: null,
            carne: null,
            opcionais: [],
            status: 'Solicitado',
            msg: null
        }
    },
    components:{
        Message
    },
    methods:{
        async getIngredients(){
            const req = await fetch("http://localhost:3000/ingredientes");
            const data = await req.json();
            this.paes = data.paes;
            this.carnes = data.carnes;
            this.opcionaisdata = data.opcionais;
        },
        async createBurger(e){
            e.preventDefault();
            const data = {
                nome: this.nome_cliente,
                carne: this.carne,
                pao: this.pao,
                opcionais: Array.from(this.opcionais),
                status: "solicitado"
            }

            const dataJson = JSON.stringify(data);

            const req = await fetch("http://localhost:3000/burgers", {
                method: "POST", 
                headers: {"Content-Type" : "application/Json"},
                body: dataJson
            })

            const res = await req.json();


            this.nome_cliente="";
            this.carne="";
            this.pao="";
            this.opcionais=[];

            const req_burgers = await fetch("http://localhost:3000/burgers");
            const data_burgers = await req_burgers.json();
            //console.log(data_burgers)
            this.msg = `Pedido Nº${data_burgers.length} realizado com sucesso!`

            setTimeout(()=>this.msg="", 3000);
        },
    },
    mounted(){
        this.getIngredients();
    }
}
</script>
<style>
    #form-burger{
        max-width: 400px;
        margin: 50px auto;
    }
    .input-container{
        margin-top: 20px;
        margin-bottom: 30px;
        display: flex;
        justify-content: space-between;
    }
    .checkbox{
        display: flex;
        margin-top: 10px;
        flex-direction: column !important;
    }
    .checkbox_container>label{
        margin-left: 10px;
    }
    .checkbox_container{
        display: flex;
        justify-content: flex-start;
        align-items: center;
        width: 120px;
        margin:5px 20px;
    }
    .all-checkbox{
        display:flex;
        flex-wrap: wrap;
        justify-content: space-around;
    }
    select,
    input[type=text]{
        width: 60%;
        padding: 5px;
        border:2px solid #222;
        border-radius: 3px;
    }
    .submit-btn{
        width: 100%;
        padding:10px 0;
        transition: 0.5s;
        border: 2px solid #222;
        color: #FCBA07;
        font-weight: bold;
        cursor: pointer;
        font-size: 1.1rem;
        background: #222;    
    }
    .submit-btn:hover{
        background: transparent;
        color: #222;
    }
    .input-container>label,
    .input-container>p{
        font-weight: bold;
        border-left:4px solid #FCBA07;
        padding:5px;
    }
</style>