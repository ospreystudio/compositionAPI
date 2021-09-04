<template>
  <form @submit.prevent="submit" class="form">
    <h1>Добавить рецепт</h1>
    <div v-if="visible">
      <div class="input">
        <input type="text" placeholder="Название рецепта" v-model="form.title">
      </div>
      <div class="input">
        <input type="text" placeholder="Описание рецепта" v-model="form.description">
      </div>
    </div>

    <div class="buttons">
      <button class="btn" type="submit" :disabled="!valid">Создать</button>
      <button class="btn secondary" type="button" @click="toggle">
        {{ visible ? 'Убрать' : 'Показать' }}

      </button>
    </div>
  </form>
</template>

<script>

import {ref, reactive, computed} from 'vue'
export default {
  props: {
    onAdd: {
      type: Function,
      required: true
    }
  },
  setup(props) {
    const visible = ref(true);
    const form = reactive({
      title: '',
      description: ''
    })
     const submit =() => {
      const recipe = {
        title: form.title.trim(),
        description: form.description.trim(),
        id: Date.now().toString()
      }
       form.title = form.description =''
       props.onAdd(recipe)
      }

    const toggle = () => {
      visible.value = !visible.value
    }

    const valid = computed(() => {
      return form.title.trim() && form.description.trim()
    })

    return {
      visible, toggle, form, submit, valid
    }
  },
  // data() {
  //   return {
  //     title: "",
  //     description: "",
  //     // visible: true
  //   }
  // },
  methods: {
    // toggle() {
    //   this.visible = !this.visible
    // },


  },
  // computed: {
  //   valid() {
  //     return this.title.trim() && this.description.trim()
  //   }
  // }

}
</script>

<style>
  .form {
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 1rem;
    border: 1px solid #eee;
    margin-bottom: 1rem;
  }

  .form h1 {
    margin: 0;
    margin-bottom: 1rem;
  }

  .input {
    margin-bottom: 1rem;
  }

  .input input {
    border: 1px solid #ccc;
    border-radius: 5px;
    padding: 5px 8px;
    width: 400px;
  }

  .buttons {
    width: 400px;
    display: flex;
    justify-content: space-around;
  }
</style>
