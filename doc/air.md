### Configurando o Air

##### ⚠️ Requisitos obrigatórios
- Go na versão 1.22 ou superior

##### Passo 1:
- Crie uma pasta para o projeto de exemplo e mude para a pasta:
```
$ mkdir live-reload-go
$ cd live-reload-go
```

##### Passo 2:
- Dentro da pasta recém-criada, inicialize um projeto Go. Normalmente, os projetos Go seguem o formato "github.com/nickname/nome-do-projeto". No entanto, você pode personalizar o caminho do repositório ou colocar outro nome conforme necessário, no meu caso colocarei meu repositório:
```
$ go mod init github.com/felipelaraujo/live-reload-go
```

##### Passo 3:
- Com o go.mod criado, precisamos de uma função main com um servidor HTTP básico dentro. Para isso, crie a pasta cmd/ e o arquivo main.go:
```
$ mkdir cmd
$ touch cmd/main.go
```

- Cole o seguinte código no seu main.go:

```
cmd/main.go

package main

import (
    "log"
    "net/http"
)

func main() {
    // Criando uma instância do servidor
    mux := http.NewServeMux()

    // Configurando rota e função handler
    mux.HandleFunc("GET /", func(w http.ResponseWriter, r *http.Request) {
        w.WriteHeader(http.StatusOK)
        w.Write([]byte("hello, world!"))
    })

    // Iniciando o servidor
    log.Fatal(http.ListenAndServe(":3000", mux))
}
```

#### ☁️ Configurando Air
##### Passo 1:
- Ao realizar a instalação global do Air, você garante a presença constante do comando air em seu sistema, eliminando a necessidade de reinstalação no futuro.
- Instale o Air globalmente com:
```
$ go install github.com/cosmtrek/air@latest
```

- Certifique-se de ter todas as variáveis de ambiente Go configuradas corretamente para que o comando air seja reconhecido pelo seu terminal.

##### Passo 2:
- Crie o arquivo de configuração do Air na raiz do projeto com o nome ```.air.toml``` e cole o seguinte conteúdo:
```
# Raiz do projeto onde os comandos serão executados.
root = "."
# Local onde a pasta temporária receberá os outputs do Air.
tmp_dir = "tmp"

[build]
# Comando shell buildando o executável.
# O primeiro argumento recebe onde será
# o output do executável e o segundo argumento é
# qual arquivo queremos transformar em binário.
cmd = "go build -o ./tmp/main ./cmd/main.go"

# Local onde estará o executável do build (binário).
bin = "./tmp/main"

# Array com nomes de arquivos e/ou diretórios para se ignorar.
# Estou ignorando a pasta /tmp pois ela não faz parte do meu programa.
exclude_dir = ["tmp", "assets"]

# Array com expressões regex para ignorar arquivos com nomes específicos.
exclude_regex = ["_test\\.go"]

# Ignora arquivos que não foram alterados.
exclude_unchanged = true

# Array de extensões de arquivos para incluir no build.
include_ext = ["go", "tpl", "tmpl", "html"]

# Nome do arquivo de log que ficará dentro da pasta tmp.
log = "build-errors.log"

# Encerra o executável antigo caso ocorra algum erro no build.
stop_on_error = false

[color]
# Cores de onde vem os logs no console.
app = ""
build = "yellow"
main = "magenta"
runner = "green"
watcher = "cyan"

[log]
# Mostra o horário do log.
time = true

# Mostra apenas os logs da aplicação e não do Air.
main_only = false

[misc]
# Deleta a pasta tmp no encerramento da aplicação.
clean_on_exit = true

[screen]
# Limpa o console após o rebuild da aplicação.
clear_on_rebuild = false    
Para uma lista completa de configurações, consulte air_example.toml. Certifique-se de ajustar as configurações conforme necessário para o seu projeto.
```

##### 🚀 Executando o projeto
A estrutura final do projeto deve ser semelhante a esta:
```
.
├── cmd/
│ └── main.go
├── .air.toml
└── go.mod
```

- Com tudo configurado, inicie a aplicação usando o comando:
```
$ air
```
