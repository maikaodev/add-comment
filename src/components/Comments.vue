<template>
  <div class="container">
        <h1>Comments</h1>
        <hr />

        <FormToDo v-on:add-todo="addComment"></FormToDo>
       
        <div class="list-group">
          <p v-if="comments.length <= 0">No comment...</p>
          <div class="list-group-item" v-for="(comment, index) in allComments" v-bind:key="index">
            <span class="comment_author">Author: <strong>{{comment.name}}</strong></span>
            <p>{{comment.message}}</p>
            <div>
              <a href="#" title="Exclude" v-on:click.prevent="removeComment(index)">Exclude</a>
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
          return {
            comments: []
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
          //Ele observa o array e qualquer mudança ele vai imprimir. Podemos usar isso quando interagimos com Banco de Dados, localStorage etc.
          comments(val) {
            console.log('val', val)
          }
        }
}
</script>

