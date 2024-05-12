# Horário de Atendimento do Professor

* Terças e quintas das 8h às 16h podem contar comigo!
* Demais dias da semana, estou no Slack, com tempo de respostas no mesmo dia.

# Horário de Atendimento do Monitor

* Segunda: 8h30 às 10h
* Quarta: 8h30 às 10h
* Sexta: 8h30 às 9h30

# Back-end II - Endpoints de leitura e escrita com documentação própria

Nesta segunda instrução sobre back-end, aprofundaremos e construiremos relações de tabelas 1:N.

Orientações iniciais:

1) Se seu Visual Code não estiver com salvamento automático, sempre digite **Ctrl+S** para salvar cada arquivo.
2) Ainda estamos entendendo de back-end. Então, tudo que veremos aqui de front-end será uma espécie de caixa preta, mas nas próximas instruções, teremos mais tempo e calma para tratar disso.

## Etapa 1 - Criando um novo projeto Sails

a) Abra o terminal e navegue até o diretório onde deseja criar o projeto.
   
b) Execute o comando **sails new superheroes**. O comando genérico para criar um novo projeto em Sails: **sails new nomeProjeto**.
   - Este comando criará um novo projeto Sails com o nome especificado.
     
c) Quando solicitado a escolher um modelo, selecione a opção **2 - Empty**.
   - Isso criará um projeto vazio sem qualquer configuração pré-definida.

d) Ainda no terminal, entre na pasta recém criada do projeto e digite **code .** para carregar o projeto inteiro no Visual Code.

## Atenção para as pastas importantes

* api
   * controllers --> responsável por lidar com as requisições HTTP relacionadas às operações CRUD (Create, Read, Update, Delete) em uma entidade chamada "Heroe".

* api
   * models --> vamos criar o heroe.js e guns.js

* config
   * datastore.js --> credenciais do seu banco Render

* config
   * models.js --> model pai de todos os models. O que for feito aqui, será feito em todos os models

* config
   * routes.js --> são rotas que determinam como o aplicativo deve responder a diferentes requisições HTTP que chegam ao servidor.

* views
   * pasta que você criará **hero**, **gun**, **herogun** --> será os seus front-ends 

* package.json (penúltimo arquivo) --> indica como estão as instâncias necessárias para o seu projeto.

## Etapa 2 - Configurando o datastores.js

a) Vá na seguinte pasta do seu projeto Sails:
* **Config**
	* **datastores.js** (são as suas conexões de qualquer banco, tipo MySQL, PostgreSQL, etc)
 		* **linha 51** onde tem ```// adapter: 'sails-mysql',```, **apague** e substitua por ```adapter: 'sails-postgresql',``` sem as // barras
   		* **linha 52** adicione a sua URL do seu Render como assim ```url: 'postgres://bdgodoi_user:ZmTVzKJXGWyB65nRGeW7S2AkMUEI3gZ1@dpg-cojpieu3e1ms73bflb6g-a.oregon-postgres.render.com/bdgodoi',```
       		* **linha 53** adicione ```ssl: true```

<picture>
   <source media="(prefers-color-scheme: light)" srcset="https://github.com/agodoi/m02-semana03b/blob/main/imgs/sails_datastores.png">
   <img alt="DataStores" src="[YOUR-DEFAULT-IMAGE](https://github.com/agodoi/m02-semana03b/blob/main/imgs/sails_datastores.png)">
</picture>

## Etapa 3 - Configure o package.json

a) Busque pelo arquivo **package.json** (penúltimo arquivo do menu vertical da esquerda). Aí dentro tem todas as dependências. Como estamos trabalhando com o **PostgreSQL**, não há pacotes default para ele. Então temos que instalar manualmente. 

b) Para instalar a biblioteca do **PostgreSQL** no seu projeto, digite esse comando **dentro da pasta do seu projeto** usando o terminal ```npm install sails-postgresql```. Quando terminar, vá no arquivo **package.json** que você vai encontrar o que a seta vermelha está apontando.

