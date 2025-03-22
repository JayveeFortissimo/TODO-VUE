<template>
  <div class="Main">
    <div class="child">
      <h1>TODO LIST</h1>
      <form @submit.prevent="handleSubmit">
        <div class="input">
          <input
            type="text"
            placeholder="Input Here"
            :value="inputs.input"
            @input="handleChange('input', $event.target.value)"
          />

          <button type="submit">ADD</button>
        </div>
      </form>

      <div class="DATAS">
        <ul v-for="x in AllData" :key="x.id">
          <div>
            <li v-if="is_Edit !== x.id">{{ x.todo }}</li>
            <li v-else>
              <input type="text" v-model="x.todo" @blur="saveEdit(x.id)" />
            </li>
          </div>

          <span>
            <button @click="editButton(x.id)">Edit</button>
            <button>Now EDIT</button>
          </span>
          <span><button @click="deleteButton(x.id)">Delete</button></span>
        </ul>
      </div>
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
      now_Edit: 0,
    }
  },
  methods: {
    handleChange(type, value) {
      this.inputs[type] = value
    },
    handleSubmit() {
      this.AllData.push({ id: this.inputs.id++, todo: this.inputs.input })
      this.inputs.input = ''
    },
    editButton(id) {
      this.is_Edit = id
    },
    deleteButton(id) {
      const result = this.AllData.filter((pro) => pro.id !== id)
      this.AllData = result
    },
  },
}
</script>
