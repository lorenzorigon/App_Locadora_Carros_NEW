<template>
    <div>
        <table class="table table-hover">
            <thead>
                <tr>
                    <th scope="col" v-for="t, key in titulos" :key="key">{{t}}</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="obj in dados" :key="obj.id">
                    <td v-if="titulos.includes(chave)" v-for="valor, chave in obj" :key="chave">
                        <span v-if="chave == 'imagem'">
                            <img :src="'/storage/'+valor" width="50px" height="50px" >
                        </span>
                        <span v-else>
                            {{valor}}
                        </span>
                    </td>
                

                <!--
                <tr v-for="m in dados" :key="m.id">
                    <th scope="row">{{m.id}}</th>
                    <td>{{m.nome}}</td>
                    <td><img :src="'/storage/'+m.imagem" width="50px" height="50px" ></td>
                </tr>
                -->
                </tr>
            </tbody>
        </table>
    </div>
</template>

<script>
    export default {
        props:['dados', 'titulos']
    }
</script>
