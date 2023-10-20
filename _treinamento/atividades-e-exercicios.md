# Treinamento | atividades e exercícios

Aqui ficam todas as atividades e exercicios deste treinamento.

## Módulo 1: Introdução a StackSpot

1. Criar uma conta Personal na StackSot;
2. Instalar a CLI;
3. Logar na conta via CLI;

## Módulo 2: Criando plugin base e conceitos básicos

Neste módulo iremos criar o plugin base para construção microsserviços que expõe APIs REST com Spring Boot e Java.

### Exercícios

1. Acesse o site do [Spring Initiliazr](https://start.spring.io/) e crie um projeto com Spring Boot com os seguintes requisitos:

    - Maven
    - Java 17
    - Spring Boot 3.x
    - Packaging: Jar
    - Dependencies:
        - Spring Web
        - Validation
        - Spring Boot DevTools

2. Faça o download do projeto (.zip) e descompate-o em algum diretório da sua máquina.

3. Agora vamos criar nosso estúdio que se chamará `popcorn-studio`. Para isso, crie o diretório `popcorn-studio`:

```sh
mkdir popcorn-studio
```

4. Dentro do diretório do estúdio, vamos criar nosso primeiro plugin via CLI da StackSpot. Basta executar o comando abaixo e responder as questões solicitadas pela comando:

```sh
stk create plugin popcorn-springboot-base-plugin
```

5. Ainda dentro do diretório do estúdio, abra-o com seue editor de texto preferido. Se estiver utilizando o Visual Studio Code (VsCode), basta executar o comando abaixo:

```sh
code .
```

6. Agora vamos fazer as configurações básicas do nosso plugin. Para isso, dentro do diretório `popcorn-springboot-base-plugin`, abra o arquivo `plugin.yaml` e edite os atributos `display-name`, `description` e  `technologies` como abaixo:

```yaml
metadata:
    display-name: Spring Boot Base plugin
    description: Plugin base para construção de APIs REST com Spring Boot

# ...

spec:
    # ...
    technologies:
        - Api
        - Java
        - Spring Boot
```

7. Agora, copie o conteúdo do projeto que criamos no site do Spring Initializr para o diretório `templates` do nosso plugin. É importante ter cuidado com os arquivos ocultos, pois eles também precisam ser copiados. Portanto, basta executar o comando abaixo:

```sh
cp -R <diretorio-do-projeto>/.* /plugin/templates
```

> 💡 **Dica**: Outra forma seria renomear o projeto do Spring Initializr para "templates" e cópia-lo por cima do diretório `templates` do plugin.

8. Agora, precisamos criar os inputs do nosso plugin. Para isso, dentro do arquivo `plugin.yaml` adicione os seguintes inputs:

```yaml
inputs:
    - label: Nome da aplicação
      name: project_name
      type: text
      required: true
    - label: Group_Id do Maven
      name: project_group_id
      type: text
      required: true
      pattern: '(^[a-zA-Z_\d.]*[a-zA-Z_\d]$)'
      default: "br.com.zup.popcornstudio"
    - label: Artifact_Id do Maven
      name: project_artifact_id
      type: text
      required: true
      help: A propriedade artifact_id do Maven é utilizada como nome do .jar da aplicação
      pattern: ^[a-z]([a-z0-9]|_)*$
    - label: Versão do Spring Boot
      name: project_springboot_version
      type: text
      default: 3.1.4
      required: true
    - label: Versão do Java
      name: project_java_version
      type: text
      items:
        - "17"
        - "21"
      default: 17
      required: true
```

9. Agora, para ter certeza que os inputs foram preenchidos corretamente, vamos validá-los com a CLI e também imprimi-los no arquivo `README.md` dentro de `templates`:

```sh
stk validate plugin
# Não deve haver erros ou warning aqui
```

E para imprimi-los, basta editar o arquivo `README.md` com o seguinte conteúdo:

```
### Inputs

- project_name: {{project_name}}
- project_group_id: {{project_group_id}}
- project_artifact_id: {{project_artifact_id}}
- project_springboot_version: {{project_springboot_version}}
- project_java_version: {{project_java_version}}
```

10. O próximo passo pedir para CLI aplicar o plugin em algum diretório vazio na nossa máquina. Dessa forma, **fora da estúdio**, crie um diretório chamado "popcorn-demo-teste" e execute o seguinte comando da CLI:

```sh
# diretório popcorn-demo-teste está na mesma hieraquia do diretório do estúdio
stk apply plugin ../popcorn-studio/popcorn-springboot-base-plugin
```

Responda as perguntas solicitadas pelo plugin e em seguida verifique se os inputs foram impressos corretamante no arquivo `README.md` gerado pelo plugin:

```sh
cat README.md 
```








