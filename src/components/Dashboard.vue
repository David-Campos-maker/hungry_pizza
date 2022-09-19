<template>
    <div id="pizza-table">
        <div>
            <div id="pizza-table-heading">
                <div class="order-id">#:</div>
                <div>Cliente:</div>
                <div>Endereço:</div>
                <div>Sabor:</div>
                <div>Opcionais:</div>
                <div>Ações:</div>
            </div>
        </div>
        <div id="pizza-table-rows">
            <div class="pizza-table-row" v-for="pedido in pedidos" :key="pedido.id">
                <div class="order-number">{{ pedido.id }}</div>
                <div>{{ pedido.nome }}</div>
                <div>{{ pedido.endereco }}</div>
                <div>{{ pedido.sabor }}</div>
                <div>
                    <ul>
                        <li v-for="(opcional , index) in pedido.opcionais" :key="index">
                            {{ opcional }}
                        </li>
                    </ul>
                </div>
                <div>
                    <select name="status" class="status">
                        <option value="">Selecione</option>
                    </select>
                    <button class="delete-btn">Cancelar</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: "Dashboard" ,

        data() {
            return  {
                pedidos: null ,
                pedidos_id: null ,
                status: []
            }
        },

        methods: {
            async getPedidos() {
                const req = await fetch("http://localhost:3000/pedidos");
                const data = await req.json();

                this.pedidos = data;
            }
        },

        mounted() {
            this.getPedidos();
        }
    }
</script>

<style scoped>
    #pizza-table {
        max-width: 1200px;
        margin: 0 auto;
    }

    #pizza-table-heading ,
    #pizza-table-rows ,
    .pizza-table-row {
        display: flex;
        flex-wrap: wrap;
    }

    #pizza-table-heading {
        font-weight: bold;
        padding: 12px;
        border-bottom: 3px solid #333;
    }

    #pizza-table-heading div ,
    .pizza-table-row div {
        width: 19%;
    }

    .pizza-table-row {
        width: 100%;
        padding: 12px;
        border-bottom: 1px solid #CCC;
    }

    #pizza-table-heading .order-id ,
    .pizza-table-row .order-number {
        width: 5%;
    }

    select {
        padding: 12px 6px;
        margin-right: 12px;
    }

    .delete-btn {
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

    .delete-btn:hover {
        background-color: transparent;
        color: #222;
    }
</style>