<picture>
   <source media="(prefers-color-scheme: light)" srcset="https://github.com/agodoi/m02-semana03b/blob/main/imgs/sails_com_postgresql.png">
   <img alt="Desespero" src="[YOUR-DEFAULT-IMAGE](https://github.com/agodoi/m02-semana03b/blob/main/imgs/sails_com_postgresql.png)">
</picture>

Caso queira acessar a documentação [https://www.npmjs.com/package/sails-postgresql](https://www.npmjs.com/package/sails-postgresql).

## Etapa 4 - Testando a conexão

No terminal, dê um ```sails lift``` na mesma pasta do seu projeto.

Caso apareça as velas, isto é, sails, sucesso na conexão!!! Se você for no seu DBeaver e conferir o que está no seu banco de dados, vai notar que não mudou nada em suas tabelas porque até o momento não criamos nenhum **model** novo nesse projeto.

<picture>
   <source media="(prefers-color-scheme: light)" srcset="https://github.com/agodoi/m02-semana04/blob/main/imgs/velaOk.png">
   <img alt="Velas" src="[YOUR-DEFAULT-IMAGE](https://github.com/agodoi/m02-semana04/blob/main/imgs/velaOk.png)">
</picture>

## Etapa 5 - Criando um model (tabela)

a) No seu terminal, dê um **Ctrl+C** e dentro da pasta do projeto, digite ```sails generate model heroe``` (enter). Ele vai criar um arquivo chamado **Hero** no modelo Sails na pasta **\api\models**. Você poderia criar um arquivo manualmente **Hero.js** sem problemas com o botão direito do mouse. Detalhe: mesmo que você coloque **hero** com **h** minúsculo, o Sails vai transformar para **H** maiúsculo se usar o comando **sails generate model**.

b) Confira em **api/models** se seu arquivo **Hero.js** foi criado e pode deletar o arquivo **.gitkeep**. Não vamos utilizá-lo. Esse arquivo aparece para manter a pasta não-vazia.

c) Limpe tudo que estiver dentro do **Hero.js**, copie e cole esse código dentro do **api/models/Hero.js**

```
module.exports = {
  attributes: {
    name: {
      type: "string",
      required: true,
    },
    power: {
      type: "string",
      required: true,
    },
    age: {
      type: "number",
      required: true,
    },
    secretIdentity: {
      type: "string",
      required: true,
    },
  },
};
```
d) Os desenhos Sails que estão em forma de comentários, você pode apagar.

e) Depois, digite ```sails lift``` no terminal e dentro da pasta do seu projeto;

f) O prompt vai te perguntar se você quer trabalhar como:

**1) FOR DEV**	--> 	faz *alter* (adiciona dados), cria tabelas, mas não deleta colunas **ESCOLHER ESSA OPÇÃO**

**2) TESTS**	--> 	faz *drop*, zera tudo em cascata (devido à primary key e foreing key) e recomeça a tabela.

**3) STAGING** 	--> 	é para quando você está em **produção**, pois ele não altera o banco. Trabalhar em **produção** é quando usuários reais utilizam seu projeto.


Escolha a opção **1) FOR DEV**. 

g) Aguarde o aparecimento da Vela do Sails no seu terminal e vá no seu DBeaver e confira se subiu a nova tabela.

Se tudo correu bem, você terá em seu Dbeaver algo assim como na figura:

<picture>
   <source media="(prefers-color-scheme: light)" srcset="https://github.com/agodoi/m02-semana04/blob/main/imgs/dbeaver_ok.png">
   <img alt="DBeaver OK!" src="[YOUR-DEFAULT-IMAGE](https://github.com/agodoi/m02-semana04/blob/main/imgs/dbeaver_ok.png)">
</picture>


# Etapa 6 - Ajustando o pai dos Models

a) Vá no arquivo config/models.js e cole essa atualização. Se você comparar o arquivo **config/models.js** original com essa atualização, estamos apenas removendo os comentários das linhas 38 e 56, respectivamente **schema: true,** e **migrate: 'alter',**.

