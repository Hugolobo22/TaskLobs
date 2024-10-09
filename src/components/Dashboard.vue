<template>
    <div id="task-table" v-if="tasks">
        <Message :msg="msg" v-show="msg" />
        <div>
            <div id="task-table-heading">
                <div class="task-id">#:</div>
                <div>Título:</div>
                <div>Descrição:</div>
                <div>Prazo:</div>
                <div>Ações:</div> <!-- Coluna Teste foi removida -->
            </div>
        </div>
        <div id="task-table-rows">
            <div class="task-table-row" v-for="task in tasks" :key="task.id">
                <div class="task-number">{{ task.id }}</div>
                <div>{{ task.titulo }}</div>
                <div>{{ task.descricao }}</div>
                <div>{{ task.duracao }}</div>
                <div class="task-acoes">
                    <select name="status" class="status" @change="updateTask($event, task.id)">
                        <option :value="s.tipo" v-for="s in status" :key="s.id" :selected="task.status == s.tipo">
                            {{ s.tipo }}
                        </option>
                    </select>
                    <button class="delete-btn" @click="deleteTask(task.id)">Remover</button>
                </div>
            </div>
        </div>
    </div>
    <div v-else>
        <h2>Não há tarefas cadastradas!</h2>
    </div>
</template>

<script>
import Message from './Message.vue';

export default {
    name: "Dashboard",
    data() {
        return {
            tasks: null,
            status: [],
            msg: null
        }
    },

    components: {
        Message
    },
    methods: {
        async getTasks() {
            const req = await fetch('http://localhost:3000/tasks');

            const data = await req.json();

            // Remover "opcaoTeste" e manter apenas os dados necessários
            this.tasks = data.map(task => ({
                id: task.id,
                titulo: task.titulo,
                descricao: task.descricao,
                duracao: task.duracao,
                status: task.status
            }));

            // Resgata os status de tarefas
            this.getStatus();
        },
        async getStatus() {
            const req = await fetch('http://localhost:3000/status');

            const data = await req.json();

            this.status = data;
        },
        async deleteTask(id) {
            const req = await fetch(`http://localhost:3000/tasks/${id}`, {
                method: "DELETE"
            });

            const res = await req.json();

            this.msg = `Tarefa removida com sucesso!`;

            this.getTasks();
        },
        async updateTask(event, id) {
            const option = event.target.value;

            const dataJson = JSON.stringify({ status: option });

            const req = await fetch(`http://localhost:3000/tasks/${id}`, {
                method: "PATCH",
                headers: { "Content-Type": "application/json" },
                body: dataJson
            });

            const res = await req.json();

            console.log(res);
        }
    },
    mounted() {
        this.getTasks();
    }
}
</script>

<style scoped>
#task-table {
    max-width: 1200px;
    margin: 20px auto;
    border-collapse: collapse;
    font-family: Arial, sans-serif;
}

#task-table-heading,
#task-table-rows,
.task-table-row {
    display: flex;
    flex-direction: column;
    align-items: stretch;
}

#task-table-heading {
    font-weight: bold;
    padding: 12px;
    border-bottom: 3px solid #333;
    background-color: #f2f2f2;
    display: flex;
    flex-direction: row;
}

.task-table-row {
    width: 100%;
    padding: 12px;
    border-bottom: 1px solid #CCC;
    background-color: #fafafa;
    display: flex;
    flex-direction: row;
    justify-content: space-between;
}

#task-table-heading div,
.task-table-row div {
    flex: 1;
    padding: 10px 5px;
    text-align: left;
}

#task-table-heading .task-id,
.task-table-row .task-number {
    width: 5%;
    text-align: center;
}

#task-table-heading .task-acoes,
.task-table-row .task-acoes {
    width: 15%;
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
    align-items: center;
}

select {
    padding: 6px;
    font-size: 14px;
    border: 1px solid #CCC;
    border-radius: 4px;
    width: 100%;
    margin-bottom: 10px;
}

.task-table-row .task-acoes .delete-btn {
    padding: 6px 8px;
    font-size: 12px;
    width: 100%;
    height: 30px;
}

.delete-btn {
    background-color: #222;
    color: #fcba03;
    font-weight: bold;
    border: 2px solid #222;
    cursor: pointer;
    transition: 0.3s ease;
    border-radius: 3px;
}

.delete-btn:hover {
    background-color: transparent;
    color: #222;
}

/* Responsividade */
@media (max-width: 768px) {
    #task-table-heading,
    .task-table-row {
        flex-direction: column;
        align-items: flex-start;
    }

    .task-table-row .task-acoes {
        flex-direction: column;
        justify-content: flex-start;
    }

    select, .delete-btn {
        margin-top: 10px;
        width: 100%;
    }
}
</style>
