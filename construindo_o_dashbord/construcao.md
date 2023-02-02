
## Processo realizado para construir o dashboard:

### 1º Passo:

Inicialmente ***exportamos os dados*** para o Power BI. 


### 2º Passo:

Em seguida foi necessario realizar alguns ***tratamentos nos dados no Power Query***, renomeamos a coluna Salario para SalarioAno já que se refere ao salario anual dos funcionarios, criamos uma coluna SalarioMes a partir da coluna SalarioAno e nas colunas de datas mudamos a localidade para inglês EUA.


### 3º Passo:

Após tratar os dados, importamos a base de dados para dentro do Power BI Desktop e ***criamos algumas medidas*** para desenvolver as visualizações.

Medidas criadas:

* Admissões: Representa o número de admissões da empresa, para fazer esse cálculo basta contarmos o número de linhas da tabela contratações, para isso usamos o comando: COUNTROWS(Tb_contratacoes);

* Desligamentos: Representa o número de desligamentos  da empresa, para fazer esse cálculo contamos o número de linhas da tabela contratações que é diferente de ativo, para isso usamos o comando: CALCULATE(COUNTROWS(Tb_contratacoes),Tb_contratacoes[Status]<>"Ativo")

* FuncionariosAtivos: Representa o número de funcionarios ativos da empresa desde o inicio, para fazer esse cálculo fazemos uma diferença do total de admissões e dos desligamentos, para isso usamos o comando:

<dl>
  <dd> VAR AdmissoesAcumulado = CALCULATE([Admissoes],FILTER(ALL(Tb_contratacoes),[DataContratacao]<=Max(Tb_contratacoes[DataContratacao])))  </dd>
  <dd> VAR DesligamentosAcumulado = CALCULATE([Desligamentos],FILTER(ALL(Tb_contratacoes),[DataContratacao]<=Max(Tb_contratacoes[DataContratacao])))  </dd>
  <dd> RETURN AdmissoesAcumulado-DesligamentosAcumulado </dd>
</dl>

* Saldo: Representa o número de funcionarios ativos atual da empresa, para fazer esse cálculo fazemos uma diferença entre as admissões e os desligamentos, para isso usamos o comando: [Admissoes]-[Desligamentos]

* TurnoverGeral: O turnover é um indicador capaz de analisar a quantidade de colaboradores que permanecem na empresa em determinado período. Quanto menor for a taxa de turnover, maior é a retenção de colaboradores e a produtividade dos mesmos, evitando também gastos por alta rotatividade. Para fazer o cálculo, é preciso primeiro saber a média entre a quantidade de admissões e desligamentos no período e depois dividir pelo número de funcionarios do inicio, para cálcula-lo usamos o comando: DIVIDE(([Admissoes]+[Desligamentos])/2, [FuncionariosAtivos] )

* MetaTurnoverGeral: Igual á 0.1.

* TurnoverDesligamentos: Esse turnover especifica a taxa de desligamentos impostos pela empresa, para cálcula-lo usamos o comando:  DIVIDE([Desligamentos], [FuncionariosAtivos])

* Absenteismo: Está relacionado a faltas ou atrasos do colaborador. Em nosso caso, apenas consideramos as faltas por não ter o tempo de atraso de cada colaborador dentro do dataset. Para cálcula-lo usamos o comando: 
   
<dl>
  <dd> CALCULATE(  </dd>
  <dd> COUNT(Tb_contratacoes[NomeEmpregado])*SUM(Tb_contratacoes[Faltas])  </dd>
  <dd> /  </dd>
  <dd> (COUNT(Tb_contratacoes[NomeEmpregado])*20),  </dd>
  <dd> Tb_contratacoes[MotivoSaida] = "Continua Empregado" )  </dd>
</dl>


* MetaAbsenteismo: Igual á 0.05. De acordo com algumas fontes, é considerado aceitável uma taxa média de Absenteísmo de 5%, porém fatores influenciam diretamente nesse percentual, como tamanho da empresa e segmento. Assim, o percentual aceitável pode variar.



### 4º Passo:

***Criamos as visualizações***.


### 5º Passo:

Nesse passo ***personalizamos o dashboard***. Utilizamos algumas imagens fornecidas pela Alura, criamos um menu com alguns filtros utilizando indicadores, deixamos o dashboard acessivel usando tabulação.

### 5º Passo:

Por fim criamos ***uma apresentação com slides*** contando uma historia sobre os resultados encontrados.









