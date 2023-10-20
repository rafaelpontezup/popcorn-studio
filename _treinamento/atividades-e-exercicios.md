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

17. Agora, vamos consumir o plugin criando uma aplicação via portal da StackSpot. Dessa forma, siga os passos abaixo:

> ⚠️ **Atenção**: Este passo só será possível após configurar o SCM (Source Code Management) na sua conta personal StackSpot. Se você ainda não fez este setup da conta com o **Github**, por favor siga o [manual na documentação oficial da StackSpot](https://docs.stackspot.com/home/account/guides/scm-integration/scm-github/).

- 17.1. **Importe a Action** oficial da StackSpot para permitir a criação de repositórios na sua conta do Github.








