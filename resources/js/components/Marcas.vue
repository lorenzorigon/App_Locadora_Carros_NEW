<template>
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-md-8">
                <!--Inicio do card de Busca-->
                <card-component titulo="Busca de Marcas">
                    <template v-slot:conteudo>
                        <div class="form-row">
                            <div class="col mb-3">
                                <input-container-component titulo='Nome da Marca' id='inputNome' idHelp='nomeHelp' textoAjuda='Opcional. Informe o nome da marca'>
                                    <input type="text" class="form-control" id="inputNome" aria-describedby="nomelHelp" placeholder="Nome da Marca">
                                </input-container-component>
                            </div>
                            <div class="col mb-3">
                                <input-container-component titulo='ID' id='inputId' idHelp='idHelp' textoAjuda='Opcional. Informe o ID da marca'>
                                    <input type="number" class="form-control" id="inputId" aria-describedby="idHelp" placeholder="ID da Marca">
                                </input-container-component>
                            </div>
                        </div>
                    </template>
                    <template v-slot:rodape>
                        <button type="submit" class="btn btn-primary btn-sm float-right">Pesquisar</button>
                    </template>    
                </card-component>
                
                <!--Fim do card de Busca-->

                <!--Inicio do card de Registros-->
                    <card-component titulo="Relação de Marcas">
                        <template v-slot:conteudo>
                            <table-component :dados="marcas" :titulos="['id', 'nome', 'imagem']"></table-component>
                        </template>
                        <template v-slot:rodape>
                            <button type="button" class="btn btn-primary btn-sm float-right" data-toggle="modal" data-target="#modalMarca">Adicionar</button>
                        </template>
                    </card-component>
                <!--Fim do card de Registros-->
            </div>
        </div>
        <modal-component id="modalMarca" titulo="Adicionar Marca">
            <template v-slot:alertas>
                <alert-component tipo="success" :detalhes="transacaoDetalhes" titulo="Marca cadastrada com sucesso!" v-if="transacaoStatus == 'adicionado'"></alert-component>
                <alert-component tipo="danger" :detalhes="transacaoDetalhes" titulo="Erro ao tentar cadastrar a marca" v-if="transacaoStatus == 'erro'"></alert-component>
            </template>
            <template v-slot:conteudo>
                <div class="form-group">
                    <input-container-component titulo='Nome da marca' id='novoNome' idHelp='novoNomeHelp' textoAjuda='Informe o nome da marca'>
                        <input type="text" class="form-control" id="novoNome" aria-describedby="novoNomeHelp" placeholder="Nome da marca" v-model="nomeMarca">
                    </input-container-component>
                    {{nomeMarca}}
                </div>
                <div class="form-group">
                    <input-container-component titulo='Imagem' id='novoImagem' idHelp='novoImagemHelp' textoAjuda='Selecione uma imagem no formato PNG'>
                        <input type="file" class="form-control-file" id="novoImagem" aria-describedby="novoImagemHelp" placeholder="Selecione uma imagem" @change="carregarImagem($event)">
                    </input-container-component>
                    {{arquivoImagem}}
                </div>
            </template>

            <template v-slot:rodape>
                <button type="button" class="btn btn-secondary" data-dismiss="modal">Fechar</button>
                <button type="button" class="btn btn-primary" @click="salvar()">Salvar</button>
            </template>
        </modal-component>
    </div>
</template>

<script>
   export default{
       computed: {
               token(){
                   let token = document.cookie.split(';').find(indice => {
                       return indice.includes('token=')
                   })
                   token = token.split('=')[1]
                   token = 'Bearer '+ token
                   return token;
               }
           },
       mounted(){
           this.carregarLista()
       },    
       data(){
           return{
               urlBase: 'http://localhost:8000/api/v1/marca',
               nomeMarca: '',
               arquivoImagem: [],
               transacaoStatus: '',
               transacaoDetalhes: {},
               marcas: []
           }
       },
       methods:{
           carregarLista(){

               let config = {
                   headers: {
                       'Accept' : 'application/json',
                       'Authorization' : this.token
                   }
                }

               axios.get(this.urlBase, config)
                .then(response => {
                    this.marcas = response.data
                    console.log(this.marcas);
                })
                .catch(errors => {
                    console.log(errors)
                })
           },
           carregarImagem(e){
               this.arquivoImagem = e.target.files
           },
           salvar(){
               let formData = new FormData();
               formData.append('nome', this.nomeMarca)
               formData.append('imagem', this.arquivoImagem[0])

               let config = {
                   headers: {
                       'Content-Type' : 'multpart/form-data',
                       'Accept' : 'application/json',
                       'Authorization' : this.token
                   }
               }

               axios.post(this.urlBase, formData, config)
                .then(response => {
                    this.transacaoStatus = 'adicionado'
                    this.transacaoDetalhes = {
                    mensagem: 'ID do registro: '+response.data.id
                    }
                    console.log(response)
                })
                .catch(errors => {
                    this.transacaoStatus = 'erro'
                    this.transacaoDetalhes = {
                    mensagem: errors.response.data.message,
                    dados: errors.response.data.errors
                    }
                    //errors.response.data.message
                })
           }
       }
   }
</script>
