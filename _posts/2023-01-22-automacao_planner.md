---
title: Automação da criação de tarefas no Ms Planner
date: 2023-01-22 12:00 -03:00
categories: [project]
tags: [learning]
---

# Introdução

O [Ms Planner](https://www.microsoft.com/pt-br/microsoft-365/business/task-management-software) é uma das ferramentas disponíveis para gestão de projetos, que se destaca pela gestão visual das atividades, possibilidade de colaboração e pela facilidade de uso.

Uma das principais dificuldades encontradas (por mim) é adicionar tarefas de um projeto, que por motivo de facilidade de acesso, estão em um arquivo `.xlsx` do [Ms Excel](https://www.microsoft.com/pt-br/microsoft-365/excel). Muitas vezes há dezenas de linhas com atividades de diferentes datas de início e de conclusão, fato que dificulta a opção pela ferramenta **Ms Planner**, já que seria necessário adicionar as tarefas manualmente e uma a uma.

Recentemente descobri uma solução integrada, da própria [Microsoft](https://www.microsoft.com/pt-br), que facilita a criação de um quadro de tarefas no [Ms Planner](https://www.microsoft.com/pt-br/microsoft-365/business/task-management-software). Este *post* visa apresentar uma maneira que descobri (depois de assistir a alguns tutoriais e ler a documentação da [Microsoft](https://www.microsoft.com/pt-br)) de automatizar o preenchimento de um quadro de tarefas no [Ms Planner](https://www.microsoft.com/pt-br/microsoft-365/business/task-management-software), utilizando a ferramenta [Power Automate](https://powerautomate.microsoft.com/pt-br/).

---

## 1. Preparação

Antes de começar a automação, é necessário ter em "mãos" a planilha com as tabelas necessárias (aqui referenciadas como `tabela 1` e `tabela 2`) e ter criado um quadro de tarefas no **Ms Planner** (aqui referenciado como `Teste`). 

Para que o fluxo do **Power Automate** funcione, é necessário que as tabelas estejam criadas com a propriedade `Tabela` no **Ms Excel**, segue um exemplo de como criar a tabela:

- **(1)** Selecionar a tabela:
<div style="text-align: center"><img src="/assets/images/automocao1.png"/></div>

- **(2)** Criar a tabela na aba `Inserir` e selecionar `Tabela`:
<div style="text-align: center"><img src="/assets/images/automocao2.png"/></div>

- **(3)** Repetir as etapas (1) e (2), para a `Tabela 2`:
<div style="text-align: center"><img src="/assets/images/automocao1.1.png"/></div>

Com o arquivo criado (referenciado como `Planilha_Teste.xlsx`) teremos 2 (duas) tabelas:

- `Tabela 1` - com os nomes dos *Buckets* do **Ms Planner**;
- `Tabela 2` - com as atividades de cada *Bucket* (com data de início e de conclusão).

> OBS: Ao utilizarmos o fluxo do **Power Automate**, as datas colocadas no **Ms Excel** perdem 1 (um) dia quando são adicionadas no **Ms Planner** desta forma. Para prevenir isto, adicione 1 (um) dia nas datas adicionadas (data de início e de conclusão).

---

## 2. Upload do arquivo do Ms Excel

Para possibilitar a integração entre **Ms Planner** e **Ms Excel**, precisamos realizar o upload do arquivo que geramos anteriormente `Planilha_Teste.xlsx`.

É possível fazer o upload de diversas formas como por exemplo:

- (i) utilizando as equipes do **Ms Teams**;
- (ii) o **Ms OneDrive** e
- (iii) o **Ms SharePoint**. 

Neste tutorial utilizou-se o **Ms OneDrive** por comodidade. Adicionou-se o arquivo `.xlsx` na raiz do **Ms OneDrive**, sem pasta específica.

---

## 3. Criação dos Buckets do Ms Planner

Conforme tratado na etapa de preparação, temos na `Tabela 1` os nomes dos *Buckets* que serão criados no **Ms Planner**. 

É possível realizar esta etapa de maneira manual no próprio **Ms Planner**, mas para simular um projeto com diversas macro atividades, optou-se por realizar esta etapa de forma automatizada pelo **Power Automate**.

- **(1)** Abra o aplicativo **Power Automate**.

- **(2)** Clique em `Criar` e depois em `Fluxo da nuvem instantâneo`:
<div style="text-align: center"><img src="/assets/images/automocao3.png"/></div>

- **(3)** Irá abrir uma janela. Selecione a opção `Disparar um fluxo manualmente` e em seguida clique em `Criar`:
<div style="text-align: center"><img src="/assets/images/automocao4.png"/></div>

- **(4)** Irá abrir uma página de criação de fluxo. Selecione `+ Nova etapa` e selecione `Excel Online (Business)`:
<div style="text-align: center"><img src="/assets/images/automocao5.png"/></div>

- **(5)** Selecione `Listar linhas presentes em uma tabela`:
<div style="text-align: center"><img src="/assets/images/automocao6.png"/></div>

- **(6)** Neste exemplo as informações inseridas ficaram desta forma, selecionando a `Tabela 1`:
<div style="text-align: center"><img src="/assets/images/automocao7.png"/></div>

- **(7)** Adicione mais uma etapa e escolha a opção `Control`, selecione `Aplicar a cada` e selecione `value` (valor de cada linha). Em seguida clique em `Adicionar uma ação`, digite `Planner` e selecione a opção `Criar um bucket`. Deverá ficar desta forma (lembrando que o quadro criado neste exemplo foi referenciado como `Teste`):
<div style="text-align: center"><img src="/assets/images/automocao8.png"/></div>

- **(8)** Clique em `Testar` no canto superior direito e observe se o fluxo funcionou:
<div style="text-align: center"><img src="/assets/images/automocao9.png"/></div>

Se todas as etapas foram bem inseridas, você deverá obter um fluxo bem executado (O *Bucket* `Tarefas Pendentes` é criado automaticamente pelo **Ms Planner** e será removido manualmente).
<div style="text-align: center"><img src="/assets/images/automocao10.png"/></div>

---

## 4. Listando o BucketID

Ainda não obtive sucesso na utilização do **Power Automate** sem esta etapa. É necessário utilizar um fluxo específico para listar os **IDs** dos Buckets criados anteriormente.

- **(1)** Crie um fluxo seguindo os mesmos passos da etapa anterior, e crie uma etapa do `Planner` chamada `Listar buckets`:
<div style="text-align: center"><img src="/assets/images/automacao11.png"/></div>

- **(2)** Após a execução bem sucedida do fluxo, clique na etapa concluída `Listar buckets` e observer o campo `SAÍDAS`:
```json
{
    "@odata.etag": "W/\"JzEtQnVja2V0QEBAQEBAQEBAQEBAQEBARCc=\"",
    "name": "Macro 10",
    "planId": "_2F00ziRMUigDyhjUpYmQWQADPad",
    "orderHint": "8585270253125222844",
    "id": "Q0uubqlCdEaOxcYyE9bvS2QAIISi"
},
```
No exemplo acima, temos a saída bruta informando o **BucketID** ("id": ) do *Bucket* `Macro 10`. Este fluxo nos dará o **BucketID** de todos os *Buckets* inseridos no **Ms Planner**.

Com essas informações, podemos criar uma nova coluna na `Tabela 2` e utilizarmos a fórmula `PROCV` do **Ms Excel** para referenciar os **BucketIDs** encontrados com os respectivos nomes dos *buckets*.

---

## 5. Preparação planilha de atividades

Para possibilitar a inclusão das atividades nos buckets criados, precisamos referenciar os **BucketIds** encontrados, anteriormente, com os respectivos nomes dos *buckets*.

É necessário que as datas estejam da forma `ano-mês-dia`, para que o fluxo funcione. Pegando as Colunas `Start date` e `Due date`, podemos utilizar a fórmula `TEXTO` do Ms Excel para formatar da maneira desejada.
```
=TEXTO([@[Start date]];"aaaa-mm-dd") # Coluna "Começo"

=TEXTO([@[Due date]];"aaaa-mm-dd") # Coluna "Final"
```
Após as modificações realizadas teremos o seguinte arquivo:
<div style="text-align: center"><img src="/assets/images/automacao12.png"/></div>

Com o novo arquivo preparado, refez-se o upload da `Planilha_Teste.xlsx` no **Ms OneDrive**.

---

## 6. Criação das tarefas no Ms Planner

Com a `Tabela 2` preparada, podemos executar o último fluxo do **Power Automate**.

- **(1)** Criar novo fluxo manual;

- **(2)** Selecionar `Excel Online (Business)` e escolher a opção `Listar linhas presentes em uma tabela`:
<div style="text-align: center"><img src="/assets/images/automacao13.png"/></div>

- **(3)** Adicione uma etapa `Control` e selecione a opção `Aplicar a cada`;

- **(4)** Adicione uma nova ação do `Planner` e selecione a opção `Criar uma tarefa`;

- **(5)** Preencha a ação da seguinte forma:
<div style="text-align: center"><img src="/assets/images/automacao14.png"/></div>

- **(6)** Salve e teste o fluxo. Caso tenha seguido todos os passos, o fluxo terá concluído com sucesso:
<div style="text-align: center"><img src="/assets/images/automacao15.png"/></div>

**Parabéns!** Você acaba de adicionar 30 tarefas no **Ms Planner** (com datas de início e conclusão) com diferentes *buckets*, em aproximadamente 30 segundos.
<div style="text-align: center"><img src="/assets/images/automacao16.png"/></div>