<template>
    <div>
      <h1>Lista de Atividades</h1>
      <input v-model="newTask" @keyup.enter="addTask" placeholder="Adicionar nova atividade" />
      <button @click="addTask">Adicionar</button>
      <ul>
        <li v-for="(task, index) in tasks" :key="index">
          <div v-if="editIndex === index">
            <input v-model="editTask" @keyup.enter="saveTask(index)" />
            <button @click="saveTask(index)">Salvar</button>
            <button @click="cancelEdit">Cancelar</button>
          </div>
          <div v-else>
            <div>
              {{ task.text }}
              <span class="timestamp">Criada em: {{ task.createdAt }}</span>
              <span class="timestamp" v-if="task.editedAt">Editada em: {{ task.editedAt }}</span>
            </div>
            <button @click="editTaskAt(index)">Editar</button>
            <button @click="removeTask(index)">Remover</button>
          </div>
        </li>
      </ul>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        newTask: '',
        tasks: [],
        editIndex: null,
        editTask: ''
      };
    },
    methods: {
      addTask() {
        if (this.newTask.trim() !== '') {
          const currentDate = new Date().toLocaleString();
          this.tasks.push({
            text: this.newTask.trim(),
            createdAt: currentDate,
            editedAt: null
          });
          this.newTask = '';
        }
      },
      removeTask(index) {
        this.tasks.splice(index, 1);
      },
      editTaskAt(index) {
        this.editIndex = index;
        this.editTask = this.tasks[index].text;
      },
      saveTask(index) {
        if (this.editTask.trim() !== '') {
          const currentDate = new Date().toLocaleString();
          this.tasks[index].text = this.editTask.trim();
          this.tasks[index].editedAt = currentDate;
          this.editIndex = null;
          this.editTask = '';
        }
      },
      cancelEdit() {
        this.editIndex = null;
        this.editTask = '';
      }
    }
  };
  </script>
  
  <style scoped>
  div {
    max-width: 400px;
    margin: 0 auto;
    padding: 20px;
    text-align: center;
  }
  
  input {
    width: 100%;
    padding: 10px;
    margin-bottom: 10px;
  }
  
  button {
    padding: 10px 20px;
    margin-top: 10px;
    margin-left: 5px;
  }
  
  ul {
    list-style-type: none;
    padding: 0;
    width: 550px;
    /* display: flex;
    flex-direction: column; */
  }
  
  li {
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 10px;
    background: #f4f4f4;
    margin-bottom: 10px;
  }
  
  li div {
    display: flex;
    justify-content: space-between;
    align-items: center;
    width: 100%;
  }
  
  li div input {
    margin-right: 10px;
  }
  
  .timestamp {
    font-size: 0.8em;
    color: #888;
    margin-left: 10px;
  }
  </style>
  