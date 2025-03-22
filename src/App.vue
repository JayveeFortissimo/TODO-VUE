<template>
  <div class="todo-container">
    <div class="todo-card">
      <h1 class="todo-title">TODO LIST</h1>
      <form @submit.prevent="handleSubmit" class="todo-form">
        <div class="input-group">
          <input
            type="text"
            placeholder="Add a new task..."
            :value="inputs.input"
            @input="handleChange('input', $event.target.value)"
            class="todo-input"
          />
          <button type="submit" class="add-btn">
            <span>ADD</span>
          </button>
        </div>
      </form>

      <div class="todo-list" v-if="AllData && AllData.length">
        <div class="todo-item" v-for="x in AllData" :key="x.id">
          <div class="todo-content">
            <p v-if="is_Edit !== x.id" class="todo-text">{{ x.todo }}</p>
            <input 
              v-else 
              type="text" 
              v-model="newEdit"
              class="edit-input"
              autofocus
            />
            {{ console.log(newEdit) }}
            {{ console.log(AllData) }}
          </div>
          <div class="todo-actions">
            <button v-if="is_Edit !== x.id" @click="editButton(x.id)" class="edit-btn">
              Edit
            </button>
            <button v-else class="editing-btn" @click="finalEdit(x.id)">
              Editing...
            </button>
            <button @click="deleteButton(x.id)" class="delete-btn">
              Delete
            </button>
          </div>
        </div>
      </div>
      <p v-else class="empty-message">No tasks yet. Add one above!</p>
    </div>
  </div>
</template>


<script>
export default {
  data() {
    return {
      AllData: [],
      inputs: {
        id: 1,
        input: '',
      },
      is_Edit: 0,
      newEdit: ''
    }
  },
  methods: {
    handleChange(type, value) {
      this.inputs[type] = value
    },
    handleSubmit() {
      this.AllData.push({ id: this.inputs.id++, todo: this.inputs.input })
      this.inputs.input = '';
    },
    editButton(id) {
      this.is_Edit = id;
    },
    deleteButton(id) {
      const result = this.AllData.filter((pro) => pro.id !== id)
      this.AllData = result
    },
    finalEdit(id){
        this.is_Edit = 0;

       this.AllData = this.AllData.map(pro =>{
          if(pro.id === id){
              return {...pro, todo:this.newEdit}
          }
          return pro
       })
    }
  },
}
</script>




<style>
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
  font-family: 'Poppins', sans-serif;
}

body {
  background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
  min-height: 100vh;
}

.todo-container {
  min-height: 100vh;
  padding: 2rem 1rem;
  display: flex;
  align-items: center;
  justify-content: center;
}

.todo-card {
  min-height: 30rem;
  width: 100%;
  max-width: 550px;
  background-color: white;
  border-radius: 16px;
  padding: 2.5rem 2rem;
  display: flex;
  flex-direction: column;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
  transition: all 0.3s ease;
}

.todo-title {
  font-size: 2rem;
  font-weight: 700;
  color: #333;
  text-align: center;
  margin-bottom: 2rem;
  letter-spacing: 1px;
}

.todo-form {
  width: 100%;
  margin-bottom: 1rem;
}

.input-group {
  display: flex;
  width: 100%;
  gap: 0.5rem;
}

.todo-input {
  flex: 1;
  height: 3rem;
  padding: 0 1rem;
  border: 1px solid #e0e0e0;
  border-radius: 8px;
  font-size: 1rem;
  outline: none;
  transition: border 0.3s ease;
}

.todo-input:focus {
  border-color: #3f51b5;
  box-shadow: 0 0 0 2px rgba(63, 81, 181, 0.2);
}

.add-btn {
  background-color: #3f51b5;
  color: white;
  height: 3rem;
  padding: 0 1.5rem;
  border: none;
  border-radius: 8px;
  font-weight: 600;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.add-btn:hover {
  background-color: #303f9f;
}

.todo-list {
  margin-top: 2rem;
  width: 100%;
  overflow-y: auto;
  max-height: 400px;
  padding-right: 5px;
}

.todo-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 1rem;
  margin-bottom: 0.75rem;
  border-radius: 8px;
  background-color: #f7f9fc;
  transition: all 0.3s ease;
  border-left: 4px solid #3f51b5;
}

.todo-item:hover {
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.05);
  transform: translateY(-2px);
}

.todo-content {
  flex: 1;
  margin-right: 1rem;
  overflow: hidden;
}

.todo-text {
  color: #333;
  font-size: 1rem;
  word-break: break-word;
}

.edit-input {
  width: 100%;
  padding: 0.5rem;
  border: 1px solid #3f51b5;
  border-radius: 4px;
  font-size: 1rem;
}

.todo-actions {
  display: flex;
  gap: 0.5rem;
}

.edit-btn, .delete-btn, .editing-btn {
  padding: 0.5rem 1rem;
  border: none;
  border-radius: 6px;
  font-weight: 500;
  cursor: pointer;
  transition: all 0.3s ease;
}

.edit-btn {
  background-color: #f0f4ff;
  color: #3f51b5;
}

.edit-btn:hover {
  background-color: #d6e0ff;
}

.editing-btn {
  background-color: #e8f5e9;
  color: #4caf50;
}

.delete-btn {
  background-color: #fff2f2;
  color: #f44336;
}

.delete-btn:hover {
  background-color: #ffdfdf;
}

.empty-message {
  text-align: center;
  color: #9e9e9e;
  margin-top: 3rem;
  font-style: italic;
}

/* Responsive adjustments */
@media (max-width: 600px) {
  .todo-card {
    padding: 1.5rem 1rem;
  }
  
  .todo-title {
    font-size: 1.5rem;
    margin-bottom: 1.5rem;
  }
  
  .todo-item {
    flex-direction: column;
    align-items: flex-start;
  }
  
  .todo-content {
    width: 100%;
    margin-right: 0;
    margin-bottom: 0.75rem;
  }
  
  .todo-actions {
    width: 100%;
    justify-content: space-between;
  }
  
  .edit-btn, .delete-btn, .editing-btn {
    padding: 0.4rem 0.8rem;
    font-size: 0.9rem;
  }
}
</style>