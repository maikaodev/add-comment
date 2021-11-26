<template>
  <div class="container">
    <div class="d-flex justify-content-between align-items-center">
      <h1>Comments</h1>
      <button v-on:click="changeTheme" class="button">
        {{ theme }}
      </button>
    </div>
    <hr />

    <FormToDo v-on:add-todo="addComment"></FormToDo>

    <div class="list-group">
      <p v-if="comments.length <= 0">No comment...</p>
      <div
        class="list-group-item"
        v-for="(comment, index) in allComments"
        v-bind:key="index"
      >
        <span class="comment_author"
          >Author: <strong>{{ comment.name }}</strong></span
        >
        <p>{{ comment.message }}</p>
        <div>
          <a href="#" title="Exclude" v-on:click.prevent="removeComment(index)"
            >Delete</a
          >
        </div>
      </div>
    </div>
    <hr />
  </div>
</template>

<script>
import FormToDo from './FormToDo'
//Por baixo dos panos ele pega o objeto e instância um new Vue, e passa esse objeto de configuração.
export default {
  components: {
    FormToDo
  },
  /* Quando tratamos de template o nosso date deve ser chamado como função e sempre retornando um objeto.
   */
  data() {
    const savedComments = localStorage.getItem('comments')
    const savedCurrentTheme = localStorage.getItem('currentTheme')
    const savedTheme = localStorage.getItem('theme')

    //Convertendo string em objeto para poder inserir no array.
    //Atribuir algum valor se tiver algo no localStorage, caso contrário será array vazia.
    return {
      comments: savedComments ? JSON.parse(savedComments) : [],
      currentTheme: savedCurrentTheme ? savedCurrentTheme : 'default-theme',
      theme: savedTheme ? savedTheme : 'Dark'
    }
  },
  methods: {
    addComment(comment) {
      this.comments.push(comment)
    },
    //No v-for adicionamos a variável index para pegar as posições de cada objeto/Comentário. Então passamos ele como paramentos no exclude para poder pegar o valor do index e conseguir excluir o objeto correto.
    removeComment(index) {
      //Splice serve para apagar os itens do array, passando como parâmentro o index colhido pelo v-for e o total de itens queremos excluir.
      this.comments.splice(index, 1)
    },
    changeTheme() {
      if (this.currentTheme == 'default-theme') {
        this.currentTheme = 'dark'
        this.theme = 'Light'
      } else {
        this.currentTheme = 'default-theme'
        this.theme = 'Dark'
      }
    },
    validatingClass() {
      const getTheme = document.getElementById('default')

      if (this.currentTheme === 'dark') {
        getTheme.classList.add('dark')
        getTheme.classList.remove('default-theme')
      } else {
        getTheme.classList.remove('dark')
        getTheme.classList.add('default-theme')
      }
    }
  },
  computed: {
    allComments() {
      return this.comments.map(comment => ({
        //Nesse array vou retornar as próprias propriedades do objeto, porém quero alterar apenas o name.
        ...comment,
        //Na propriedade name mesmo tirando os espaço continua igual a uma string vazia, eu vou printar 'Anonymous', senão for string vazia print o nome inserido pelo usuário.
        name: comment.name.trim() === '' ? 'Anonymous' : comment.name
      }))
    }
  },
  watch: {
    //Ele observa o array e qualquer mudança ele vai imprimir(quando tem mudança). Podemos usar isso quando interagimos com Banco de Dados, localStorage etc.
    comments(val) {
      //convertendo objeto para string para poder salvar no localStorage
      localStorage.setItem('comments', JSON.stringify(val))
    },
    currentTheme(val) {
      localStorage.setItem('currentTheme', val)
      this.validatingClass()
    },
    theme(val) {
      localStorage.setItem('theme', val)
    }
  },
  created() {
    this.validatingClass()
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  border: none;
}
body .button {
  background-color: #333;
  color: #fff;

  padding: 6px 16px;
  border-radius: 4px;

  font-weight: bold;
}
body.dark {
  background-color: #333;
  color: #fff;
}
body.dark .button {
  background-color: #fff;
  color: #333;
}
</style>