```
/**
 * Default model settings
 * (sails.config.models)
 *
 * Your default, project-wide model settings. Can also be overridden on a
 * per-model basis by setting a top-level properties in the model definition.
 *
 * For details about all available model settings, see:
 * https://sailsjs.com/config/models
 *
 * For more general background on Sails model settings, and how to configure
 * them on a project-wide or per-model basis, see:
 * https://sailsjs.com/docs/concepts/models-and-orm/model-settings
 */

module.exports.models = {


  /***************************************************************************
  *                                                                          *
  * Whether model methods like `.create()` and `.update()` should ignore     *
  * (and refuse to persist) unrecognized data-- i.e. properties other than   *
  * those explicitly defined by attributes in the model definition.          *
  *                                                                          *
  * To ease future maintenance of your code base, it is usually a good idea  *
  * to set this to `true`.                                                   *
  *                                                                          *
  * > Note that `schema: false` is not supported by every database.          *
  * > For example, if you are using a SQL database, then relevant models     *
  * > are always effectively `schema: true`.  And if no `schema` setting is  *
  * > provided whatsoever, the behavior is left up to the database adapter.  *
  * >                                                                        *
  * > For more info, see:                                                    *
  * > https://sailsjs.com/docs/concepts/orm/model-settings#?schema           *
  *                                                                          *
  ***************************************************************************/

  schema: true,


  /***************************************************************************
  *                                                                          *
  * How and whether Sails will attempt to automatically rebuild the          *
  * tables/collections/etc. in your schema.                                  *
  *                                                                          *
  * > Note that, when running in a production environment, this will be      *
  * > automatically set to `migrate: 'safe'`, no matter what you configure   *
  * > here.  This is a failsafe to prevent Sails from accidentally running   *
  * > auto-migrations on your production database.                           *
  * >                                                                        *
  * > For more info, see:                                                    *
  * > https://sailsjs.com/docs/concepts/orm/model-settings#?migrate          *
  *                                                                          *
  ***************************************************************************/

  migrate: 'alter',

  //quando você escolhe a opção DEV na etapa 5, aqui fica 'alter' 
  //mas se você escolher TESTS, ele fica como 'drop'
  //o correto é deixar 'alter' enquanto vc está desenvolvendo porque
  //assim ele não vai apagar tudo todas as vezes que vc der um sails lift
  //ou mandar preenher o seu formlário de entrada que está no model


  /***************************************************************************
  *                                                                          *
  * Base attributes that are included in all of your models by default.      *
  * By convention, this is your primary key attribute (`id`), as well as two *
  * other timestamp attributes for tracking when records were last created   *
  * or updated.                                                              *
  *                                                                          *
  * > For more info, see:                                                    *
  * > https://sailsjs.com/docs/concepts/orm/model-settings#?attributes       *
  *                                                                          *
  ***************************************************************************/

  attributes: {
    createdAt: { type: 'number', autoCreatedAt: true, },
    updatedAt: { type: 'number', autoUpdatedAt: true, },
    id: { type: 'number', autoIncrement: true, },
  },

  dataEncryptionKeys: {
    default: 'Ts8sNZeopBy8tvyP4C3Ntmz9T3Y3NK7g5sitCe7M9nk='
  },

  cascadeOnDestroy: true

};
```

## Etapa 7 - Criando um controller

a) Vá no seu terminal, dentro da pasta do projeto, e se você não estiver vendo o path do seu projeto, dê um **Ctrl+C**;

b) Digite ```sails generate controller Heroes``` (nome do controller sempre no plural como boas práticas);

Deve aparecer essa mensagem: **info: Created a new controller ("Heroes") at api/controllers/HeroesController.js!**

E se você for em **api/controller** verá um novo arquivo **HeroesController.js**.

c) Note que cada arquivo gerado no Sails possui um help [https://sailsjs.com/docs/concepts/actions](https://sailsjs.com/docs/concepts/actions) onde você poderá explorar todas as opções do Sails. Nesse help você fica sabendo as ações possíveis dentro do Controller.

d) Copie esse código e cole no seu **api/controller/HeroesController.js**. Mas antes, apague tudo o que está no api/controller/HeroesController.js.

