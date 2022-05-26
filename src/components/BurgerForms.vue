<template>
    <div>
        <Message :msg="msg" v-show="msg"/>
        <div>
            <form id="burgerForm" @submit="createBuger">
                <div class="inputContainer">
                    <label for="nome">Nome do Cliente: </label>
                    <input type="text" id="nome" name="nome" v-model="nome" placeholder="Digite seu nome">
                </div>
                <div class="inputContainer">
                    <label for="pao">Escolha o pão:</label>
                    <select name="pao" id="pao" v-model="pao">
                        <option value="">Selecione seu pão</option>
                        <option v-for="pao in paes" :key="pao.id" :value="pao.tipo">{{ pao.tipo}}</option>
                    </select>
                </div>
                <div class="inputContainer">
                    <label for="carne">Escolha a carne do seu burger:</label>
                    <select name="carne" id="carne" v-model="carne">
                        <option value="">Selecione sua carne</option>
                        <option v-for="carne in carnes" :key="carne.id" :value="carne.tipo">{{ carne.tipo}}</option>
                    </select>
                </div>
                <div id="opcionalContainer" class="inputContainer">
                    <label id="opcionalTitle"  for="opcional">Selecione os opcionais:</label>
                    <div class="checkboxContainer" v-for="opcional in opcionaisdata" :key="opcional.id">
                        <input type="checkbox" name="opcional" v-model="opcionais" :value="opcional.tipo">
                        <span>{{opcional.tipo}}</span>
                    </div>
                </div>
                <div class="inputContainer">
                    <input type="submit" class="submitBtn" value="Criar meu burger">
                </div>
                
            </form>
        </div>
    </div>
</template>

<script>
    import Message from "./Message.vue"

export default{
    name: "BurgerForms",
    components:{
        Message
    },
    data(){
        return{
            paes: null,
            carnes: null,
            opcionaisdata: null,
            nome: null,
            pao: null,
            carne: null,
            opcionais: [],
            msg: null

        }
    },
    methods:{
        async getIngredientes(){
            const req = await fetch("http://localhost:3000/ingredientes");
            const data = await req.json();

            this.paes = data.paes;
            this.carnes = data.carnes;
            this.opcionaisdata = data.opcionais;
        },
        async createBuger(e){
            e.preventDefault();


            const data = {
                nome: this.nome,
                carne: this.carne,
                pao: this.pao,
                opcionais: Array.from(this.opcionais),
                status: "Solicitado"
            }
            
            const dataJson = JSON.stringify(data);
            const req = await fetch("http://localhost:3000/burgers",{
                method: "POST",
                headers: {"Content-Type": "application/json"},
                body: dataJson
            });

            const res = await req.json();

            //Colocar mensagem no sistema
            this.msg = `Pedido nº ${res.id} Realizado com sucesso`;

            //limpar menssagem
            setTimeout(() => this.msg = "", 3000)


            //limpar os campos
            this.nome = "";
            this.carne = "";
            this.pao= "";
            this.opcionais= "";
        }
    },
    mounted(){
        this.getIngredientes()
    }
}
</script>

<style scoped>
    #burgerForm{
        max-width: 400px;
        margin: 0 auto;
    }
    .inputContainer{
        display: flex;
        flex-direction: column;
        margin-bottom: 20px;
    }
    label{
        font-weight: bold;
        margin-bottom: 15px;
        color: #222;
        padding: 5px 10px;
        border-left: 4px solid #FCBA03;
    }
    input, select{
        padding: 5px 10px;
        width: 300px;
    }
    #opcionalContainer{
        flex-direction: row;
        flex-wrap: wrap;
    }
    #opcionalTitle{
        width: 100%;
    }
    .checkboxContainer{
        display: flex;
        align-items: flex-start;
        width: 50%;
        margin-bottom: 20px;
    }
    .checkboxContainer input, .checkboxContainer span{
        width: auto;
    }
    .checkboxContainer span{
        margin-left: 6px;
        font-weight: bold;
    }
    .submitBtn{
        background-color: #222;
        color: #FCBA03;
        font-weight: bold;
        border: 2px solid #222;
        padding: 10px;
        font-size: 16px;
        margin: 0 auto;
        cursor: pointer;
        transition: 0.5s;
    }
    .submitBtn:hover{
        background-color: transparent;
    }
</style>
