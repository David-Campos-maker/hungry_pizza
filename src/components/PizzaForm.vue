<template>
    <div>
        <Message :msg="msg" v-show="msg" />
        <div>
            <form id="pizza-form" @submit="createPedido">
                <div class="input-container">
                    <label for="nome">Nome do cliente:</label>
                    <input type="text" id="nome" name="nome" v-model="nome" placeholder="Digite o seu nome">
                </div>
                <div class="input-container">
                    <label for="endereco">Endereço de entrega:</label>
                    <input type="text" id="endereco" name="endereco" v-model="endereco" placeholder="Digite o seu endereço">
                </div>
                <div class="input-container">
                    <label for="telefone">Telefone para contato:</label>
                    <input type="text" id="telefone" name="telefone" v-model="telefone" placeholder="Digite o seu número de telefone com DDD">
                </div>
                <div class="input-container">
                    <label for="sabor">Escolha o sabor:</label>
                    <select name="sabor" id="sabor" v-model="sabor">
                        <option value="">Selecione o sabor</option>
                        <option v-for="sabor in sabores" :key="sabor.id" :value="sabor.tipo">
                            {{ sabor.tipo }}
                        </option>
                    </select>
                </div>
                <div id="opcionais-container" class="input-container">
                    <label id="opcionais-title" for="opcionais">Selecione os opcionais:</label>
                    <div class="checkbox-container" v-for="opcional in opcionais_data" :key="opcional.id">
                        <input type="checkbox" name="opcionais" v-model="opcionais" :value="opcional.tipo">
                        <span>{{ opcional.tipo }}</span>
                    </div>
                </div>
                <div class="input-container">
                    <input type="submit" class="submit-btn" value="Finalizar Pedido">
                </div>
                
            </form>
        </div>
    </div>
</template>

<script>
import Message from './Message.vue';

    export default {
    name: "PizzaForm",
    components: {
        Message
    },
    data() {
        return {
            sabores: null,
            opcionais_data: null,
            nome: null,
            endereco: null,
            telefone: null,
            sabor: null,
            opcionais: [],
            msg: null
        };
    },
    methods: {
        async getPizzass() {
            const req = await fetch("http://localhost:3000/pizzas");
            const data = await req.json();
            this.sabores = data.sabores;
            this.opcionais_data = data.opcionais;
        },
        async createPedido(event) {
            event.preventDefault();
            const data = {
                nome: this.nome,
                endereco: this.endereco,
                telefone: this.telefone,
                sabor: this.sabor,
                opcionais: Array.from(this.opcionais),
                status: "Solicitado"
            };
            const dataJson = JSON.stringify(data);
            const req = await fetch("http://localhost:3000/pedidos", {
                method: "POST",
                headers: { "Content-Type": "application/json" },
                body: dataJson
            });
            const res = await req.json();

            this.msg = `Pedido N°${res.id} realizado com sucesso!`;

            setTimeout(() => this.msg = "" , 3000);

            this.nome = "";
            this.endereco = "";
            this.telefone = "";
            this.sabor = "";
            this.opcionais = "";
        }
    },
    mounted() {
        this.getPizzass();
    }
}
</script>

<style scoped>
    #pizza-form {
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
        color: #222;
        padding: 5px 10px;
        border-left: 4px solid #FCBA03;
    }

    input , select {
        padding: 5px 10px;
        width: 300px
    }

    #opcionais-container {
        flex-direction: row;
        flex-wrap: wrap;
    }

    #opcionais-title {
        widows: 100%;
    }

    .checkbox-container {
        display: flex;
        align-items: flex-start;
        width: 50%;
        margin-bottom: 20px;
    }

    .checkbox-container span ,
    .checkbox-container input {
        width: auto;
    }

    .checkbox-container span {
        margin-left: 6px;
        font-weight: bold;
    }

    .submit-btn {
        background-color: #222;
        color: #FCBA03;
        font-weight: bold;
        border: 2px solid #222;
        border-radius: 2px;
        padding: 10px;
        font-size: 16px;
        margin: 0 auto;
        cursor: pointer;
        transition: .5s; 
    }

    .submit-btn:hover {
        background-color: transparent;
        color: #222
    }
</style>