```
module.exports = {
  list: async (req, res) => {
    const heroes = await Hero.find();
    return res.json(heroes);
  },

 create: async (req, res) => {
    try {
      const hero = await Hero.create(req.body).fetch();
      return res.json(hero);
    } catch (err) {
      return res.serverError(err);
    }
  },
};
```


## Etapa 8 - Configurando routes.js

Esse arquivo serve para configurar as rotas. 

a) Copie e cole esse código para o seu routes.js e entenda o que está acontendo logo depois.

Note que vamos habilitar uma rota de GET e um POST.

```
module.exports.routes = {
  // Views
  "/": { view: "pages/homepage" },
  
  // API
  "GET /heroes": "HeroesController.list",
  "POST /heroes": "HeroesController.create",
};
```

b) Dê um **sails lift** para renderizar.

# Etapa 9 - Faça um teste de conexão

a) Dê um **sails lift** no seu terminal e escolha o **alter**.

b) Vamos fazer um INSERT no seu banco de dados Heroe sem usar comandos SQL. Para isso, instale essa extensão no seu Visual Code **Thunder Client**

<picture>
   <source media="(prefers-color-scheme: light)" srcset="https://github.com/agodoi/m02-semana05/blob/main/imgs/Thunder%20Client01.png">
   <img alt="Thunder Cliente" src="[YOUR-DEFAULT-IMAGE](https://github.com/agodoi/m02-semana05/blob/main/imgs/Thunder%20Client01.png)">
</picture>


c) Para instalar a extensão Thunder Client, vá no seu menu esquerdo vertical do Visual Code e clique no botão **Extensions** marcado pela seta, digite **Thunder** e depois clique em **Install**.


<picture>
   <source media="(prefers-color-scheme: light)" srcset="https://github.com/agodoi/m02-semana05/blob/main/imgs/ThunderCliente02.png">
   <img alt="Instalando Thunder Cliente" src="[YOUR-DEFAULT-IMAGE](https://github.com/agodoi/m02-semana05/blob/main/imgs/ThunderCliente02.png)">
</picture>


Esse programa serve para você popular o seu banco de dados de forma rápida com comandos JSON e ao mesmo tempo, testar as rotas GET e POST que estão ou **routes.js**.

c) Abra o Thunder clicando na seta.

<picture>
   <source media="(prefers-color-scheme: light)" srcset="https://github.com/agodoi/m02-semana05/blob/main/imgs/ThunderCliente03.png">
   <img alt="Instalando Thunder Cliente" src="[YOUR-DEFAULT-IMAGE](https://github.com/agodoi/m02-semana05/blob/main/imgs/ThunderCliente03.png)">
</picture>

d) Siga os seguintes passos:

<picture>
   <source media="(prefers-color-scheme: light)" srcset="https://github.com/agodoi/m02-semana05/blob/main/imgs/ThunderClient04v2.png">
   <img alt="Usando Thunder Client" src="[YOUR-DEFAULT-IMAGE](https://github.com/agodoi/m02-semana05/blob/main/imgs/ThunderClient04v2.png)">
</picture>

d.1) Seta vermelha, clique no botão **New Request** (seta vermelha)

d.2) Seta amarela, clique em **Body**

d.3) Seta verde, no campo **JSON Content** cole o código abaixo
```
{
  "name": "Homem Aranha",
  "power": "Agilidade e sensitivo",
  "age": 25,
  "secretIdentity": "Peter Parker"
}
```

d.4) Seta laranja, escolha a opção **POST**

d.5) Seta azul, no campo **Enter URL** digite **http://localhost:1337/heroes**

d.6) Seta roxa, clique em **Send**.

d.7) Vá no seu **DBeaver**, e confira se populou os dados da etapa **d.3**.

d.8) Faça um novo POST usando esses dados no **JSON Content**:

