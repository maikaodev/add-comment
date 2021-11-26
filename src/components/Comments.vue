<template>
  <div class="container">
          <div class="d-flex justify-content-between align-items-center">
            <h1>Comments</h1>
            <button v-on:click="switchTheme" class="btn btn-dark" >Switch Theme</button>
          </div>
        <hr />

        <FormToDo v-on:add-todo="addComment"></FormToDo>
       
        <div class="list-group">
          <p v-if="comments.length <= 0">No comment...</p>
          <div class="list-group-item" v-for="(comment, index) in allComments" v-bind:key="index">
            <span class="comment_author">Author: <strong>{{comment.name}}</strong></span>
            <p>{{comment.message}}</p>
            <div>
              <a href="#" title="Exclude" v-on:click.prevent="removeComment(index)">Delete</a>
            </div>
          </div>
        </div>
        <hr />
      </div>
</template>

<script>

import FormToDo from './FormToDo';
//Por baixo dos panos ele pega o objeto e instância um new Vue, e passa esse objeto de configuração.
export default {

  components: {
    FormToDo
  },
   /* Quando tratamos de template o nosso date deve ser chamado como função e sempre retornando um objeto.
    */
        data() {
          const savedComments = localStorage.getItem("comments");
          const savedTheme = localStorage.getItem("theme");

          //Convertendo string em objeto para poder inserir no array.
          //Atribuir algum valor se tiver algo no localStorage, caso contrário será array vazia.
          return {
            comments: savedComments ? JSON.parse(savedComments) : [],
            theme: savedTheme ? savedTheme : "default-theme"
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
            switchTheme(){
                
                if(this.theme == "default-theme"){
                    this.theme = "dark"
                  }else{
                    this.theme =  "default-theme"
                  }

            },
            validatingClass(){           
              
              const getTheme = document.getElementById("default")


              if(this.theme === "dark"){
                  getTheme.classList.add("dark")
                  getTheme.classList.remove("default-theme")
              }else{                        
                  getTheme.classList.remove("dark")
                  getTheme.classList.add("default-theme")
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
            localStorage.setItem("comments", JSON.stringify(val));
          },
          theme(val){

              localStorage.setItem("theme", val)
              this.validatingClass()
          }
        },
          created(){
            this.validatingClass()
          }
        
}
</script>

<style>
  body.dark {
    background-color:#333;
    color:#fff;
  }
</style>