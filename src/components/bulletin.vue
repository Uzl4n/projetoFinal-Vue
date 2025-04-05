<script setup>
//Requisições GET

import 'bootstrap/dist/css/bootstrap.min.css';
import 'bootstrap/dist/js/bootstrap.min.js';
import {  onMounted, ref } from 'vue';

    let objeto = ref({'id':0, 'aluno':'', 'nota1':0, 'nota2':0, 'media':0, 'situacao':''});     

    let notas = ref([]);

    //Visibilidade Button

    let btnCadastrar = ref(true);

    let termoFiltragem = ref('');

    let vetor = ref ([]);

    function filtrar(){
        return notas.value.filter(obj => obj.aluno.toLowerCase().includes(termoFiltragem.value.toLowerCase()));
    }

    onMounted(() => {
         fetch('http://localhost:3000/notas')
            .then(requisicao => requisicao.json())
                .then(retorno => notas.value = retorno);
    }); 
    

    function cadastrar(event){
        
        objeto.value.media    = (objeto.value.nota1 + objeto.value.nota2)/2;

        objeto.value.situacao = objeto.value.media >= 7 ? 'Aprovado' : objeto.value.media >= 5 ? 'Em exame': 'Reprovado';

        if(objeto.value.aluno == null){
            
            alert('Formulário invalidado');
        }

        fetch('http://localhost:3000/notas', {
            method:'POST',
            body:JSON.stringify(objeto.value),
            headers:{'Content-Type':'application/json'}
        })
            .then(requisicao => requisicao.json())
            .then(retorno => {

           

                

                notas.value.push(retorno)

                objeto.value.aluno  = '';
                objeto.value.nota1  = 0;
                objeto.value.nota2  = 0;
                objeto.value.media  = 0;  
                 
            });

                 event.preventDefault();
    }

    function editar(){

        objeto.value.media = (objeto.value.nota1 + objeto.value.nota2)/2;

        objeto.value.situacao = objeto.value.media >= 7 ? 'Aprovado' : objeto.value.media >= 5 ? 'Em exame': 'Reprovado';
        
            fetch(`http://localhost:3000/notas/${objeto.value.id}`, {
                method:'PUT',
                body:JSON.stringify(objeto.value),
                headers:{'Content-Type':'application/json'}
            })
                .then(requisicao => requisicao.json())
                .then(retorno => {

                    let indiceAluno = notas.value.findIndex(ObjetoP => {
                        return ObjetoP.id === retorno.id;
                    });
                
                        notas.value[indiceAluno] = retorno;

                        btnCadastrar.value = true;

                        objeto.value.id    = 0;
                        objeto.value.aluno = '';
                        objeto.value.nota1 = 0;
                        objeto.value.nota2 = 0;
                
            });      

    }

    function remover(){
        
            fetch(`http://localhost:3000/notas/${objeto.value.id}`, {
                method:'DELETE',
                
                headers:{'Content-Type':'application/json'}
            })
                .then(requisicao => requisicao.json())
                .then(() => {

                    let indiceAluno = notas.value.findIndex(ObjetoP => {
                        return ObjetoP.id === objeto.value.id;
                    });
                
                        notas.value.splice(indiceAluno, 1);

                        btnCadastrar.value = true;

                        objeto.value.id = 0;
                        objeto.value.produto= '';
                        objeto.value.valor = 0;

            });
    }

    function selecionar(indice){

        objeto.value = {

            id:     notas.value[indice].id,
            aluno:  notas.value[indice].aluno,
            nota1:  notas.value[indice].nota1,
            nota2:  notas.value[indice].nota2
        }

    
                btnCadastrar.value = false;
            
             
    }
    
    const alunoValidation = [
        value => {
            if(value) return true;

            return 'O email é obrigatório';
        }
    ];

</script>


<template>

    <h1 class="nt">Notas Escolares</h1>
    
        <form @submit="cadastrar">

           


            <!-- Filtragem -->
        <div class="row">
                <div class="col-12">
                    <input type="text" v-model="termoFiltragem" placeholder="Qual aluno você está procurando?" class="form-control pesquisa">

                    <div class="alunos">
                    <p v-if="filtrar().length == 0">Não foi encontrado nenhum Aluno.</p>
                    <p v-else-if="filtrar().length == 1">Foi encontrado apenas um Aluno.</p>
                    <p v-else>Foram encontrados {{ filtrar().length }} alunos.</p>
                    </div>

                </div>
        </div>


        <input type="hidden" v-model="objeto.id">
            <input type="text" placeholder="Nome" class="form-control" v-model="objeto.aluno" :rules="alunoValidation">
            <input type="number" placeholder="Primeira Nota" class="form-control" v-model="objeto.nota1">
            <input type="number" placeholder="Segunda Nota" class="form-control" v-model="objeto.nota2">
            <input type="submit" v-if="btnCadastrar" placeholder="Cadastrar" class="btn btn-primary">
            <input type="button" @click="editar" v-if="!btnCadastrar" value="Editar" class="btn btn-primary espacamentoBtn">
            <input type="button" @click="remover" v-if="!btnCadastrar" value="Remover" class="btn btn-primary">



        </form>
    
        <table class="table table-striped">
            <thead>
                <tr>
                    <th>Nome</th>
                    <th>Primeira Nota</th>
                    <th>Segunda  Nota</th>
                    <th>Média</th>
                    <th>Situação</th>
                </tr>
            </thead>
            
            <tbody>
                <tr vclass="col-12 col-sm-6 col-md-4 col-lg-3" v-for="(p, indice) in filtrar()">

                        <td>{{ p.aluno }}</td>
                        <td>{{ p.nota1 }}</td>
                        <td>{{ p.nota2 }}</td>
                        <td>{{ p.media }}</td>
                        <td>{{ p.situacao }}</td>

                    <td><button @click="selecionar(indice)" class="btn btn-primary">Selecionar</button></td>
                </tr>
            </tbody>
        </table>
</template>