```
{
  "name": "Batman",
  "power": "Rico, Artes Marciais",
  "age": 34,
  "secretIdentity": "Bruce Wayne"
}
```

d.9) Confira se o Batman está no seu banco de dados. Use o DBeaber.

d.10) Volte na seta laranja, altere **POST** para **GET** e clique em **Send** e você terá à sua direita, os dados que estão no seu banco de dados do Render.

# Etapa 10 - Desenvolvendo o Layout do Front-end

a) Vá no arquivo views/layouts/layout, na linha 109 conforme o local está apontado pela seta, cole esse código:

<picture>
   <source media="(prefers-color-scheme: light)" srcset="https://github.com/agodoi/m02-semana05/blob/main/imgs/layout01.png">
   <img alt="Layout" src="[YOUR-DEFAULT-IMAGE](https://github.com/agodoi/m02-semana05/blob/main/imgs/layout01.png)">
</picture>

```
<script src="/dependencies/sails.io.js"></script>
<!--SCRIPTS END-->

<!-- CSS -->
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/css/bootstrap.min.css" rel="stylesheet">
<!-- JS com Popper -->
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/js/bootstrap.bundle.min.js"></script>
```

Deixe o **</body>** e **</html>** no final de tudo sem mexer.

# Etapa 11 - Desenvolvendo uma View: homepage.ejs

a) Vá no arquivo **views/pages/homepage.ejs** seleciona tudo que é original, apague e cole esse código:

```
<div class="container">
  <h1>List of Heroes</h1>
  <table class="table table-striped">
    <thead>
      <tr>
        <th>Id</th>
        <th>Name</th>
        <th>Power</th>
        <th>Age</th>
        <th>Secret Identity</th>
      </tr>
    </thead>
    <tbody id="heroes-list"></tbody>
  </table>
</div>

<script>
  fetch('/heroes')
    .then(response => response.json())
    .then(data => {
      const heroesList = document.getElementById('heroes-list');
      data.forEach(hero => {
        const row = document.createElement('tr');
        row.innerHTML = `
                        <td>${hero.id}</td>
                        <td>${hero.name}</td>
                        <td>${hero.power}</td>
                        <td>${hero.age}</td>
                        <td>${hero.secretIdentity}</td>
                    `;
        heroesList.appendChild(row);
      });
    })
    .catch(error => console.error('Error fetching heroes:', error));
</script>
```

b) Dê um **Sails lift**

c) Digite **localhost:1337** no seu navegador e deve encontrar essa tela:

COLOCA IMAGEM DO LIST OF HEROES


# Etapa 12 - Desenvolvendo uma page: addhero.ejs

