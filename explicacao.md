```javascript 
const form = document.getElementById("novoItem");
```
acessamos o formulário pelo id e salvamos na const form.  


```javascript 
form.addEventListener("submit", (evento) => {
    evento.preventDefault()
})  
``` 

adicionamos um `eventListener` a `const form` - o submit - que chamamos  
de evento. Cancelamos o comportamento padrão do botão com o 
`preventDefault()`.  

```javascript
form.addEventListener("submit", (evento) => {
    evento.preventDefault()

    console.log(evento);
})
/*
com o console log em evento, conseguimos acessar várias propriedades do form 
ao clicar em submit.

Uma dessas propriedades é a propriedade (objeto) "elements" encontrada dentro da propriedade 
"srcElements".
Em, "elements", podemos acessar as inputs pelo id/nome

sabendo disso, temos:
*/ 

form.addEventListener("submit", (evento) => {
    evento.preventDefault()

    evento.target.elements["nome"].value;
    evento.target.elements["quantidade"].value;
    
})

/* 
Assim, podemos acessar o valor de nome e a quantidade ao escutar o evento.


Até aqui, apenas, trilhamos o caminho para acessarmos o input.
Agora, vamos preparar o código para criarmos um novo elemento.
Começando pela função criaElemento, que deve passar como parâmetro
o nome e a quantidade.
*/

function criaElemento(nome, quantidade) {

}

/* 
Então, chamamos a função, aonde acontece o evento, e passamos então,
o mesmo caminho que encontramos do input, como parâmetro da função.
Pois esses caminhos que acessamos, são do nome e da quantidade.
*/

form.addEventListener("submit", (evento) => {
    evento.preventDefault()   
    
    criaElemento(evento.target.elements["nome"].value, 
    evento.target.elements["quantidade"].value);
})

/*
E dentro da criaElemento, vamos começar a passar o passo a passo, do
que a função deve fazer para criar o novo elemento.
*/

function criaElemento(nome, quantidade) {

    const novoItem = document.createElement("li");
    novoItem.classList.add("item");

    const numeroItem = document.createElement("strong");
    numeroItem.innerHTML = quantidade;

    novoItem.innerHTML = numeroItem + nome;

    console.log(novoItem);
}

/*
Porém, se criarmos todos os elementos, com o createElement, criaremos um objeto
contendo todos esses valores. E NÃO conseguimos criar um novo elemento na lista:

        <ul class="lista">
            <li class="item"><strong>7</strong>Novo Elemento</li>
        </ul>

Então, para conseguirmos colocar, a quantidade e o nome, dentro da "li"
usamos a função appendChild().
Que adiciona um nó ao final da lista de filhos de um nó pai especificado. 
Se o nó já existir no documento, ele é removido de seu nó pai atual antes de ser 
adicionado ao novo pai.

    Sintaxe:

        var filho = elementoPai.appendChild(filho);

*/

function criaElemento(nome, quantidade) {

    const novoItem = document.createElement("li");
    novoItem.classList.add("item");

    const numeroItem = document.createElement("strong");
    numeroItem.innerHTML = quantidade;

    novoItem.appendChild(numeroItem);
    novoItem.innerHTML += nome;

    console.log(novoItem);
}

/*

Depois de passarmos

    elementoPai.appendChild(filho);

Apenas, terminamos a lista adicionando o nome

        novoItem.appendChild(numeroItem);
        novoItem.innerHTML += nome;

Agora, sim. O resultado é

    innerHTML: "<strong>3</strong>camisa"

Agora, precisamos acessar a "ul" pai da "li", para adicionarmos o conteudo
da lista.
*/

function criaElemento(nome, quantidade) {

    const novoItem = document.createElement("li");
    novoItem.classList.add("item");

    const numeroItem = document.createElement("strong");
    numeroItem.innerHTML = quantidade;

    novoItem.appendChild(numeroItem);
    novoItem.innerHTML += nome;

    const lista = document.getElementById("lista");

    lista.appendChild(novoItem);

    console.log(novoItem);
}

/*
 
E para adicionar "li"  dentro de "ul", usamos a mesma função appendChild()
Passando a lista = "ul" .appendChild(novoItem = "li")

Nesse caso, a const lista pode ficar fora da função, delcarada como
uma variável global:
 */

const form = document.getElementById("novoItem");
const lista = document.getElementById("lista");

form.addEventListener("submit", (evento) => {
    evento.preventDefault()   
    
    criaElemento(evento.target.elements["nome"].value, 
    evento.target.elements["quantidade"].value);
})

function criaElemento(nome, quantidade) {

    const novoItem = document.createElement("li");
    novoItem.classList.add("item");

    const numeroItem = document.createElement("strong");
    numeroItem.innerHTML = quantidade;

    novoItem.appendChild(numeroItem);
    novoItem.innerHTML += nome;

    lista.appendChild(novoItem);

}

/**
 * Até aqui, só adicionamos o item na lista
 * mas, não conseguimos salva-lo no local storage.
 * 
 * local storage:
 * 
 *  é um pequeno espaço dentro do navegador, aonde sites, podem salvar
 *  dados do tipo string.
 * 
 * É bastante usada, para geração de conteudo dinamico, dentro do html, gerada
 * via js. Como uma lista, ou o resultado de um formulário. O que seria a mesma
 * coisa, já que nesse caso, a lista é criada via tag <form> também.
 * 
 */

/*
 * Antes, vamos refatorar o formulário para que ele apague o valor do input
 * depois de registrado o item na lista
 */

form.addEventListener("submit", (evento) => {
    evento.preventDefault() 
    
    const nome = evento.target.elements["nome"];
    const quantidade = evento.target.elements["quantidade"];

    criaElemento(nome.value, quantidade.value);

    nome.value = "";
    quantidade.value = "";
})

/** 
 * Dessa maneira, armazenamos os elementos em variáveis constantes 
 * e depois que criaElemento, adicionar o item na lista
 * nome e quantidade recebem uma string vazia, para limpar o input
*/

/**
 * O código abaixo funcionaria...
 * mas, ele sobreescreve os itens, adicionando apenas um item no local storage.
 */

function criaElemento(nome, quantidade) {

    const novoItem = document.createElement("li");
    novoItem.classList.add("item");

    const numeroItem = document.createElement("strong");
    numeroItem.innerHTML = quantidade;

    novoItem.appendChild(numeroItem);
    novoItem.innerHTML += nome;

    lista.appendChild(novoItem);

    localStorage.setItem("nome", nome);
    localStorage.setItem("quantidade", quantidade);

}

/**
 *  Para resolver isso, criamos uma const contendo uma array, vazia, no escopo global do codigo
 */

const itens = [];

/**
 * E então, criamos um objeto, para guardar o nome e a quantidade
 * que tinhamos passado para o localStorage.setItem()
 */

const itemAtual = {
    "nome": nome,
    "quantidade": quantidade
}

/**
 * usamos o push() para empurrar esses valores para a nossa array criada
 * 
 * Essa função, pega o itemAtual e empurra para dentro de const itens = []
 */


itens.push(itemAtual);

/**
 * Por fim, dizemos ao setItem() que agora ele deve adicionar ao "item" a string da array itens.
 * 
 * JSON.stringfy() transforma o conteudo do objeto, em uma string. Se não for feito isso, o retorno é
 * um objeto, e o codigo vai ser subescrito novamente.
 */

localStorage.setItem("item", JSON.stringify(itens));


/**
 * ao final temos algo parecido com o codigo abaixo
 */


const form = document.getElementById("novoItem");
const lista = document.getElementById("lista");
const itens = [];

form.addEventListener("submit", (evento) => {
    evento.preventDefault() 
    
    const nome = evento.target.elements["nome"];
    const quantidade = evento.target.elements["quantidade"];

    criaElemento(nome.value, quantidade.value);

    nome.value = "";
    quantidade.value = "";
})

function criaElemento(nome, quantidade) {

    const novoItem = document.createElement("li");
    novoItem.classList.add("item");

    const numeroItem = document.createElement("strong");
    numeroItem.innerHTML = quantidade;

    novoItem.appendChild(numeroItem);
    novoItem.innerHTML += nome;

    lista.appendChild(novoItem);

    const itemAtual = {
        "nome": nome,
        "quantidade": quantidade
    }

    itens.push(itemAtual);

    localStorage.setItem(novoItem, JSON.stringify(itens));

}

/**
 * vamos transformar a const itens para ela conseguir armazenar objetos, usando JSON.parse(). 
 * E se caso o local storage estiver vazio, ela armazena uma array vazia.
 * Se não fizermos o JSON.parse() ela retornara uma string, pois fizemos.   
 *      localStorage.setItem(novoItem, JSON.stringify(itens));
 * transformando o arquivo JSON em string. 
 */

const itens = JSON.parse(localStorage.getItem("itens")) || [];

/**
 * refatorando o codigo, da maneira a baixo
 * podemos começar com a lista no html vazia
 * e os itens serao adicionados dinamicamente pelo js
 */

const form = document.getElementById("novoItem");
const lista = document.getElementById("lista");
const itens = JSON.parse(localStorage.getItem("itens")) || [];

itens.forEach((elemento) => {
    criaElemento(elemento);
})

form.addEventListener("submit", (evento) => {
    evento.preventDefault() 
    
    const nome = evento.target.elements["nome"];
    const quantidade = evento.target.elements["quantidade"];

    const itemAtual = {
        "nome": nome.value,
        "quantidade": quantidade.value
    }

    criaElemento(itemAtual);

    itens.push(itemAtual);

    localStorage.setItem("itens", JSON.stringify(itens));

    nome.value = "";
    quantidade.value = "";
})

function criaElemento(item) {

    const novoItem = document.createElement("li");
    novoItem.classList.add("item");

    const numeroItem = document.createElement("strong");
    numeroItem.innerHTML = item.quantidade;

    novoItem.appendChild(numeroItem);
    novoItem.innerHTML += item.nome;

    lista.appendChild(novoItem);

}

/**
 * fazendo uma condicional, para atualizarmos um item já existente, ao inves de adicionarmos uma nova linha
 */

const existe = itens.find(elemento => elemento.nome === nome.value);
//existe é = procura no itens *array* (no elemento => o nome do elemento === o valor do nome digitado *input*)
/**
 * ou seja, a const existe, será o resultado da busca, dentro do array pelo elemento na posição nome, exatamente
 * igual ao valor nome, digitado no input
 */

/**
 * Com o método find(), ele procura um elemento e, com o operador de comparação ===, 
 * ele compara se o valor e tipo de dois elementos são idênticos.
 */

/**
 * E então, precisamos adicionar um identificador no elemento criado, ou seja, na função criaElemento.
 * Para isso, o objeto deve estar declarado antes, da condição.
 */

const itemAtual = {
    "nome": nome.value,
    "quantidade": quantidade.value
}

if()...

/**
 * Agora, precisamos definir um data-set (data attributes) via js, no nosso strong. Ou seja, no numero
 * do item, pois é ele que queremos atualizar. 
 */

const numeroItem = document.createElement("strong");
numeroItem.innerHTML = item.quantidade;
numeroItem.dataset.id = item.id 
//em numeroItem, adicione o dataset, id, que se chamara item.id

/**
 * agora, criamos uma função para atualizar o item
 */

function atualizaElemento(item) {
    document.querySelector("[data-id='"+item.id+"']").innerHTML = item.quantidade;
}
//seleciona o data-id (não entendi essa estrutura).coloca no html, como quantidade do item.

/**
 * e a nossa condição, fica assim
 */

if(existe) {                    //se existir
    itemAtual.id = existe.id;   //o id do item atual é igual ao do item que existe
    
    atualizaElemento(itemAtual); //atualiza o elemento do item atual
}else{                           //senão
    itemAtual.id = itens.length; //o id do item atual recebe um novo valor do tamanho da lista de id (length)

    criaElemento(itemAtual);     //cria o elemento no item atual

    itens.push(itemAtual);       // empura o item atual para a lista
}

/**
 * feito isso, a interface sera atualizada. porém o local storage, não.
 * vamos corrigir isso...
 * 
 * Para isso, é preciso lembrar que o local storage armazena um valor do tipo string e para atualizar esse
 * valor basta sobreescreve-lo. 
 * E para fazer isso, temos que atualizar a nossa array...
 * 
 * então, se o elemento já existe, precisamos da posição atual desse elemento
 * e a posição é o nossso existe.id e ele vai ter armazenado o itemAtual.
 */


itens[existe.id] = itemAtual;

/**
 * e o if ficara assim
 */

if(existe) {
    itemAtual.id = existe.id;
    
    atualizaElemento(itemAtual);

    itens[existe.id] = itemAtual;

}else{
    itemAtual.id = itens.length;

    criaElemento(itemAtual);

    itens.push(itemAtual);
}

/**
 * agora, vamos tratar a interface para conseguirmos remover os elementos da lista.
 * 
 * nota sobre arrow function: a arrow function não reconhece o this. Por isso, quando for necessário
 * usar o this, é melhor que seja declarada uma função.
 * 
 * nota sobre padrão de construção de lógica: aqui, usamos o padrão de primeiro modificar a interface e depois
 * modificar o comportamento do local storage.
 * 
 */

function botaoDeleta() {
    const elementoBotao = document.createElement("button"); //createElement() cria um elemento no html.
    elementoBotao.innerText = "X"; //.innerText adiciona um texto no elemento criado.
}

/**
 * iniciamos declarando a função que crira o botão na interface
 */


function botaoDeleta() {
    const elementoBotao = document.createElement("button"); 
    elementoBotao.innerText = "X";

    elementoBotao.addEventListener("click", function() {
        deletaElemento(this.parentNode);
    })

    return elementoBotao
}

/**
 * repare, declaramos uma função anonima dentro da função delcarada botaoDeleta.
 * 
 * Isso, porque quando criamos elementos html via js, nós não conseguimos atrelar nenhum escutador de evento
 * nele, fora da função de onde ele foi criado. Então, esses escutadores, devem estar declarados 
 */

/**
 * então, chamando o nosso <button> de tag criamos a função de remover o item da nossa lista
 */

function deletaElemento(tag) {
    tag.remove();
}

/**
 * feito tudo isso, agora falta fazer um novo appendChild lá no novoItem, 
 * na função criaElemento, para que os botões sejam
 * criados junto com os itens adicionados na lista
 */

function criaElemento(item) {

    const novoItem = document.createElement("li");
    novoItem.classList.add("item");

    const numeroItem = document.createElement("strong");
    numeroItem.innerHTML = item.quantidade;
    numeroItem.dataset.id = item.id 

    novoItem.appendChild(numeroItem);
    novoItem.innerHTML += item.nome;
    novoItem.appendChild(botaoDeleta ()); // esse aqui

    lista.appendChild(novoItem);

}

/**
 * para deletar os itens do local storage, precisamos primeiro, encontrar os itens dentro do objeto.
 * pois dentro do objeto eles estão recebendo um identificador. Então é esse identificador que precisamos
 * passar primeiro para as funções de deletar o item. Só vamos conseguir editar o local storage, se o objeto
 * estiver atualizado. Caso contrário ao recarregar a pagina, o objeto não atualizado, será carregado no 
 * local storage novamente.
 * 
 * para isso, vamos buscar o identificador e passar para as funções aonde o deletaBotao é chamado
 */

function deletaElemento(tag, id) { // então, o deletaBotao recebe a tag e o id
    tag.remove();

    itens.splice(itens.findIndex(elemento => elemento.id === id), 1);
    //fazemos um splica no itens. findIndex, procura qual é a posição do elemento (0, 1, 2...)
    // então se a posição do elemento for identica ao do id do elemento, deletamos um item 
    // apartir daquela posição.

    localStorage.setItem("itens", JSON.stringify(itens));
    //repetimos o stringfy para escrever no local storage
}

/**
 * agora, a function botaoDeleta, tambem deve receber o id
 */

function botaoDeleta(id) { // como parâmetro da função
    const elementoBotao = document.createElement("button"); 
    elementoBotao.innerText = "x";

    elementoBotao.addEventListener("click", function() {
        deletaElemento(this.parentNode, id); //  passada para o elemento botão para identificar qual o botão que foi clicado
    })

    return elementoBotao
}

/**
 * e então, chamaremos botaoDeleta lá no criaElemento, para que o botão seja criado, junto com o elemento da lista
 * e quando chamarmos o deletaBotao, também devemos passar o id do item
 */

function criaElemento(item) {

    const novoItem = document.createElement("li");
    novoItem.classList.add("item");

    const numeroItem = document.createElement("strong");
    numeroItem.innerHTML = item.quantidade;
    numeroItem.dataset.id = item.id 

    novoItem.appendChild(numeroItem);
    novoItem.innerHTML += item.nome;
    novoItem.appendChild(botaoDeleta (item.id)); //assim

    lista.appendChild(novoItem);

}

/**
 * se o id não for passado aqui para o appendChild quando chamamos o botaoDeleta os itens serão deletados
 * apartir dos itens que foram adicionados por ultimo, no objeto.
 * 
 * porque? não sei ainda....
 */


/**
 * para atualizarmos o objeto e fazermos com que os identificadores não se repitam nós fazemos
 */

if(existe) {
    itemAtual.id = existe.id;
    
    atualizaElemento(itemAtual);

    itens[itens.findIndex(elemento => elemento.id === existe.id)] = itemAtual;
    //procura o elemento e o id do elemento ve se ele existe e se existir será o item atual

}else{
    itemAtual.id = itens[itens.length -1] ? (itens[itens.length-1]).id +1 : 0;
    //pega o id do item atual e na posição -1 incrementa 1 ou se a array estiver vazia ele é zero

    criaElemento(itemAtual);

    itens.push(itemAtual);
}
```  