# Treinamento | atividades e exercícios

Aqui ficam todas as atividades e exercicios deste treinamento. É importante que você treine as habilidades necessárias para construção de estúdios e plugins da StackSpot.

Em cada módulo você exercitará de forma progressiva conceitos básicos e fundamentais na StackSpot que te habilitarão a construir e distribuir padrões de microsserviços via StackSpot. Além disso, você também aprenderá dicas que te ajudarão em decisões de design, implementação e também em validações e testes de qualidade para cada plugin criado. 

## Módulos de estudos

Estes são as atividades e exercicíos de cada módulo que iremos estudar e treinar a fim de dominar a construção de plugins de aplicação (`app`) na Stackspot: 

1. [Módulo 1: Introdução a StackSpot](#módulo-1-introdução-a-stackspot)

2. [Módulo 2: Criando plugin base e conceitos básicos](#módulo-2-criando-plugin-base-e-conceitos-básicos)

3. [Módulo 3: Habilitando capacidade de monitoramento e health checking](#módulo-3-habilitando-capacidade-de-monitoramento-e-health-checking)

4. [Módulo 4: Habilitando capacidade de persistência em bancos de dados relacionais](#módulo-4-habilitando-capacidade-de-persistência-em-bancos-de-dados-relacionais)


## Módulo 1: Introdução a StackSpot

Neste módulo criaremos nossa conta personal na Stackspot, instalaremos a CLI na nossa máquina e por fim faremos o login na nossa conta pela CLI.

### Exercícios

Para isso, acesse o site e siga as atividades abaixo:

1. Criar uma conta Personal na StackSot
    * Acesse o menu [Download](https://www.stackspot.com/pt/download-pt) e crie sua conta personal utilizando sua conta do Github;
2. [Instalar a CLI](https://www.stackspot.com/pt/download-pt);
    * Após criar e logar na sua conta Stackspot, basta baixar a CLI de acordo com seu sistema operacional (OS);
3. Logar na conta via CLI
    * Após instalar a CLI, basta executar o seguinte comando para logar na sua conta Stackspot:
    ```sh
    stk login
    ```

Em caso de dúvidas, utilize o argumento `-h` nos comando da CLI, por exemplo:

```sh
# comandos básicos
stk -h

# entrar e sair de uma workspace
stk use workspace
stk exit workspace

# listando componentes
stk list -h
stk list workspace
stk list plugin

# criando plugins
stk create -h
stk create plugin -h

# aplicando plugins
stk apply -h
stk apply plugin -h

# publicando plugins
stk publish -h
stk publish plugin
```

Em caso de dúvidas ou para mais detalhes, acesse a [documentação oficial da StackSpot](https://docs.stackspot.com/).

## Módulo 2: Criando plugin base e conceitos básicos

Neste módulo iremos criar o plugin base para construção de microsserviços que expõe APIs REST com Spring Boot e Java. Além disso, aprenderemos conceitos importantes sobre design, construção e publição de plugins na Stackspot.

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

5. Ainda dentro do diretório do estúdio, abra-o com seu editor de texto preferido. Se estiver utilizando o **Visual Studio Code (VsCode)**, basta executar o comando abaixo dentro do diretório:

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

7. Agora, copie o conteúdo do projeto que criamos no site do Spring Initializr para o diretório `templates` do nosso plugin. **É importante ter cuidado com os arquivos ocultos, pois eles também precisam ser copiados**. Portanto, basta executar o comando abaixo:

```sh
cp -R <diretorio-do-projeto>/. /plugin/templates
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
      pattern: '^[a-zA-Z][a-zA-Z0-9_](\\.[a-zA-Z][a-zA-Z0-9_])*$'
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

9. Agora, para ter certeza que os inputs foram preenchidos corretamente, vamos validá-los com a CLI e também imprimi-los no arquivo `README.md` dentro de `templates`. Dessa forma, siga os passos abaixo:

- 9.1. Dentro do diretório do plugin `popcorn-springboot-base-plugin`, execute o comando de validação da CLI:

```sh
stk validate plugin
# Não deve haver errors ou warnings aqui
```

- 9.2. E para imprimi-los, basta editar o arquivo `README.md` dentro do diretório `templates` com o seguinte conteúdo:

```
### Inputs

- project_name: {{project_name}}
- project_group_id: {{project_group_id}}
- project_artifact_id: {{project_artifact_id}}
- project_springboot_version: {{project_springboot_version}}
- project_java_version: {{project_java_version}}
```

10. O próximo passo é pedir à CLI para aplicar o plugin em algum diretório vazio na nossa máquina. Portanto, siga os passos abaixo:

- 10.1. **Fora do estúdio**, crie um diretório chamado "`popcorn-demo-teste`" e execute o seguinte comando da CLI:

```sh
# crie o diretório e entre nele
mkdir popcorn-demo-teste; cd popcorn-demo-teste

# diretório popcorn-demo-teste está na mesma hieraquia do diretório do estúdio
stk apply plugin ../popcorn-studio/popcorn-springboot-base-plugin
```

- 10.2. Responda as perguntas solicitadas pelo plugin (inputs configurados) e em seguida verifique se os inputs foram impressos corretamante no arquivo `README.md` gerado pelo plugin:

```sh
cat README.md 
```

11. Em seguida, vamos adicionar os `computed-inputs` no arquivo `plugin.yaml` e também imprimi-los no `README.md`. Para isso, siga os passos:

- 11.1. Adicione o código abaixo no arquivo `plugin.yaml`:

```yaml
computed-inputs:
    project_name_capitalized: "{{project_name | to_camel}}"
    project_artifact_id_formatted: "{{project_artifact_id | lower | to_kebab}}"
    project_artifact_id_sanitized: "{{project_artifact_id_formatted | regex_replace('[^0-9a-zA-Z_]', '')}}"
    project_base_package: "{{project_group_id}}.{{project_artifact_id_sanitized}}"
    project_base_package_dir: "{{project_base_package | group_id_folder}}"
```

> ⚠️ **Atenção**: A propriedade `computed-inputs` deve estar na mesma hierarquia da propriedade `inputs`. Ou seja, no mesmo nível de indentação.

- 11.2. Valide o plugin com o comando da CLI abaixo:

```sh
stk validate plugin
# Não deve haver errors ou warnings aqui
```

- 11.3. Edite o `README.md` com o seguinte conteudo:

```
## Computed-inputs

- project_name_capitalized: "{{project_name_capitalized}}"
- project_artifact_id_formatted: "{{project_artifact_id_formatted}}"
- project_artifact_id_sanitized: "{{project_artifact_id_sanitized}}"
- project_base_package: "{{project_base_package}}"
- project_base_package_dir: "{{project_base_package_dir}}"
```

- 11.4. E por fim, vamos limpar o conteúdo do diretório `popcorn-demo-teste`, aplicar o plugin novamente e ver o resultado:

```sh
# limpa diretório "porpcorn-demo-teste"
rm -rf * .*

# aplica o plugin
stk apply plugin ../popcorn-studio/popcorn-springboot-base-plugin

# verifica conteúdo dos computed-inputs
cat README.md
```

12. Agora, vamos interpolar os `inputs` e `computed-inputs` nos arquivos do nosso projeto dentro do diretório de `templates`. Para isso, siga os passos abaixo:

- 12.1. Edite as tags do arquivo `pom.xml` com o seguinte conteúdo:

```xml
<parent>
    <version>{{project_springboot_version}}</version>
</parent>

<groupId>{{project_group_id}}</groupId>
<artifactId>{{project_artifact_id_formatted}}</artifactId>
<name>{{project_name}}</name>

<properties>
    <java.version>{{project_java_version}}</java.version>
</properties>
```

- 12.2. Altere o nome da classe `DemoAppApplication.java` para `{{project_name_capitalized}}Application.java`.

> ⚠️ **Atenção**: O nome da classe `DemoAppApplication.java` pode variar de acordo com os parâmetros informados no site do Spring Initializr. Mas ela é a classe anotada com `@SpringBootApplication` do Spring Boot que fica na source folder `src/main/java` do projeto.

- 12.3. Agora, dentro da classe `{{project_name_capitalized}}Application.java`, altere seu conteúdo com os inputs do nosso plugin:

```java
package {{project_base_package}};

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class {{project_name_capitalized}}Application {

	public static void main(String[] args) {
		SpringApplication.run({{project_name_capitalized}}Application.class, args);
	}

}
```

- 12.3. Faremos o mesmo para a classe de testes do projeto, chamada de `DemoAppApplicationTests.java` que fica na source folder `src/test/java` do projeto. Para isso, altere o nome da classe para `{{project_name_capitalized}}ApplicationTests.java`.

- 12.4. Dentro da classe `{{project_name_capitalized}}ApplicationTests.java`, altere seu conteúdo com os inputs do nosso plugin:

```java
package {{project_base_package}};

import org.junit.jupiter.api.Test;
import org.springframework.boot.test.context.SpringBootTest;

@SpringBootTest
class {{project_name_capitalized}}ApplicationTests {

	@Test
	void contextLoads() {
	}

}
```

- 12.5. O próximo passo é renomear o caminho (*path*) de pacotes das classes do projeto:

    -  De `src/main/java/com/example/demo` para `src/main/java/{{project_base_package_dir}}`;
    - De `src/test/java/com/example/demo` para `src/test/java/{{project_base_package_dir}}`;

> ⚠️ **Atenção**: Lembre-se que os pacotes podem variar de nome de acordo com os dados preenchidos no site do Spring Initializr.

- 12.6. Agora vamos testar se o código foi interpolado corretamente nos arquivos de `templates`. Para isso, **dentro do diretório de testes do plugin (`porpcorn-demo-teste`)**, execute os comandos abaixo:

```sh
# limpa diretório "porpcorn-demo-teste"
rm -rf * .*

# aplica o plugin
stk apply plugin ../popcorn-studio/popcorn-springboot-base-plugin

# valida conteúdo com Maven: compilando código e rodando a bateria de testes
./mvnw clean test
```

> 💡 **Dica**: Outras formas de validar o conteúdo gerado pelo plugin é importando o projeto na sua IDE preferida (como IntelliJ ou Eclipse) ou inicializando a aplicação através do comando `./mvnw spring-boot:run` do Maven.

13. Em seguida, vamos customizar nosso template da forma que entendemos que um microsserviço em Java e Spring Boot deve ser. Dessa forma, siga os passos a seguir:

- 13.1. Primeiramente vamos adicionar a dependência do OpenAPI com Swagger-UI na aplicação. Para isso, adicione a seguinte dependência do `springdoc-openapi` no arquivo `pom.xml` da aplicação:

```xml
<dependency>
    <groupId>org.springdoc</groupId>
    <artifactId>springdoc-openapi-starter-webmvc-ui</artifactId>
    <version>2.1.0</version>
</dependency>
```

- 13.2. O próximo passo é converter os arquivos de configuração do Spring Boot da aplicação para o formado YAML:
   -  Renomeie o arquivo `application.properties` para `application.yaml`;
   -  Renomeie o arquivo `application-test.properties` para `application-test.yaml` (se ele não existir, crie-o dentro do diretório `src/test/resources`);

- 13.3. Agora, altere o conteúdo do arquivo `application.yaml` como abaixo:

```yaml
##
# Server
##
server:
    port: 8080
    servlet:
        context-path: /
    error:
        include-message: always
        include-binding-errors: always
        include-stacktrace: on_param
        include-exception: false

##
# Spring Application
##
spring:
    application:
        name: {{project_artifact_id_formatted}}
    output:
        ansi:
            enabled: ALWAYS
```

- 13.4. E também altere o conteúdo do arquivo `application-test.yaml` como abaixo:

```yaml
##
# Spring Application
##
spring:
    jackson:
        serialization:
            indent_output: true
```

- 13.5. Para ter certeza que o que fizemos não quebrou nada no nosso template, vamos aplicá-lo e ver se a aplicação funciona como esperado. Portanto, dentro do diretório de testes do plugin (`porpcorn-demo-teste`) execute os comandos abaixo:

```sh
# limpa diretório "porpcorn-demo-teste"
rm -rf * .*

# aplica o plugin
stk apply plugin ../popcorn-studio/popcorn-springboot-base-plugin

# valida conteúdo com Maven: compilando código e rodando a bateria de testes
./mvnw clean test
```

14. Com a customização de dependências e configuração da aplicação feitas e funcionando, o próximo passo é adicionar **sample code** de API REST (seguindo nossos padrões de design e arquitetura) para ajudar os desenvolvedores(as) nos primeiros passos. Dessa forma, siga os passos abaixo:

- 14.1. Primeiramente, para facilitar a manipulação e identificação de código de exemplo (*sample code*), vamos criar os seguintes pacotes no classpath da aplicação:

    - Na source folder `src/main`, crie o pacote `samples` dentro do diretório `/java/{{project_base_package_dir}}`;
    - Crie o pacote `produtos` dentro do pacote `samples` da `src/main`;
    - Faça o mesmo na source folder `src/test`;

- 14.2. Em seguida, crie o controller de exemplo `NovoProdutoController.java` dentro do pacote `samples.produtos` localizado na source folder `src/main` com o conteúdo abaixo:

```java
package {{project_base_package}}.samples.produtos;

import jakarta.validation.Valid;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class NovoProdutoController {

    @PostMapping("/api/v1/produtos")
    public ResponseEntity<?> cadastra(@RequestBody @Valid NovoProdutoRequest request) {

        // executa lógica de negócio

        return ResponseEntity
                .status(HttpStatus.CREATED)
                .build();
    }

}
```

- 14.3. Ainda dentro do pacote `samples.produtos`, crie a classe de DTO `NovoProdutoRequest.java` com o seguinte conteúdo:

```java
package {{project_base_package}}.samples.produtos;

import jakarta.validation.constraints.NotBlank;
import jakarta.validation.constraints.NotNull;
import jakarta.validation.constraints.Positive;
import jakarta.validation.constraints.Size;

import java.math.BigDecimal;

public record NovoProdutoRequest(
        @NotBlank @Size(max = 60) String nome,
        @NotBlank @Size(max = 2000) String descricao,
        @NotNull @Positive Double preco
) {
}
```

- 14.4. Em seguida, dentro do pacote `samples.produtos` da source folder `src/test`, crie a classe de testes de integração para nosso controller com o nome `NovoProdutoControllerTest.java` (repare no sufixo `Test`) com o seguinte conteúdo:

```java
package {{project_base_package}}.samples.produtos;

import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.databind.ObjectMapper;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.AutoConfigureMockMvc;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.ActiveProfiles;
import org.springframework.test.web.servlet.MockMvc;

import static org.springframework.http.HttpHeaders.ACCEPT_LANGUAGE;
import static org.springframework.http.MediaType.APPLICATION_JSON;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.post;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;

@SpringBootTest
@ActiveProfiles("test")
@AutoConfigureMockMvc(printOnlyOnFailure = false)
class NovoProdutoControllerTest {

    @Autowired
    protected MockMvc mockMvc;

    @Autowired
    protected ObjectMapper mapper;

    @Test
    @DisplayName("deve cadastrar novo produto")
    void t1() throws Exception {
        // scenario
        NovoProdutoRequest request = new NovoProdutoRequest(
                "iPad Mini",
                "iPad Mini 6a geração",
                3500.99
        );

        // action and validation
        mockMvc.perform(post("/api/v1/produtos")
                        .contentType(APPLICATION_JSON)
                        .content(toJson(request))
                        .header(ACCEPT_LANGUAGE, "en"))
                .andExpect(status().isCreated())
        ;
    }

    @Test
    @DisplayName("não deve cadastrar novo produto quando parametros invalidos")
    void t2() throws Exception {
        // scenario
        NovoProdutoRequest request = new NovoProdutoRequest(
                "",
                "a".repeat(2001),
                -0.01
        );

        // action and validation
        mockMvc.perform(post("/api/v1/produtos")
                        .contentType(APPLICATION_JSON)
                        .content(toJson(request))
                        .header(ACCEPT_LANGUAGE, "en"))
                .andExpect(status().isBadRequest())
        ;
    }

    /**
     * Converts an object payload to {@code String} in JSON format
     */
    public String toJson(Object payload) throws JsonProcessingException {
        return mapper.writeValueAsString(payload);
    }

}
```

- 14.5. Certifique-se que o resultado final do diretório `templates` do nosso plugin deve ser semelhante a este:

```
templates
    ├── HELP.md
    ├── README.md
    ├── mvnw
    ├── mvnw.cmd
    ├── pom.xml
    └── src
        ├── main
        │   ├── java
        │   │   └── {{project_base_package_dir}}
        │   │       ├── samples
        │   │       │   └── produtos
        │   │       │       ├── NovoProdutoController.java
        │   │       │       └── NovoProdutoRequest.java
        │   │       └── {{project_name_capitalized}}Application.java
        │   └── resources
        │       └── application.yaml
        └── test
            ├── java
            │   └── {{project_base_package_dir}}
            │       ├── samples
            │       │   └── produtos
            │       │       └── NovoProdutoControllerTest.java
            │       └── {{project_name_capitalized}}ApplicationTests.java
            └── resources
                └── application-test.yaml
```

- 14.6. Por fim, para ter certeza que tudo está funcionando como esperado, vamos testar nosso plugin. Portanto, dentro do diretório de testes do plugin (`porpcorn-demo-teste`) execute os comandos abaixo:

```sh
# limpa diretório "porpcorn-demo-teste"
rm -rf * .*

# aplica o plugin
stk apply plugin ../popcorn-studio/popcorn-springboot-base-plugin

# valida conteúdo com Maven: compilando código e rodando a bateria de testes
./mvnw clean test
```

15. Agora, vamos adicionar um hook (`hook`) no nosso plugin para garantir que o executável do Maven Wrapper (arquivo `mvnw`) possui permissão de execução logo após a renderização do template. Para isso, siga os passos:

- 15.1. No arquivo `plugin.yaml` adicione o seguinte código:

```yaml
spec:
    # ...
    inputs:
        # ...
    computed-inputs:
        # ...
    ##
    # Hooks
    ##
    hooks:
        - type: run
        trigger: after-render
        commands:
            - chmod +x mvnw mvnw.cmd
```

- 15.2. Valida o plugin:

```sh
stk validate plugin
# Não deve haver errors ou warnings aqui
```

- 15.3. E verifique se o arquivo `mvnw` do Maven Wrapper está sendo gerado com permissões de execução. Para isso, dentro do diretório de testes do plugin (`porpcorn-demo-teste`) execute os comandos abaixo:

```sh
# limpa diretório "porpcorn-demo-teste"
rm -rf * .*

# aplica o plugin
stk apply plugin ../popcorn-studio/popcorn-springboot-base-plugin

# verifica se mvnw é executável
test -x mvnw && echo "It's executable" || echo "It's NOT executable"
```

16. Agora, vamos publicar o plugin no estúdio da nossa conta da StackSpot. Siga os passos:

- 16.1. Acesse o [portal da StackSpot](https://stackspot.com/) e faça o login na sua conta personal (ou outra conta na qual você tenha permissão de criar conteúdo);
- 16.2. **Crie um Studio** com o nome `popcorn-studio`, preencha suas informações básicas e escolha um icone bacana (Google Images ajuda aqui 😉);

- 16.3. Agora, na linha de comando e dentro do diretório do plugin (`popcorn-springboot-base-plugin`), **publique o plugin** no estúdio que criamos no portal da StackSpot:

    ```sh
    stk publish plugin --studio popcorn-studio
    ```

- 16.4. De volta a portal, e dentro do estúdio, **crie uma Stack** chamada de "`Spring Boot REST API Stack`", preencha suas informações básicas e escolha um icone bacana (Google Images ajuda aqui 😉);

- 16.5. Entre na Stack criada e adicione nosso plugin `popcorn-springboot-base-plugin`;

- 16.6. Ainda dentro da Stack, **crie um Starter** com o nome "`rest-api-base`", preencha suas informações básicas e também adicione nosso plugin `popcorn-springboot-base-plugin`"

17. Agora, antes de consumir nossa Stack e Plugins, precisamos configurar uma Workspace. Dessa forma, siga os passos abaixo:

> ⚠️ **Atenção**: Este passo só será possível após configurar o SCM (Source Code Management) na sua conta personal da StackSpot. Se você ainda não fez este setup da conta, por favor siga o [manual na documentação oficial da StackSpot](https://docs.stackspot.com/home/account/guides/scm-integration/) e configure sua conta StackSpot com o **Github**.

- 17.1. **Importe a Action** oficial da StackSpot, `create-repo-github`, para permitir a criação de repositórios durante o fluxo de criação de aplicações pelo portal:

    ```sh
    # clone o repositório de workflows da StackSpot
    git clone https://github.com/stack-spot/stackspot-workflows-action.git

    # entre na action de criação de repositórios do github: create-repo-github
    cd stackspot-workflows-action/stackspot-actions/github/create-repo-github

    # publique a action no nosso estúdio
    stk publish action
    ```

    > 💡 **Dica**: O único diretório que importa do repositório `stackspot-workflows-action` que clonamos é o diretório `<repo>/stackspot-actions/github/create-repo-github`, pois ela é a Action que precisamos de fato publicar no nosso Studio. Portanto, você pode cópia-lo para seu Studio e em seguida versioná-lo no seu Github.

- 17.2. Crie uma nova versão da nossa Stack,  **adicione a Action** `create-repo-github` na Stack e por fim publique-a;

- 17.3. Agora, na área Workspaces, **crie uma nova Workspace** com o nome "`ingressos-cinema`", preencha suas informações básicas e escolha um icone bacana (Google Images ajuda aqui 😉);

- 17.4. Dentro da nossa Workspace, adicione nossa Stack;

- 17.5. Ainda na Workspace, acesse a Stack e **configure o plugin** com inputs padrão e mandatórios que você entender que façam sentido;

- 17.6. Ainda na Stack, **configure a action** `create-repo-github` com o valor padrão `public` como opcional;

- 17.7. Ainda na Stack, **configure o Workflow** *Create-app (Portal)* anexando nossa action `create-repo-github` a evento *Before* do workflow;


18. Por fim, vamos consumir nossa Stack, Starters e Plugins **criando uma aplicação** via portal da StackSpot. Dessa forma, siga os passos abaixo:

- 18.1. Dentro da nossa Workspace, **crie uma nova aplicação** pelo portal, preencha as informações em todas as etapas do fluxo de criação (se desejar, pule a etapa de "*Api Definitions*"); Na última etapa revise os dados e submeta as mudanças.

- 18.2. Acompanhe a pipeline de criação de aplicação na aba *Actions* da sua conta do Github (organização). Quando ela finalizar, acesse o repositório criado na sua organização e aceite o Pull Request (PR);

- 18.3. Faça o clone do repositório criado, e rode o build e bateria de testes via Maven:

    ```sh
    ./mvnw clean test
    ```

- 18.4. Se desejar, importe o projeto na sua IDE favorite e comece o desenvolvimento do seu microsserviço 🥳


## Módulo 3: Habilitando capacidade de monitoramento e health checking

Neste módulo criaremos um novo plugin para habilitar a capacidade de monitoramento e health checking dos nossos microsserviços. Por se tratar do ecossistema Spring Boot, utilizaremos o módulo Spring Boot Actuator.

### Exercícios

1. Primeiramente, dentro do diretório do nosso estúdio `popcorn-studio`, crie um novo plugin com o nome `popcorn-springboot-actuator-plugin` com o comando a abaixo e responda as questões solicitadas pela CLI:

```sh
stk create plugin popcorn-springboot-actuator-plugin
```

2. Ainda dentro do diretório do estúdio, abra-o com seu editor de texto preferido. Se estiver utilizando o **Visual Studio Code (VsCode)**, basta executar o comando abaixo dentro do diretório:

```sh
code .
```

3. Agora vamos fazer as configurações básicas do nosso plugin. Para isso, dentro do diretório `popcorn-springboot-actuator-plugin`, abra o arquivo `plugin.yaml` e edite os atributos `display-name`, `description` e `technologies` como abaixo:

```yaml
metadata:
    display-name: Spring Boot Actuator plugin
    description: Habilita microsserviço a expor endpoint de monitoramento e health checking via modulo Spring Boot Actuator

# ...

spec:
    # ...
    technologies:
        - Api
        - Java
        - Spring Boot
```

4. Aproveitando que estamos no arquivo `plugin.yaml`, vamos adicionar um input para solicitar os endpoints que o usuário gostaria de expor no microsserviço. Dessa forma, siga os passos:

- 4.1. Adicione o `input` para solicitar quais endpoints o usuário deseja que o plugin exponha no microsserviço:

    ```yaml
    inputs:
        - label: Quais endpoints a aplicação deve expor?
        name: actuator_endpoints
        type: multiselect
        required: true
        items:
            - health
            - metrics
            - env
        default: health
        help: Selecione quais endpoints de monitoramento serão expostos pelo Spring Boot Actuator
    ```

- 4.2. Agora, também adicione um `computed-input` para formatar os endpoints por virgula (ele será utilizado nos snippets que criaremos mais a frente):

    ```yaml
    computed-inputs:
        actuator_endpoints_joined: "{{actuator_endpoints | join(',')}}"
    ```

    > ⚠️ **Atenção**: A propriedade `computed-inputs` deve estar na mesma hierarquia da propriedade `inputs`. Ou seja, no mesmo nível de indentação.

5. Agora, vamos criar os snippets de código que serão renderizados e utilizados no microsserviço existente para habilitar a capacidade de monitoramento do Spring Boot Actuator. Para isso, siga os passos abaixo:

- 5.1. Na raiz do plugin, crie o diretório `snippets`. É nele que colocaremos todos os snippets de código do plugin;

- 5.2. Agora, dentro do diretório `snippets`, crie o arquivo de snippet que será responsável por adicionar a dependência do Spring Boot Actuator no projeto. O nome do arquivo deve ser `snippet-pom.xml.jinja` e conterá o seguinte pedaço de código (atenção a indentação e quebras de linhas):

    ```xml
        
            <!-- *************** -->
            <!-- Spring Actuator -->
            <!-- *************** -->
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-actuator</artifactId>
            </dependency>
    ```

- 5.3. Em seguida, ainda no diretório `snippets`, crie outro arquivo de snippet, desta vez para configurar os endpoints de monitoramento que serão expostos pela aplicação. O nome do arquivo deve ser `snippet-application.yaml.jinja` (atenção a indentação e quebras de linhas):

    ```yaml

    ##
    # Spring Actuator
    ##
    management:
        endpoints:
            jmx:
                exposure:
                    include: "*"
            web:
                exposure:
                    include: {{actuator_endpoints_joined}}
        endpoint:
            health:
                show-details: always
                show-components: always
                probes:
                    enabled: true
                    add-additional-paths: true
    ```

    > ⚠️ **Atenção**: Por estarmos modificando conteúdo de formato YAML, como `application.yaml` ou `application-test.yaml`, é muito importante ter uma atenção redobrada com a formatação e indentação do código YAML que será inserido ou alterado pelo plugin, especialmente em snippets de código que serão mergeado em arquivos existentes, grandes e complexos. Qualquer erro de indentação pode gerar erros no startup da aplicação ou, pior, ser ignorada silenciosamente pela aplicação como se nada tivesse acontecido.

- 5.4. O próximo passo é configurar o plugin para utilizar nossos snippets para mergear os arquivos `pom.xml` e `application.yaml` da aplicação. Para isso, adicione os 2 hooks de edição `edit` no arquivo `plugin.yaml`:

    ```yaml
    hooks:
        ##
        # Edit pom.xml
        # (Using hook type=edit)
        ##
        - type: edit
          path: pom.xml
          trigger: after-render    
          changes:
            - search:
              string: "</dependencies>"
              insert-before:
                snippet: snippets/snippet-pom.xml.jinja
              when:
                not-exists-snippet: snippets/snippet-pom.xml.jinja
        ##
        # Edit application.yaml
        # (Using hook type=edit)
        ##
        - type: edit
          path: src/main/resources/application.yaml
          trigger: after-render    
          changes:
            - insert:
              line: -1
              snippet: snippets/snippet-application.yaml.jinja
              when:
                not-exists: "management:"
    ```

    > ⚠️ **Atenção**: A propriedade `hooks` deve estar na mesma hierarquia das propriedades `inputs` e `computed-inputs`. Ou seja, no mesmo nível de indentação.

- 5.5. Agora, vamos resetar o conteúdo do diretório `popcorn-demo-teste`, aplicar o plugin novamente e ver o resultado:

    ```sh
    # reseta modificações do "porpcorn-demo-teste"
    git reset --hard HEAD ; git clean -fd 

    # aplica o plugin
    stk apply plugin ../popcorn-studio/popcorn-springboot-actuator-plugin

    # valida conteúdo com Maven: compilando código e rodando a bateria de testes
    ./mvnw clean test
    ```

    > 💡 **Dica**: Durante a construção e aplicação de plugins em projetos existentes, é comum encontrarmos erros de renderização de templates e snippets, o que muitas vezes pode ser trabalhoso ou impossível de desfazer manualmente. Para evitar essa fadiga, podemos tirar proveito do Git.
    > 
    > Para isso, basta que o projeto existente esteja versionado pelo Git e tenha todo seu conteúdo comitado (ao menos localmente). Em seguida, basta executar o comando de reset do Git abaixo:
    > 
    > ```sh
    > git reset --hard HEAD ; git clean -fd
    > ```
    >
    > Pronto! Agora basta corrigir o plugin e re-aplicá-lo novamente sempre que necessário 🥳

- 5.6. Por não haver testes de integração para a feature de monitoramento, precisamos verificar se os endpoints foram de fatos expostos. Para isso, siga os passos:

    - Levante a aplicação na sua IDE ou via o comando `./mvnw spring-boot:run`;
    - No browser, acesse cada um dos endpoints expostos para verificar o conteúdo retornado pela requisição. Os endpoints são `/actuator/health`, `/actuator/metrics` e `/actuator/env`;

- 5.7. Por fim, **revise e valide** as modificações dos snippets aplicados nos arquivos `pom.xml` e `application.yaml`. Embora o build e bateria de testes passem, é comum encontrar erros de indentação ou formatação após o merge dos snippets. Aqui, você pode utilizar sua IDE ou mesmo o próprio Git com o comando abaixo:

    ```sh
    # dentro do diretório "porpcorn-demo-teste"
    git diff
    ```

6. Agora que tudo está funcionando como esperado. Vamos explorar o uso de 2 hooks especiais desenhados para lidar com edição de XML e YAML e entender suas diferenças com o hook `edit` que utilizamos anteriormente. Portanto, siga os passos abaixo:

- 6.1. Dentro do arquivo `plugin.yaml`, substitua (ou comente) o hook `edit` do arquivo `pom.xml` pelo hook `edit-xml` especificado no código abaixo:

    ```yaml
    hooks:
        ##
        # Edit pom.xml
        # (Using hook type=edit-xml)
        ##
        - type: edit-xml
          trigger: after-render 
          path: pom.xml
          encoding: UTF-8
          changes:
            - xpath: .//dependencies
              append:
                snippet: snippets/snippet-pom.xml.jinja
              when:
                not-exists: "./dependencies/dependency/artifactId[.='spring-boot-starter-actuator']/.."
    ```

- 6.2. Agora, faça o mesmo para o hook `edit` do arquivo `application.yaml`. Substitua ele pelo hook `edit-yaml` como abaixo:

    ```yaml
    hooks:
        ##
        # Edit application.yaml
        # (Using hook type=edit-yaml)
        ##
        - type: edit-yaml
          path: src/main/resources/application.yaml
          trigger: after-render
          indent: 4
          encoding: UTF-8
          changes:
            - yamlpath: "$"
              update:
                snippet: snippets/snippet-application.yaml.jinja
              when:
                not-exists: "$.management.endpoints"
    ```

- 6.3. Agora, vamos resetar o conteúdo do diretório `popcorn-demo-teste`, aplicar o plugin novamente e ver o resultado:

    ```sh
    # reseta modificações do "porpcorn-demo-teste"
    git reset --hard HEAD ; git clean -fd 

    # aplica o plugin
    stk apply plugin ../popcorn-studio/popcorn-springboot-actuator-plugin

    # valida conteúdo com Maven: compilando código e rodando a bateria de testes
    ./mvnw clean test
    ```

- 6.4. E claro, não esqueça de **revisar e validar** as modificações dos snippets aplicados nos arquivos `pom.xml` e `application.yaml`. Aqui, você pode utilizar tanto sua IDE preferida quanto o próprio Git com o comando abaixo:

    ```sh
    # dentro do diretório "porpcorn-demo-teste"
    git diff
    ```

7. Agora, para consumir nosso novo plugin, precisamos publicá-lo no nosso Studio, adiciona-lo na Stack e por fim adiciona-lo na nossa Workspace. Dessa forma, siga os passos abaixo:

- 7.1. Na linha de comando e dentro do diretório do plugin (`popcorn-springboot-actuator-plugin`), publique o plugin no estúdio que criamos no portal da StackSpot:

    ```sh
    stk publish plugin --studio popcorn-studio
    ```

- 7.2. Agora, **crie uma nova versão da Stack existente** e adicione o nosso novo plugin à Stack;

- 7.3. Ainda dentro da Stack, **adicione o plugin ao Starter existente**. Garanta que ele venha **depois** do plugin base;

- 7.4. Salve e publique a nova versão da Stack;

8. Para criar a aplicação com o novo plugin, precisamos antes adicionar a nova versão da Stack a nossa Workspace e configurar cada um dos plugins e actions existentes da Stack. Basta seguir os passos abaixo:

- 8.1. Acesse a nossa Workspace e **adicione a nova versão da Stack**;

- 8.2. Ainda na Workspace, acesse a Stack e **configure o plugin** com inputs padrão e mandatórios que você entender que façam sentido;

- 8.3. Ainda na Stack, **configure a action** `create-repo-github` com o valor padrão `public` como opcional;

- 8.4. Ainda na Stack, **configure o Workflow** *Create-app* (Portal) anexando nossa action `create-repo-github` a evento *Before* do workflow;

9. Por fim, vamos consumir nossa Stack, Starters e Plugins **criando uma aplicação** via portal da StackSpot. Dessa forma, siga os passos abaixo:

- 9.1. Dentro da nossa Workspace, **crie uma nova aplicação** pelo portal, preencha as informações em todas as etapas do fluxo de criação (se desejar, pule a etapa de "*Api Definitions*"); Na última etapa revise os dados e submeta as mudanças.

- 9.2. Acompanhe a pipeline de criação de aplicação na aba *Actions* da sua conta do Github (organização). Quando ela finalizar, acesse o repositório criado na sua organização e aceite o Pull Request (PR);

- 9.3. Faça o clone do repositório criado, e rode o build e bateria de testes via Maven:

    ```sh
    ./mvnw clean test
    ```

- 9.4. Se desejar, importe o projeto na sua IDE favorite e comece o desenvolvimento do seu microsserviço 🥳


## Módulo 4: Habilitando capacidade de persistência em bancos de dados relacionais

Neste módulo criaremos um novo plugin para habilitar a capacidade de persistência em banco de dados relacional dos nossos microsserviços. Por se tratar do ecossistema Spring Boot, utilizaremos o módulo Spring Data JPA com Hibernate.

Também adicionaremos a dependência da biblioteca TestContainers para permitir levantar localmente nosso banco de dados relacional através de containers Docker.

### Exercícios

1. Primeiramente, dentro do diretório do nosso estúdio `popcorn-studio`, crie um novo plugin com o nome `popcorn-springboot-data-jpa-plugin` com o comando a abaixo e responda as questões solicitadas pela CLI:

```sh
stk create plugin popcorn-springboot-data-jpa-plugin
```

2. Ainda dentro do diretório do estúdio, abra-o com seu editor de texto preferido. Se estiver utilizando o **Visual Studio Code (VsCode)**, basta executar o comando abaixo dentro do diretório:

```sh
code .
```

3. Agora vamos fazer as configurações básicas do nosso plugin. Para isso, dentro do diretório `popcorn-springboot-actuator-plugin`, abra o arquivo `plugin.yaml` e edite os atributos `display-name`, `description` e `technologies` como abaixo:

```yaml
metadata:
  display-name: Spring Boot Data JPA plugin
  description: Habilta microsserviço com persistência em bancos de dados relacionais com Spring Data JPA e Hibernate

# ...

spec:
    # ...
    technologies:
        - Api
        - Java
        - Spring Boot
```

4. Ainda no arquivo `plugin.yaml`, vamos adicionar os inputs para perguntar ao usuário qual banco de dados será utilizado e qual pacote base (*base package*) utilizado no projeto. Dessa forma, siga os passos:

- 4.1. Adicione os `input`'s para solicitar o banco de dados e o pacote base do projeto:

    ```yaml
    inputs:
        - label: Escolha um banco de dados relacional (RDBMS)
          name: database_name
          type: text
          items:
            - H2
            - PostgreSQL
          default: H2
          help: Se você não possui infraestrutura pronta, você pode escolher o H2 como banco em memória
        - label: Informe o pacote base do seu projeto
          name: project_base_package
          type: text
          required: true
          default: br.com.zup.popcornstudio.demo
          help: Pacote base raiz (group_id + artifact_id)
    ```

- 4.2. Agora, também adicione a sessão `computed-input` para formatar os inputs entrados pelo usuário (eles serão utilizados nos snippets que criaremos mais a frente):

    ```yaml
    computed-inputs:
        database_name_formatted: "{{database_name | lower}}"
        project_base_package_dir: "{{project_base_package | group_id_folder}}"
    ```

    > ⚠️ **Atenção**: A propriedade `computed-inputs` deve estar na mesma hierarquia da propriedade `inputs`. Ou seja, no mesmo nível de indentação.

- 4.3. Lembre-se de validar seu arquivo `plugin.yaml` com o comando abaixo:

    ```sh
    # dentro do diretorio do plugin
    stk validate plugin
    ```

5. Agora, vamos criar os snippets de código que serão renderizados e utilizados no microsserviço existente para habilitar a capacidade de persistência com Spring Data JPA para os bancos de dados H2 e PostgreSQL. Para isso, siga os passos abaixo:

- 5.1. Na raiz do plugin, crie o diretório `snippets`. É nele que colocaremos todos os snippets de código do plugin;

- 5.2. Agora, dentro do diretório `snippets`, crie o arquivo de snippet que será responsável por adicionar a dependência do Spring Boot Actuator no projeto. O nome do arquivo deve ser `snippet-pom.xml.jinja` e conterá o seguinte pedaço de código (atenção a indentação e quebras de linhas):

    ```xml
        
            <!-- *********************** -->
            <!-- Spring Data JPA with {{database_name}} -->
            <!-- *********************** -->
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-data-jpa</artifactId>
            </dependency>

    {% if database_name_formatted == 'h2' %}
            <dependency>
                <groupId>com.h2database</groupId>
                <artifactId>h2</artifactId>
                <scope>runtime</scope>
            </dependency>
    {% endif %}
    {% if database_name_formatted == 'postgresql' %}
            <dependency>
                <groupId>org.postgresql</groupId>
                <artifactId>postgresql</artifactId>
                <scope>runtime</scope>
            </dependency>
            
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-testcontainers</artifactId>
                <scope>test</scope>
            </dependency>

            <dependency>
                <groupId>org.testcontainers</groupId>
                <artifactId>junit-jupiter</artifactId>
                <scope>test</scope>
            </dependency>

            <dependency>
                <groupId>org.testcontainers</groupId>
                <artifactId>postgresql</artifactId>
                <scope>test</scope>
            </dependency>
    {% endif %}
    ```

    > ⚠️ **Atenção**: Perceba que estamos utilizado estruturas de fluxos do Jinja, como `if-endif` para decidir qual trecho de código será renderizado de acordo com os inputs do usuário.

- 5.3. Em seguida, ainda no diretório `snippets`, crie outro arquivo de snippet, desta vez para configurar as propriedades de acesso ao banco de dados e otimizações de performance do pool de conexões e Hibernate. O nome do arquivo deve ser `snippet-application.yaml.jinja` (atenção a indentação, quebras de linhas e código Jinja):

    ```yaml
        ##
        # DataSource and JPA/Hibernate ({{database_name}})
        ##
    {% if database_name_formatted == 'h2' %}
        datasource:
            driverClassName: org.h2.Driver
            url: jdbc:h2:mem:dev_db
            username: sa
            password: sa
            hikari:
                auto-commit: false
                maximum-pool-size: 20
                connection-timeout: 10000       # 10s
                validation-timeout: 5000        # 5s
                max-lifetime: 1800000           # 30min
                leak-detection-threshold: 60000 # 1min
        h2:
            console: # http://localhost:8080/h2-console/
                enabled: true
    {% endif %}
    {% if database_name_formatted == 'postgresql' %}
        datasource:
            driverClassName: org.postgresql.Driver
            url: jdbc:postgresql://localhost:5432/dev_db
            username: postgres
            password: postgres
            hikari:
                auto-commit: false
                maximum-pool-size: 20
                connection-timeout: 10000       # 10s
                validation-timeout: 5000        # 5s
                max-lifetime: 1800000           # 30min
                leak-detection-threshold: 60000 # 1min
    {% endif %}
        jpa:
            generate-ddl: true
            show-sql: true
            open-in-view: false
            hibernate:
                ddl-auto: update
            properties:
                hibernate:
                    format_sql: true
                    jdbc:
                        time_zone: UTC
                        batch_size: 15
                        order_inserts: true
                        order_updates: true
                        batch_versioned_data: true
                    connection:
                        provider_disables_autocommit: true
                    query:
                        in_clause_parameter_padding: true
                        fail_on_pagination_over_collection_fetch: true
                        plan_cache_max_size: 4096

    ```

    > ⚠️ **Atenção**: Por estarmos modificando conteúdo de formato YAML, como `application.yaml` ou `application-test.yaml`, é muito importante ter uma atenção redobrada com a formatação e indentação do código YAML que será inserido ou alterado pelo plugin, especialmente em snippets de código que serão mergeados em arquivos existentes, grandes e complexos. Qualquer erro de indentação pode gerar erros no startup da aplicação ou, pior, ser ignorada silenciosamente pela aplicação como se nada tivesse acontecido.

- 5.4. Agora faremos o mesmo para o arquivo de configuração de testes do Spring. Crie um novo arquivo de snippet com o nome `snippet-application.yaml.jinja` (atenção a indentação, quebras de linhas e código Jinja):

    ```yaml
        ##
        # DataSource and JPA/Hibernate ({{database_name}})
        ##
    {% if database_name_formatted == 'h2' %}
        datasource:
            url: jdbc:h2:mem:test_db
            username: sa
        h2:
            console:
                enabled: false
    {% endif %}
    {% if database_name_formatted == 'postgresql' %}
        datasource:
            driverClassName: org.testcontainers.jdbc.ContainerDatabaseDriver
            url: jdbc:tc:postgresql:14.5:////test_db
    {% endif %}

    ```

- 5.5. O próximo passo é configurar o plugin para utilizar nossos snippets para mergear os arquivos `pom.xml`,  `application.yaml` e `application-test.yaml` da aplicação. Para isso, adicione os hooks de edição (`edit`) no arquivo `plugin.yaml`:

    ```yaml
    hooks:
        ##
        # Edit pom.xml
        ##
        - type: edit
          path: pom.xml
          trigger: after-render    
          changes:
            - search:
                string: "</dependencies>"
                insert-before:
                    snippet: snippets/snippet-pom.xml.jinja
                when:
                    not-exists-snippet: snippets/snippet-pom.xml.jinja
        ##
        # Edit application.yaml
        ##
        - type: edit
          path: src/main/resources/application.yaml
          trigger: after-render    
          changes:
            - search:
                string: "spring:"
                insert-after:
                    snippet: snippets/snippet-application.yaml.jinja
                when:
                    not-exists: "datasource:"
        ##
        # Edit application-test.yaml
        ##
        - type: edit
          path: src/test/resources/application-test.yaml
          trigger: after-render    
          changes:
            - search:
                string: "spring:"
                insert-after:
                    snippet: snippets/snippet-application-test.yaml.jinja
                when:
                    not-exists: "datasource:"
    ```

    > ⚠️ **Atenção**: A propriedade `hooks` deve estar na mesma hierarquia das propriedades `inputs` e `computed-inputs`. Ou seja, no mesmo nível de indentação.

- 5.6. Lembre-se de validar seu arquivo `plugin.yaml` com o comando abaixo:

    ```sh
    # dentro do diretorio do plugin
    stk validate plugin
    ```

- 5.7. Agora, vamos resetar o conteúdo do diretório `popcorn-demo-teste`, aplicar o plugin novamente e ver o resultado:

    ```sh
    # reseta modificações do "porpcorn-demo-teste"
    git reset --hard HEAD ; git clean -fd 

    # aplica o plugin
    stk apply plugin ../popcorn-studio/popcorn-springboot-data-jpa-plugin

    # valida conteúdo com Maven: compilando código e rodando a bateria de testes
    ./mvnw clean test
    ```

    > 💡 **Dica**: Durante a construção e aplicação de plugins em projetos existentes, é comum encontrarmos erros de renderização de templates e snippets, o que muitas vezes pode ser trabalhoso ou impossível de desfazer manualmente. Para evitar essa fadiga, podemos tirar proveito do Git.
    > 
    > Para isso, basta que o projeto existente esteja versionado pelo Git e tenha todo seu conteúdo comitado (ao menos localmente). Em seguida, basta executar o comando de reset do Git abaixo:
    > 
    > ```sh
    > git reset --hard HEAD ; git clean -fd
    > ```
    >
    > Pronto! Agora basta corrigir o plugin e re-aplicá-lo novamente sempre que necessário 🥳

- 5.8. Por fim, **revise e valide** as modificações dos snippets aplicados nos arquivos `pom.xml`, `application.yaml` e `application-test.yaml`. Embora o build e bateria de testes passem, é comum encontrar erros de indentação ou formatação após o merge dos snippets. Aqui, você pode utilizar sua IDE ou mesmo o próprio Git com o comando abaixo:

    ```sh
    # dentro do diretório "porpcorn-demo-teste"
    git diff
    ```

6. Com a customização de dependências e configuração da aplicação feitas e funcionando, o próximo passo é adicionar **sample code** de acesso ao banco de dados (seguindo nossos padrões de design e arquitetura) para ajudar os desenvolvedores(as) nos primeiros passos. Dessa forma, siga os passos abaixo:

- 6.1. Primeiramente, para facilitar a manipulação e identificação de código de exemplo (*sample code*), vamos criar os seguintes pacotes no classpath da aplicação:

    - Dentro do diretório `snippets`, crie um novo diretório chamado `samples`;
    - Dentro do diretório `samples`, crie a estrutura de diretórios `src/main/java/{{project_base_package_dir}}/samples/books`
    - Crie a mesma estrutura para source folder `src/test/java/{{project_base_package_dir}}/samples/books`;

- 6.2. Em seguida, crie a entidade de exemplo `Book.java` dentro do pacote `samples.books` localizado na source folder `src/main` com o conteúdo abaixo:

    ```java
    package {{project_base_package}}.samples.books;

    import jakarta.persistence.*;
    import jakarta.validation.constraints.NotBlank;
    import jakarta.validation.constraints.Size;
    import org.hibernate.validator.constraints.ISBN;

    import java.util.Objects;

    import static org.hibernate.validator.constraints.ISBN.Type.ISBN_13;

    @Entity
    @Table(
        uniqueConstraints = {
            @UniqueConstraint(name = "uk_isbn", columnNames = "isbn")
        }
    )
    public class Book {

        @Id
        @GeneratedValue
        private Long id;

        @NotBlank
        @ISBN(type = ISBN_13)
        @Column(nullable = false, length = 13)
        private String isbn;

        @NotBlank
        @Size(max = 120)
        @Column(nullable = false, length = 120)
        private String title;

        @NotBlank
        @Size(max = 4000)
        @Column(nullable = false, length = 4000)
        private String description;

        @Version
        private int version;

        /**
         * @deprecated Exclusive use of Hibernate
         */
        @Deprecated
        public Book() {}

        public Book(@NotBlank @ISBN String isbn,
                    @NotBlank @Size(max = 120) String title,
                    @NotBlank @Size(max = 4000) String description) {
            this.isbn = isbn;
            this.title = title;
            this.description = description;
        }

        public Long getId() {
            return id;
        }

        @Override
        public boolean equals(Object o) {
            if (this == o) return true;
            if (o == null || getClass() != o.getClass()) return false;
            Book book = (Book) o;
            return Objects.equals(isbn, book.isbn);
        }

        @Override
        public int hashCode() {
            return Objects.hash(isbn);
        }

        @Override
        public String toString() {
            return "Book{" +
                    "id=" + id +
                    ", isbn='" + isbn + '\'' +
                    ", title='" + title + '\'' +
                    ", description='" + description + '\'' +
                    '}';
        }
    }
    ```

- 6.3. Ainda dentro do pacote `samples.books`, crie a classe de repositório `BookRepository.java` com o seguinte conteúdo:

    ```java
    package {{project_base_package}}.samples.books;

    import org.springframework.data.jpa.repository.JpaRepository;
    import org.springframework.stereotype.Repository;
    import org.springframework.transaction.annotation.Transactional;

    import java.util.Optional;

    /**
     * Since most repository calls are for read operations, it's good practice
     * to define, at class level, that transactions are read-only by default.
     * 
     * Reference: https://vladmihalcea.com/spring-transaction-best-practices/
     */
    @Repository
    @Transactional(readOnly = true)
    public interface BookRepository extends JpaRepository<Book, Long> {

        public Optional<Book> findByIsbn(String isbn);
    }
    ```

- 6.4. Em seguida, dentro do pacote `samples.books` da source folder `src/test`, crie a classe de testes de integração para nosso repositório com o nome `BookRepositoryTest.java` (repare no sufixo `Test`) com o seguinte conteúdo:

    ```java
    package {{project_base_package}}.samples.books;

    import jakarta.validation.ConstraintViolation;
    import jakarta.validation.ConstraintViolationException;
    import org.junit.jupiter.api.BeforeEach;
    import org.junit.jupiter.api.DisplayName;
    import org.junit.jupiter.api.Test;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.boot.test.context.SpringBootTest;
    import org.springframework.dao.DataIntegrityViolationException;
    import org.springframework.test.context.ActiveProfiles;
    import org.springframework.transaction.TransactionSystemException;

    import java.util.Optional;

    import static org.assertj.core.api.Assertions.*;
    import static org.assertj.core.api.InstanceOfAssertFactories.iterable;
    import static org.assertj.core.groups.Tuple.tuple;
    import static org.junit.jupiter.api.Assertions.assertEquals;
    import static org.junit.jupiter.api.Assertions.assertThrows;

    @SpringBootTest
    @ActiveProfiles("test")
    class BookRepositoryTest {

        @Autowired
        private BookRepository repository;

        @BeforeEach
        void setUp() {
            repository.deleteAll();
        }

        @Test
        @DisplayName("should save a book")
        void t1() {
            // scenario
            Book book = new Book("9788550800653", "Domain-Driven Design", "DDD - The blue book");

            // action
            repository.save(book);

            // validation
            assertThat(repository.findAll())
                    .hasSize(1)
                    .usingRecursiveFieldByFieldElementComparator()
                    .containsExactly(book)
                    ;
        }

        @Test
        @DisplayName("should not save a book with invalid parameters")
        void t2() {
            // scenario
            Book book = new Book("97885-invalid", "a".repeat(121), "");

            // action and validation
            assertThatThrownBy(() -> {
                repository.save(book);
            })
                    .isInstanceOf(TransactionSystemException.class)
                    .hasRootCauseInstanceOf(ConstraintViolationException.class)
                    .getRootCause()
                    .extracting("constraintViolations", as(iterable(ConstraintViolation.class)))
                    .extracting(
                            t -> t.getPropertyPath().toString(),
                            ConstraintViolation::getMessage
                    )
                    .containsExactlyInAnyOrder(
                            tuple("isbn", "invalid ISBN"),
                            tuple("title", "size must be between 0 and 120"),
                            tuple("description", "must not be blank")
                    )
            ;
            // Tip: Try always to verify the side effects
            assertEquals(0, repository.count());
        }

        @Test
        @DisplayName("should not save a book when a book with same isbn already exists")
        void t3() {
            // scenario
            String isbn = "9788550800653";
            Book ddd = new Book(isbn, "Domain-Driven Design", "DDD - The blue book");

            // action
            repository.save(ddd);

            // validation
            assertThrows(DataIntegrityViolationException.class, () -> {
                Book cleanCode = new Book(isbn, "Clean Code", "Learn how to write clean code with Uncle Bob");
                repository.save(cleanCode);
            });
            // Tip: Try always to verify the side effects
            assertEquals(1, repository.count());
        }

        @Test
        @DisplayName("should find a book by isbn")
        void t4() {
            // scenario
            String isbn = "9788550800653";
            Book book = new Book(isbn, "Domain-Driven Design", "DDD - The blue book");
            repository.save(book);

            // action
            Optional<Book> optionalBook = repository.findByIsbn(isbn);

            // validation
            assertThat(optionalBook)
                    .isPresent().get()
                    .usingRecursiveComparison()
                    .isEqualTo(book)
                    ;
        }

        @Test
        @DisplayName("should not find a book by isbn")
        void t5() {
            // scenario
            Book book = new Book("9788550800653", "Domain-Driven Design", "DDD - The blue book");
            repository.save(book);

            // action
            String notExistingIsbn = "1234567890123";
            Optional<Book> optionalBook = repository.findByIsbn(notExistingIsbn);

            // validation
            assertThat(optionalBook).isEmpty();
        }

    }
    ```

- 6.5. Certifique-se que o resultado final do diretório `snippets` do nosso plugin deve ser semelhante a este:

    ```
    snippets
        ├── samples
        │   └── src
        │       ├── main
        │       │   └── java
        │       │       └── {{project_base_package_dir}}
        │       │           └── samples
        │       │               └── books
        │       │                   ├── Book.java
        │       │                   └── BookRepository.java
        │       └── test
        │           └── java
        │               └── {{project_base_package_dir}}
        │                   └── samples
        │                       └── books
        │                           └── BookRepositoryTest.java
        ├── snippet-application-test.yaml.jinja
        ├── snippet-application.yaml.jinja
        └── snippet-pom.xml.jinja
    ```

- 6.6. Agora, dentro do arquivo `plugin.yaml`, adicione o hook `render-templates` para renderizarmos nosso diretório de sample codes na aplicação:

    ```yaml
    hooks:
        # ... outros hooks
        
        ##
        # Create sample code
        ##
        - type: render-templates
          trigger: after-render
          path: snippets/samples
    ```
- 6.7. Por fim, para ter certeza que tudo está funcionando como esperado, vamos testar nosso plugin para **os 2 bancos de dados: H2 e PostgreSQL**. Portanto, dentro do diretório de testes do plugin (`porpcorn-demo-teste`) execute os comandos abaixo para cada banco:

    ```sh
    # reseta modificações do "porpcorn-demo-teste"
    git reset --hard HEAD ; git clean -fd

    # aplica o plugin
    stk apply plugin ../popcorn-studio/popcorn-springboot-data-jpa-plugin

    # valida conteúdo com Maven: compilando código e rodando a bateria de testes
    ./mvnw clean test
    ```

7. Agora, vamos adicionar suporte a Docker-Compose no projeto, dessa forma o desenvolvedor(a) poderá construir a aplicação localmente. Para isso, siga os passos abaixo:

- 7.1. Dentro do diretório `snippets`, crie o diretório `docker-postgresql`;

- 7.2. Dentro do diretório `docker-postgresql`, crie o arquivo `docker-compose.yaml` com o seguinte conteúdo:

    ```yaml
    version: '3.8'

    services:
    postgres:
        image: postgres:14.5
        restart: unless-stopped
        ports:
        - "5432:5432"
        environment:
        POSTGRES_DB: ${POSTGRES_USER:-dev_db}
        POSTGRES_USER: ${POSTGRES_USER:-postgres}
        POSTGRES_PASSWORD: ${POSTGRES_PASSWORD:-postgres}
        volumes:
        - postgres-data:/var/lib/postgresql/data/ # persist data even if container shuts down

    volumes:
    postgres-data: # named volumes can be managed easier using docker-compose
        driver: local
    ```

- 7.3. Por fim, no arquivo `plugin.yaml`, adicione um novo hook do tipo `render-templates` para renderizar nosso arquivo `docker-compose.yaml`, mas desta vez condionado a renderizar somente para o banco de dados `PostgreSQL`:

    ```yaml
    hooks:
        # ...outros hooks

        ##
        # Create or edit docker-compose.yaml
        ##
        - type: render-templates
          trigger: after-render
          path: snippets/docker-postgresql
          condition:
            variable: database_name_formatted
            operator: ==
            value: postgresql
    ```