a) Adicione um arquivo em branco no diretório **views/pages/** conforme mostra a figura. O nome do arquivo é **addhero.ejs**.


<picture>
   <source media="(prefers-color-scheme: light)" srcset="https://github.com/agodoi/m02-semana05/blob/main/imgs/addheroe01.png">
   <img alt="Layout" src="[YOUR-DEFAULT-IMAGE](https://github.com/agodoi/m02-semana05/blob/main/imgs/addheroe01.png)">
</picture>

b) E dentro do arquivo **views/pages/addhero.ejs**, cole esse código:

```
<div class="container">
  <h1>Add New Hero</h1>
  <form id="add-hero-form">
    <div class="form-group">
      <label for="name">Name:</label>
      <input type="text" class="form-control" id="name" name="name" required>
    </div>
    <div class="form-group">
      <label for="power">Power:</label>
      <input type="text" class="form-control" id="power" name="power" required>
    </div>
    <div class="form-group">
      <label for="age">Age:</label>
      <input type="number" class="form-control" id="age" name="age" required>
    </div>
    <div class="form-group">
      <label for="secretIdentity">Secret Identity:</label>
      <input type="text" class="form-control" id="secretIdentity" name="secretIdentity" required>
    </div>
    <button type="submit" class="btn btn-primary m-2">Submit</button>
  </form>
</div>

<script>
  // Adiciona um evento de envio ao formulário
  document.getElementById('add-hero-form').addEventListener('submit', function (event) {
    event.preventDefault(); // Evita que o formulário seja enviado

    // Obtém os valores do formulário
    const name = document.getElementById('name').value;
    const power = document.getElementById('power').value;
    const age = document.getElementById('age').value;
    const secretIdentity = document.getElementById('secretIdentity').value;

    // Envia uma solicitação POST para adicionar um novo herói
    fetch('/heroes', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({
        name: name,
        power: power,
        age: age,
        secretIdentity: secretIdentity
      })
    })
      .then(response => response.json())
      .then(data => {
        alert('Hero added successfully!');
        window.location.href = '/';
      })
      .catch(error => console.error('Error adding hero:', error));
  });
</script>
```

c) Volte no arquivo **config\routes.js** e adicione a lista **"GET /addhero": { view: "pages/addhero" },** da forma que está sendo mostrado:

```
module.exports.routes = {
  // Views
  "/": { view: "pages/homepage" },
  "GET /addhero": { view: "pages/addhero" },
  
  // API
  "GET /heroes": "HeroesController.list",
  "POST /heroes": "HeroesController.create",
};
```


d) Dê um **sails l** (sails lift)

e) digite na URL do seu navegador: **localhost:1337/addhero** e vai aparecer essa tela

f) Preencha com os dados que você quiser e clique em **Submit**

g) Vai aparecer a tela homepage com os novos dados que você inseriu.

g) Vá no seu DBeaver e confira se populou lá também. Tem que estar lá também.

COLOCA IMAGEM ADD HERO


# Etapa 13 - Adicionando um novo Model. Vamos fazer 1:N

Vamos adicionar um novo model, chamado **Guns.js** e amarrar com o model **Heroe.js**.

a) Vá no seu terminal, e dentro da pasta do seu projeto, digite **sails generate model guns**.

b) Vá no path **api/models/Guns.js**, limpa tudo o que está lá e cole esse código:

```
module.exports = {
  attributes: {
    name: { type: "string", required: true },
    type: { type: "string", required: true },

    // Add a reference to Hero (foreing key do heroe)
    owner: {
      model: "hero",
      required: true,
    },
  },
};
```
c) Não esquece de salvar o seu projeto.

# Etapa 14 - Criando um Controller

Todo Model possui o seu Controller separado.

a) Então, vá no seu terminal e dentro da sua pasta digite **sails generate controller guns**.

b) Vá no path **api/controller/GunsController.js**, limpe tudo que estiver lá originalmente e cole esse código:

```
module.exports = {
  create: async function (req, res) {
    try {
      const gun = await Gun.create(req.body).fetch();
      return res.json(gun);
    } catch (err) {
      return res.serverError(err);
    }
  },
};
```

c) Dê um novo **sails l** no seu diretório.

d) Vá no seu DBeaver e veja se essa entidade (tabela) foi devidamente criada.

# Etapa 15 - Ajustando o Routes.js

Depois que você criou uma nova entidade, o **routes.js** precisa ter a rota de POST para ele.

a) Vá no **config/routes.js** e adicione a linha **"POST /gun": "GunsController.create",** da forma que está sendo mostrado:


```
module.exports.routes = {
  // Views
  "/": { view: "pages/homepage" },
  "GET /addhero": { view: "pages/addhero" },

  
  // API
  "GET /heroes": "HeroesController.list",
  "POST /heroes": "HeroesController.create",
  "POST /gun": "GunsController.create",
};
```
# Etapa 16 - Criando uma nova página (page) addgun.ejs

Precisamos de um novo **views/pages/** que terá o nome de **addgun.ejs**.

a) Para criar pages, não tem comando Sails. Precisa fazer manualmente como você fez na **Etapa 12**.

b) Depois de adicionar um novo arquivo em **views/pages/** chamado **addgun.ejs**, limpe tudo que está originalmente lá e cole esse script que é um formulário:

```
<div class="container">
  <h1>Add New Gun</h1>
  <form id="add-gun-form">
    <div class="form-group">
      <label for="name">Name:</label>
      <input type="text" class="form-control" id="name" name="name" required>
    </div>
    <div class="form-group">
      <label for="type">Type:</label>
      <input type="text" class="form-control" id="type" name="type" required>
    </div>
    <div class="form-group">
      <label for="owner">Owner:</label>
      <select class="form-control" id="owner" name="owner" required>
        <!-- Opções de heróis serão adicionadas aqui dinamicamente -->
      </select>
    </div>
    <button type="submit" class="btn btn-primary">Submit</button>
  </form>
</div>

<script>
  // Faz uma solicitação GET para obter os heróis disponíveis
  fetch('/heroes')
    .then(response => response.json())
    .then(data => {
      // Para cada herói na resposta, adiciona uma opção à caixa de seleção de proprietários
      const ownerSelect = document.getElementById('owner');
      data.forEach(hero => {
        const option = document.createElement('option');
        option.value = hero.id;
        option.textContent = hero.name;
        ownerSelect.appendChild(option);
      });
    })
    .catch(error => console.error('Error fetching heroes:', error));

  // Adiciona um evento de envio ao formulário
  document.getElementById('add-gun-form').addEventListener('submit', function (event) {
    event.preventDefault(); // Evita que o formulário seja enviado

    // Obtém os valores do formulário
    const name = document.getElementById('name').value;
    const type = document.getElementById('type').value;
    const owner = document.getElementById('owner').value;

    // Envia uma solicitação POST para adicionar uma nova arma
    fetch('/gun', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({
        name: name,
        type: type,
        owner: owner
      })
    })
      .then(response => response.json())
      .then(data => {
        alert('Gun added successfully!');
        // Redireciona para a página de listagem de armas
        window.location.href = '/';
      })
      .catch(error => console.error('Error adding gun:', error));
  });
</script>
```
### Fica tranquilo que vamos falar mais sobre front-end nas próximas instruções.

# Etapa 17 - Atualizando o routes.js

Devido à criação da última página **addgun**, temos que atualizar o **routes.js**.

a) No arquivo **config/routes.js** acrescente a linha **"GET /addgun": { view: "pages/addgun" },** conforme mostrado:

```
module.exports.routes = {
  // Views
  "/": { view: "pages/homepage" },
  "GET /addhero": { view: "pages/addhero" },
  "GET /addgun": { view: "pages/addgun" },
  
  // API
  "GET /heroes": "HeroesController.list",
  "POST /heroes": "HeroesController.create",
  "POST /gun": "GunsController.create",
};
```
b) Dê um **sails l** no seu terminal.

c) Vá na URL do seu navegador e digite **localhost:1337/addgun** e preencha com os dados que desejar.

Note que a coluna **Owner** está puxando dados da tabela **Heroe**.

d) Dê o **Submit** da página que você está vendo (Add Gun).

e) Você observará a homepage devido à esse comando **window.location.href = '/';** da linha 63 de **views/pages/addgun.ejs**

f) Mas você não estará vendo a coluna **gun** que você acabou de adicionar.

# Etapa 18 - Criando uma nova página

Nessa nova página vamos listar as **guns** juntamente com os **heroes**.

a) Novamente, para criar pages, não tem comando Sails. Precisa fazer manualmente como você fez na **Etapa 12**.

b) Depois de adicionar um novo arquivo em **views/pages/** chamado **herogun.ejs**, limpe tudo que está originalmente lá e cole esse script que é um formulário:

```
<div class="container">
  <h1>List of Guns and Heroes</h1>
  <table class="table table-striped">
    <thead>
      <tr>
        <th>Id</th>
        <th>Name</th>
        <th>Power</th>
        <th>Age</th>
        <th>Secret Identity</th>
        <th>Guns</th>
      </tr>
    </thead>
    <tbody id="heroes-list"></tbody>
  </table>
</div>

<script>
  fetch('/heroesandguns')
    .then(response => response.json())
    .then(data => {
      const heroesList = document.getElementById('heroes-list');
      data.forEach(hero => {
        const row = document.createElement('tr');
        let guns = ''; // Inicializa a variável para armazenar os nomes das armas

        // Verifica se o herói possui armas associadas
        if (hero.guns && hero.guns.length > 0) {
          guns = hero.guns.map(gun => gun.name).join(', '); // Obtém os nomes das armas do herói
        }

        row.innerHTML = `
          <td>${hero.id}</td>
          <td>${hero.name}</td>
          <td>${hero.power}</td>
          <td>${hero.age}</td>
          <td>${hero.secretIdentity}</td>
          <td>${guns}</td>
        `;

        heroesList.appendChild(row);
      });
    })
    .catch(error => console.error('Error fetching heroes:', error));
</script>
```

# Etapa 19 - Atualizando o routes.js

Devido à criação da última página **heroegun**, temos que atualizar o **routes.js**.

a) Vá em **config/routes.js** e adicione **"GET /herogun": { view: "pages/herogun" },** como mostra a seguir:

```
module.exports.routes = {
  // Views
  "/": { view: "pages/homepage" },
  "GET /addhero": { view: "pages/addhero" },
  "GET /addgun": { view: "pages/addgun" },
  "GET /herogun": { view: "pages/herogun" },
  
  // API
  "GET /heroes": "HeroesController.list",
  "POST /heroes": "HeroesController.create",
  "POST /gun": "GunsController.create",
};
```
b) Dê um **sails l** no seu terminal.

c) Carregue a URL **localhost:1337/herogun** no seu nevegador, uma tabela será carregada, mas não vai aparecer a última coluna da direita **Guns**.

O problema é que no HeroesController.js só está trazendo os heróis e não está trazendo as armas.

d) Então, adicione mais uma linha no **config/routes.js** que é **"GET /heroesandguns": "HeroesController.listwithgun",** resultando finalmente em:

```
module.exports.routes = {
  // Views
  "/": { view: "pages/homepage" },
  "GET /addhero": { view: "pages/addhero" },
  "GET /addgun": { view: "pages/addgun" },
  "GET /herogun": { view: "pages/herogun" },

  // API
  "GET /heroes": "HeroesController.list",
  "GET /heroesandguns": "HeroesController.listwithgun",
  "POST /heroes": "HeroesController.create",
  "POST /gun": "GunsController.create",
};
```

# Etapa 20 - Atualizando o HeroesController.js

Para que a homepage liste os itens da entendade **gun.js**, tem que mexer no **HeroesController.js**.

a) Vá no arquivo **api/controller/HeroesController** e cole essa atualização abaixo. As mudanças são da linha 6 à 19:

```
module.exports = {
  list: async (req, res) => {
    const heroes = await Hero.find();
    return res.json(heroes);
  },
  listwithgun: async (req, res) => {
    try {
      const query = "SELECT * FROM hero INNER JOIN gun ON hero.id = gun.owner";
      const heroes = await Hero.getDatastore().sendNativeQuery(query);

      if (heroes.rows && heroes.rows.length > 0) {
        return res.json(heroes.rows);
      } else {
        return res.json([]);
      }
    } catch (err) {
      return res.serverError(err);
    }
  },
  create: async (req, res) => {
    try {
      const hero = await Hero.create(req.body).fetch();
      return res.json(hero);
    } catch (err) {
      return res.serverError(err);
    }
  },
};
```
b) Dê um **sails l** para renderizar tudo novamente.

c) Agora, você pode fazer novos testes da seguinte forma:

c.1) Digite **localhost:1337** na URL do navegador e você vai puxar os dados atuais;

c.2) Digite **localhost:1337/addhero** para adicionar mais um super-herói;

c.3) Digite **localhost:1337/addgun** para adicionar mais uma arma;

c.4) Confira no seu DBeaver se a população está atualizada;

c.5) Digite **localhost:1337/herogun** para conferir como a tabela final veio relacionada.




# Resolvendo problemas

1) Se você contrar essa mensagem **> Is something else already running on port 1337 ?** significa que você está rodando um outra aplicação Sails. Cancele uma delas.




