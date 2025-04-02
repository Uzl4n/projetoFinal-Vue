<script setup>
//Requisições GET

import 'bootstrap/dist/css/bootstrap.min.css';
import 'bootstrap/dist/js/bootstrap.min.js';


import {  onMounted, ref } from 'vue';

let notas = ref([]);

onMounted(() => {
         fetch('http://localhost:3000/notas')
            .then(requisicao => requisicao.json())
                .then(retorno => notas.value = retorno);
    }); 


    //Requisições POST

    let objeto = ref({'id':0, 'aluno':'', 'nota1':0, 'nota2':0, 'media':0, 'situacao':''});

    let valor2 = 10;
    let valor1 = 2;

    function media(){
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

                objeto.value.nota1 = value.nota1 + value.nota2;
                

            });
    }


    function cadastrar(event){
        
        fetch('http://localhost:3000/notas', {
            method:'POST',
            body:JSON.stringify(objeto.value),
            headers:{'Content-Type':'application/json'}
        })
            .then(requisicao => requisicao.json())
            .then(retorno => {

                notas.value.push(retorno)

                objeto.value.aluno  = '';
                objeto.value.nota1 = 0;
                objeto.value.nota2 = 0;

                objeto.value.media = 3+3;
                 
            });

        event.preventDefault();
    }

    function editar(){
        
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

                objeto.value.id = 0;
                objeto.value.aluno= '';
                objeto.value.nota1 = 5;
                objeto.value.nota2 = 5;
                

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

    //Visibilidade Button

    let btnCadastrar = ref(true);


</script>

<style>

        form{
            width: 50%;
            margin: 25px auto;
            text-align: center;
        }

        input{
            margin-bottom: 10px;
        }

        .espacamentoBtn{

            margin-left: 5px;
            margin-right: 5px;

        }
</style>

<template>

    <h1>Notas Escolares</h1>
        {{ console.log(valor2 + valor1) }}
        {{ console.log(nota1) }}
        <form @submit="cadastrar">
            <input type="hidden" v-model="objeto.id">
            <input type="text" placeholder="Nome" class="form-control" v-model="objeto.aluno">
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
                <tr v-for="(p, indice) in notas">
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