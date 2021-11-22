<template>
   <div class="form-todo form-group">
          <p>
            <input
              placeholder="Name"
              type="text"
              name="author"
              class="form-control"
              v-model="name"
            />
          </p>

          <p>
            <textarea
              placeholder="Comment"
              type="text"
              name="message"
              id=""
              class="form-control"
              v-model="message"
            ></textarea>
          </p>
          <button v-on:click="addComment" type="submit" class="btn btn-primary" >Comment</button>
          <hr />
        </div>
</template>

<script>
export default {
  data(){
    return {
      name: "",
      message: "",
    }
  },
  methods: {
    addComment() {
            //Validação para que não possa ser inserido textos vazios.
            //Trim() serve para apagar o texto em branco antes e depois do que ele digitou.
            if (this.message.trim() === '') {
              return
            }
            //Ele dispara um evento para todo mundo que está escutando vai capturar o evento e o argumento.
            this.$emit('add-todo',{
              name: this.name,
              message: this.message
            })

            //Logo após adicionar os comentários no array, resetamos o que foi inserido antes pelo usuário.
            this.name = '';
            this.message = '';
          }
  }
}
</script>