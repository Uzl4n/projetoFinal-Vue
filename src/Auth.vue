
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

<!-- Listagem -->

<div class="row">
   <div class="col-12 col-sm-6 col-md-4 col-lg-3" v-for="(p, indice) in filtrar()">
           <p>{{ p.aluno }}</p>
       </div>
</div>





listagem

<div class="row">
   <tr class="col-12 col-sm-6 col-md-4 col-lg-3" v-for="(p, indice) in filtrar()">
           <td>{{ p.aluno }}</td>
           <td>{{ p.nota1 }}</td>
           <td>{{ p.nota2 }}</td>
           <td>{{ p.media }}</td>
           <td>{{ p.situacao }}</td>
   </tr>
</div>



----------------


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

<!-- Listagem -->

<div class="row">
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
       <tr class="col-12 col-sm-6 col-md-4 col-lg-3" v-for="(p, indice) in filtrar()">
           <td>{{ p.aluno }}</td>
           <td>{{ p.nota1 }}</td>
           <td>{{ p.nota2 }}</td>
           <td>{{ p.media }}</td>
           <td>{{ p.situacao }}</td>
   </tr>
   </tbody>
</table>

</div>