# 🎲 True Random Number Generator (n8n Workflow)

Este projeto é um workflow para o n8n que gera números aleatórios a partir de uma entrada do usuário via formulário, utilizando a API do Random.org
 para garantir maior entropia.
 
# 🚀 Como funciona:

1. O usuário acessa o formulário e digita um valor mínimo (Min) e um valor máximo (Max).
2. O workflow valida os valores informados:
-Ambos precisam ser números inteiros.
-O valor mínimo não pode ser maior que o máximo.
3. Caso os valores sejam inválidos, o usuário recebe uma mensagem de erro.
4. Se forem válidos:
-O workflow consulta a API do Random.org para gerar um número inteiro aleatório dentro do intervalo informado.
5. O número sorteado é retornado ao usuário diretamente no formulário de resposta.

# 📂 Estrutura do Workflow
1. On form submission → Gatilho de formulário no n8n.
2. Edit Fields → Extrai e organiza os campos Min e Max.
3. Code in JavaScript → Valida se os números informados são corretos.
4. If → Decide o caminho:
   -✅ Se válido → gera o número com a API.
   -❌ Se inválido → retorna mensagem de erro.
5. Edit Fields2 → Get Integers → Form → Faz a chamada ao Random.org e retorna o número aleatório.
6. Edit Fields1 → Form1 → Exibe mensagem de erro caso a entrada seja inválida.

# 🖼️ Diagrama do Fluxo (Mermaid.js)
flowchart LR
    A[📝 On form submission] --> B[✏️ Edit Fields]
    B --> C[🖥️ Code in JavaScript]
    C --> D{❓ If válido?}
    D -- Sim --> E[✏️ Edit Fields2]
    E --> F[🌐 Get Integers (Random.org API)]
    F --> G[🎲 Form (Número Aleatório)]
    D -- Não --> H[✏️ Edit Fields1]
    H --> I[⚠️ Form1 (Erro)]


# 🛠️ Pré-requisitos
-n8n instalado e rodando (via Docker ou localmente).
-Node.js v22 (LTS) ou superior.
-Conexão com a internet (necessária para acessar o Random.org).
Diferente de outros workflows, este não exige chave de API, pois usa a versão pública do Random.org.

# 🔧 Configuração do Ambiente

Embora o workflow funcione sem nenhuma configuração extra, você pode definir variáveis de ambiente para deixar o uso mais flexível:
# Exemplos de variáveis
MIN_DEFAULT → Valor mínimo padrão (caso o usuário não informe).
MAX_DEFAULT → Valor máximo padrão (caso o usuário não informe).
RANDOM_API_URL → URL base da API (pode ser alterada se quiser usar outro serviço).

Como definir
Linux / macOS
export MIN_DEFAULT=1
export MAX_DEFAULT=100
export RANDOM_API_URL="https://www.random.org/integers/"

Windows (PowerShell)
setx MIN_DEFAULT "1"
setx MAX_DEFAULT "100"
setx RANDOM_API_URL "https://www.random.org/integers/"


No Docker, você pode passar variáveis diretamente ao rodar o container:

docker run -it --rm \
  -p 5678:5678 \
  -v ~/.n8n:/home/node/.n8n \
  -e MIN_DEFAULT=1 \
  -e MAX_DEFAULT=100 \
  -e RANDOM_API_URL="https://www.random.org/integers/" \
  n8nio/n8n


Dentro do n8n, basta usar {{$env.MIN_DEFAULT}} ou {{$env.MAX_DEFAULT}} para acessar as variáveis.

   
# 📥 Importando o Workflow
1. Baixe o arquivo [True Random number Generator (n8n).json](./True%20Random%20number%20Generator%20(n8n).json)
2. No painel do n8n:
   -Vá em Workflows > Import from File.
   -Selecione o arquivo baixado.
3. Ative o workflow.

# 🐳 Rodando o n8n com Docker
Instalação do n8n com Docker

O **n8n** pode ser facilmente instalado e executado usando o Docker.  
Aqui está um guia passo a passo:

1. Instalar o Docker

- Acesse: [https://www.docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop)  
- Baixe e instale o **Docker Desktop** (Windows, macOS ou Linux).  
- Após instalar, abra o Docker Desktop e certifique-se de que ele está rodando (ícone ativo na barra de tarefas).
  
2. Baixar e rodar o n8n com Docker
🔹 Opção A — Usando o Docker Desktop (interface gráfica, sem terminal)

1. Abra o **Docker Desktop**.  
2. Clique em **Images** (menu lateral).  
3. Clique no ícone da **lupa (Search)** no canto superior direito.  
4. Pesquise por **n8nio/n8n** e clique em **Pull** para baixar a imagem.  
5. Após baixar, vá até **Containers**.  
6. Clique em **Run** para criar um container do n8n.  
7. Configure:  
   - Porta: `5678` → `5678`  
   - Volume:  
     - Linux/macOS: `~/.n8n:/home/node/.n8n`  
     - Windows: `%USERPROFILE%\.n8n:/home/node/.n8n`  
8. Clique em **Start**.  

🔹 Opção B  — Usando o Terminal

**Linux / macOS / WSL:**
```bash
docker run -it --rm \
  -p 5678:5678 \
  -v ~/.n8n:/home/node/.n8n \
  n8nio/n8n

Depois, abra no navegador:
👉 http://localhost:5678

Acesse a interface, vá em Workflows > Import from File e carregue o JSON deste repositório.

🔹 Opção C  — Usando o Windows (PowerShell)

docker run -it --rm `
  -p 5678:5678 `
  -v ${HOME}/.n8n:/home/node/.n8n `
  n8nio/n8n

Depois, abra no navegador:
👉 http://localhost:5678

# ✅ Testando o Workflow

1. Acesse o formulário gerado pelo n8n.
2. Informe os valores de Min e Max.
3. Clique em enviar:

Se válido → retorna um número aleatório dentro do intervalo.

Se inválido → exibe mensagem de erro explicando o problema.

#💡 Exemplos de uso
Entrada: Min = 1, Max = 10
Saída possível: 7

Entrada: Min = 50, Max = 60
Saída possível: 53

Entrada inválida: Min = 100, Max = 10
Saída: "O valor mínimo não pode ser maior que o máximo."

📜 Licença

Este projeto está sob a licença MIT.
Sinta-se livre para usar, modificar e compartilhar.

✨ Desenvolvido para estudos e automações com n8n.
