<template>
    <div>
        <Mensagem :msg="msg" v-show="msg" />
        <div>
            <form id="sand-form" @submit="criarSand">
                <div class="input-container">
                    <label for="nome">Seu Nome</label>
                    <input type="text" id="nome" name="nome"
                    v-model="nome" placeholder="Seu nome">
                </div>

                <div class="input-container">
                    <label for="pao">Escolha seu pão preferido:</label>
                    <select name="pao" id="pao" v-model="pao">
                        <option value="">Selecione seu pão</option>
                        <option v-for="pao in paes" :key="pao.id" :value="pao.tipo">
                            {{pao.tipo}}
                        </option>
                    </select>  
                </div>

                <div class="input-container">
                    <label for="carne">Escolha o tipo de carne :</label>
                    <select name="carne" id="carne" v-model="carne">
                        <option value="">Selecione sua carne</option>
                        <option v-for="carne in carnes" :key="carne.id" :value="carne.tipo">
                            {{carne.tipo}}
                        </option>
                    </select>  
                </div>

                <div id="opcionais-container" class="input-container">
                    <label for="opcionais-title">Escolha os complementos:</label>
                    <div class="checkbox-container" v-for="opcional in opcionaisdata" :key="opcional.id">
                        <input type="checkbox" name="opcionais" v-model="opcionais" :value="opcional.tipo">
                        <span>{{opcional.tipo}}</span>
                    </div> 
                </div>

                <div id="molhos-container" class="input-container">
                    <label for="molho-title">Escolha seu molho:</label>
                    <div class="checkbox-container" v-for="molho in molhosdata" :key="molho.id" >
                        <input type="checkbox" name="molhos" v-model="molhos" :value="molho.tipo">
                        <span>{{molho.tipo}}</span>
                    </div> 
                </div>

                <div class="input-container">
                    <input type="submit" class="submit-btn" value="Montar sanduba">
                </div>
            </form>
        </div>
    </div>
</template>

<script>
    import Mensagem from './Mensagem.vue'
    export default {
        name: "SandForm",
        data(){
            return{
                paes: null,
                carnes: null,
                opcionaisdata: null,
                molhosdata: null,
                nome: null,
                pao: null,
                carne: null,
                opcionais: [],
                molhos: [],
                msg: null
            }
        },
        methods: {
            async Ingredientes(){
                const requis = await fetch("http://localhost:3000/ingredientes")
                const data = await requis.json();

                this.paes = data.paes;
                this.carnes = data.carnes;
                this.opcionaisdata = data.opcionais;
                this.molhosdata = data.molhos;
            },
            async criarSand(e){
                e.preventDefault(); //fazer que o evento pare de ser executado
                
                const data = {
                    nome: this.nome,
                    carne: this.carne,
                    pao: this.pao,
                    opcionais: Array.from(this.opcionais),
                    molhos: Array.from(this.molhos),
                    status: "Solicitou"
                }

                const dataJson = JSON.stringify(data) //transformando em texto

                //fazendo a requisicao para o banco
                const req = await fetch("http://localhost:3000/sandwiches", {
                    method: "POST",
                    headers: {"Content-Type": "application/json"},
                    body: dataJson
                });

                const res = await req.json();
                
                //mensagem no sistema
                this.msg = 'Sanduba montado com sucesso!'

                //limpar msg
                setTimeout(()=> this.msg = "", 2000)


                //limpando campos
                this.nome = "",
                this.carne = "",
                this.pao= "",
                this.opcionais = "",
                this.molhos = ""

            }
        },
        mounted(){
            this.Ingredientes()
        },
        components: {
            Mensagem
        }
    }
</script>


<style scoped>
    #sand-form {
        max-width: 400px;
        margin: 0 auto;
    }

    .input-container {
        display: flex;
        flex-direction: column;
        margin-bottom: 20px;
    }

    label {
        font-weight: bold;
        margin-bottom: 15px;
        color: black;
        padding: 5px 10px;
        border: 2px solid orange;
    }

    input, select {
        padding: 5px 10px;
        width: 300px;
    }

    #opcionais-container {
        flex-direction: row;
        flex-wrap: wrap;
    }

    #molhos-container {
        flex-direction: row;
        flex-wrap: wrap;
    }

    #opcionais-title {
        width: 100%;
    }

    #molho-title {
        width: 100%;
    }

    .checkbox-container {
        display: flex;
        align-items: flex-start;
        width: 50%;
        margin-bottom: 20px;
    }

    .checkbox-container span,
    .checkbox-container input {
        width: auto;
    }

    .checkbox-container span {
        margin-left: 6px;
        font-weight: bold;
    }

    .submit-btn {
        backdrop-filter: black;
        color: orange;
        font-weight: bold;
        border: 2px solid #222;
        padding: 10px;
        font-size: 15px;
        margin: 0 auto;
        cursor: pointer;
        transition: .5s;
    }

    .submit-btn:hover{
        background-color: transparent;
        color: #222;
    }
</style>