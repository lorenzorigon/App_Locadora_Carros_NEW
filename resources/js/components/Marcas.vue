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
                         <button type="button" class="btn btn-primary btn-sm float-right">Adicionar</button>
                    </template>
                </card-component>
            <!--Fim do card de Registros-->
        </div>
    </div>
</div>
</template>

<script>
   
</script>
