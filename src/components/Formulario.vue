<template>
    <div>
        <Message :msg="msg" v-show="msg" />
        <div>
            <form id="task-form" @submit="createTask">
                <div class="input-container">
                    <label for="titulo">Título da Tarefa</label>
                    <input type="text" id="titulo" name="title" v-model="titulo" placeholder="Digite o título da tarefa">
                </div>

                <div class="input-container">
                    <label for="descricao">Descrição da Tarefa</label>
                    <textarea id="descricao" name="description" v-model="descricao" placeholder="Digite a descrição"></textarea>
                </div>

                <!-- Seleção de prazo -->
                <div class="input-container">
                    <label for="duracao">Prazo</label>
                    <select id="duracao" v-model="duracao">
                        <option value="Curto Prazo">Curto Prazo</option>
                        <option value="Médio Prazo">Médio Prazo</option>
                        <option value="Longo Prazo">Longo Prazo</option>
                    </select>
                </div>

                <div class="input-container">
                    <input type="submit" class="submit-btn" value="Criar Tarefa">
                </div>
            </form>
        </div>
    </div>
</template>

<script>
import Message from './Message.vue';

export default {
    name: "Formulario",

    data() {
        return {
            titulo: null,
            descricao: null,
            duracao: null, // Agora como Curto/Médio/Longo Prazo
            opcaoTeste: [], // Array para armazenar as opções de teste selecionadas
            msg: null
        }
    },

    methods: {
        async createTask(e) {
            e.preventDefault();

            const data = {
                titulo: this.titulo,
                descricao: this.descricao,
                duracao: this.duracao,
                opcaoTeste: this.opcaoTeste, // As opções selecionadas
                status: "Pendente"
            }

            const dataJson = JSON.stringify(data);

            try {
                const req = await fetch("http://localhost:3000/tasks", {
                    method: "POST",
                    headers: { "Content-Type": "application/json" },
                    body: dataJson
                });

                const res = await req.json();

                this.msg = `Tarefa Nº ${res.id} criada com sucesso!`;

                setTimeout(() => this.msg = "", 3000);

                // Limpa os campos após a criação da tarefa
                this.titulo = "";
                this.descricao = "";
                this.duracao = "";
                this.opcaoTeste = []; // Limpa as opções de teste selecionadas
            } catch (error) {
                console.error('Erro ao criar a tarefa:', error);
            }
        }
    },

    components: {
        Message
    }
}
</script>

<style scoped>

    #task-form {
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
        border-left: 4px solid #C7D5E0;
    }

    input, textarea, select {
        padding: 5px 10px;
        width: 300px;
    }

    textarea {
        height: 100px;
    }

    .submit-btn {
        background-color: #222;
        color: #C7D5E0;
        font-weight: bold;
        border: 2px solid #222;
        padding: 10px;
        font-size: 16px;
        margin: 0 auto;
        cursor: pointer;
        transition: .5s;
    }

    .submit-btn:hover {
        background-color: transparent;
        color: #222;
    }

</style>
