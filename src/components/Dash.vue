<template>
    <div id="sandw-table">
        <Mensagem :msg="msg" v-show="msg" />
        <div id="sandw-table-head">
            <div class="order-id">N°.</div>
            <div>Cliente:</div>
            <div>Pão:</div>
            <div>Carne:</div>
            <div>Opcionais:</div>
            <div>Molhos:</div>
            <div>Ações:</div>         
        </div>
    </div>
    
    <div id="sandw-table-linhas">
        <div class="sandw-table-linha" v-for="sandwiche in sandwiches" 
        :key="sandwiche.id" >
            <div class="order-number">{{ sandwiche.id}}</div>
            <div>{{ sandwiche.nome}}</div>
            <div>{{ sandwiche.pao}}</div>
            <div>{{ sandwiche.carne}}</div>
            <div>
                <ul>
                    <li v-for="(opcional, index) in sandwiche.opcionais"
                    :key="index">{{opcional}}</li>
                </ul>
            </div>
            <div>
                <ul>
                    <li v-for="(molho, index) in sandwiche.molhos"
                    :key="index">{{molho}}</li>
                </ul>
            </div>
            <div>
                <select name="status" class="status" 
                @change="updateSand($event, sandwiche.id)">
                    <option value="">Selecione</option>
                    <option v-for="s in status" :key="s.id" :value="s.tipo" 
                    :selected="(sandwiche.status = s)">
                        {{ s.tipo }}
                    </option>
                </select>
                <button class="delete-btn" @click="deletarSand(sandwiche.id)">Cancelar</button>
            </div>
        </div>
    </div>
</template>

<script>
    import Mensagem from './Mensagem.vue'
    export default {
        name: "Dash",
        data(){
            return{
                sandwiches: null,
                sandwiche_id: null,
                status: [],
                msg: null
            }
        },
        components: {
            Mensagem
        },
        methods: {
            async getPedidos() {
                const req = await fetch("http://localhost:3000/sandwiches");
                const data = await req.json();
                this.sandwiches = data;

                //status resgatando
                this.getStatus();
            },
            async getStatus() {
                const req = await fetch("http://localhost:3000/status");
                const data = await req.json();
                this.status = data;  
                          
            },
            async deletarSand(id){
                const req = await fetch(`http://localhost:3000/sandwiches/${id}`,{
                    method: "DELETE"
                });
                const res = await req.json();
                //mensagem no sistema
                this.msg = 'Sanduba excluído com sucesso!'

                //limpar msg
                setTimeout(()=> this.msg = "", 2000)
                this.getPedidos();
            },
            async updateSand(event, id){
                const option = event.target.value;
                const dataJson = JSON.stringify({status: option});
                const req = await fetch(`http://localhost:3000/sandwiches/${id}`,{
                    method: "PATCH",
                    headers: { "Content-Type": "application/json"},
                    body: dataJson
                });

                const resq = await req.json();
                //mensagem no sistema
                this.msg = `O pedido N° ${resq.id} foi atulizada para ${resq.status}`;

                //limpar msg
                setTimeout(()=> this.msg = "", 2000)

                const res = await req.json();
                console.log(res);
            }
        },
        mounted() {
            this.getPedidos();
        }
    }
</script>

<style scoped>
    #sandw-table{
        max-width: 1200px;
    }

    #sandw-table-head,
    #sandw-table-linhas,
    .sandw-table-linha{
        display: flex;
        flex-wrap: wrap;
        max-width: 1200px;
        
    }

    #sandw-table-head {
        font-weight: bold;
        padding: 12px;
        border-bottom: 3px solid #333;
    }

    #sandw-table-head div,
    .sandw-table-linha div {
        width: 19%;
    }

    .sandw-table-linha {
        width: 100%;
        padding: 12px;
        border-bottom: 1px solid #CCC;
    }

    #sandw-table-head .order-id,
    .sandw-table-linha .order-number{
        width: 5%;
    }

    select {
        padding: 12px 6px;
        margin-right: 12px;
    }

    .delete-btn {
        background-color: #222;
        color: orange;
        font-weight: bold;
        border: 2px solid #222;
        padding: 10px;
        font-size: 10px;
        margin: 0 auto;
        cursor: pointer;
        transition: .5s;        
    }

    .delete-btn:hover {
        background-color: transparent;
        color: #222;

    }
</style>