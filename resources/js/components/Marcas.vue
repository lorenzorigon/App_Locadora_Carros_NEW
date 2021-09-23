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
                                    <input type="number" class="form-control" id="inputNome" aria-describedby="nomelHelp" placeholder="Nome da Marca">
                                </input-container-component>
                            </div>
                            <div class="col mb-3">
                                <input-container-component titulo='ID' id='inputId' idHelp='idHelp' textoAjuda='Opcional. Informe o ID da marca'>
                                    <input type="text" class="form-control" id="inputId" aria-describedby="idHelp" placeholder="ID da Marca">
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
                            <table-component></table-component>
                        </template>
                        <template v-slot:rodape>
                            <button type="button" class="btn btn-primary btn-sm float-right" data-toggle="modal" data-target="#modalMarca">Adicionar</button>
                        </template>
                    </card-component>
                <!--Fim do card de Registros-->
            </div>
        </div>
        <modal-component id="modalMarca" titulo="Adicionar Marca">
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
       data(){
           return{
               urlBase: 'http://localhost:8000/api/v1/marca',
               nomeMarca: '',
               arquivoImagem: []
           }
       },
       methods:{
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
                       'Accept' : 'application/json'
                   }
               }

               axios.post(this.urlBase, formData, config)
                .then(response => {
                    console.log(response)
                })
                .catch(errors => {
                    console.log(errors)
                })
           }
       }
   }
</script>
