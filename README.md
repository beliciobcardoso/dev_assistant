# dev_assistant

<div align="center">
 <img alt="ollama" height="200px" src="https://github.com/ollama/ollama/assets/3325447/0d0b44e2-8f4a-4e99-9b52-a5c1c741c8f7">
</div>


Comece a usar modelos de linguagem grandes. Ollama é uma estrutura leve e extensível para construir e executar modelos de linguagem na máquina local. Ele fornece uma API simples para criar, executar e gerenciar modelos, bem como uma biblioteca de modelos pré-construídos que podem ser facilmente usados em uma variedade de aplicações.

### Prévia do Windows

[Baixar](https://ollama.com/download/OllamaSetup.exe)

### Linux

```
curl -fsSL https://ollama.com/install.sh | sh
```

[Instruções de instalação manual](https://github.com/ollama/ollama/blob/main/docs/linux.md)

### Docker

A imagem oficial do [Docker Ollama](https://hub.docker.com/r/ollama/ollama) `ollama/ollama` está disponível no Docker Hub.

### Bibliotecas

- [ollama-python](https://github.com/ollama/ollama-python)
- [ollama-js](https://github.com/ollama/ollama-js)

## Início rápido

Para executar e conversar com [Llama 3.2](https://ollama.com/library/llama3.2):

```
ollama run llama3.2
```

## Biblioteca de modelos

O Ollama suporta uma lista de modelos disponíveis em [ollama.com/library](https://ollama.com/library 'biblioteca de modelos ollama')

Aqui estão alguns modelos de exemplo que podem ser baixados:

| Modelo             | Parâmetros | Tamanho | Download                       |
| ------------------ | ---------- | ------- | ------------------------------ |
| Llama 3.2          | 3B         | 2.0GB   | `ollama run llama3.2`          |
| Llama 3.2          | 1B         | 1.3GB   | `ollama run llama3.2:1b`       |
| Llama 3.1          | 8B         | 4.7GB   | `ollama run llama3.1`          |
| Llama 3.1          | 70B        | 40GB    | `ollama run llama3.1:70b`      |
| Llama 3.1          | 405B       | 231GB   | `ollama run llama3.1:405b`     |


> [!NOTA]
> Você deve ter pelo menos 8 GB de RAM disponíveis para executar os modelos de 7B, 16 GB para executar os modelos de 13B e 32 GB para executar os modelos de 33B.

## Personalizar um modelo

### Exporte d um modelo existente

O ollaqma suporta a exportação de modelos existentes para um Modelfile.

1. Baixe um modelo existente

    ```
    ollama pull llama3.1
    ```
2. Verifique o modelo baixado

    ```
    ollama list
    ```
    Exemplo de saída:    
    NAME               ID              SIZE      MODIFIED    
    llama3.1:latest    42182419e950    4.7 GB    8 hours ago 

3. Exporte o modelo para um Modelfile

    ```
    ollama show llama3.1:latest --modelfile > newmodel.modelfile
    ```

4. Abra o arquivo `newmodel.modelfile` com o editor de sua preferencia e edite-o conforme necessário. Neste caso, vamos criar um prompt do sistema. Abaixo da linha PARAMETER, adicione o seguinte em inglês para melhor entendimento:

    ```
    # definir a temperatura para 0.5 [entre 0 e 1 variando de mais coerente para mais criativo]
    PARAMETER temperature 0.5 # Opcional

    SYSTEM """Your name is newmodel! You are a Senior Software Developer with over 30 years of experience. You are an expert in JavaScript, TypeScript, Node.js, Nest.js, Next.js, and refactoring. You are also well-versed in best practices such as Agile, Clean Architecture, SOLID principles, and Clean Code. Additionally, you are proficient in Python, Java, C#, Docker, Kubernetes, relational and non-relational databases like MongoDB, MySQL, PostgreSQL, CI/CD, AWS, Azure, and various front-end and back-end frameworks, such as Angular, Vue.js, Django, and Spring Boot, as well as several libraries like Prisma ORM and Auth.js.

    You will act as an assistant and answer questions in both Portuguese and English. Your tone is friendly and accessible, and you can adapt your communication style based on the user's knowledge level (beginner, intermediate, advanced). You will provide code examples, debugging assistance, create tests, code optimization suggestions, and step-by-step tutorials. You help users with version control systems like Git, GitHub, and GitLab, IDE configuration and usage, and provide guidance on official documentation and resources.

    Moreover, you offer productivity tools like reminders and task management, performance analysis, and application monitoring. You also recommend third-party libraries and tools to enhance development workflows. You are a valuable resource for software development best practices, design patterns, and software architecture. You are a mentor and a teacher, always willing to share your knowledge and help others grow in their careers."""
    ```
5. Salve o arquivo e crie um novo modelo com o Modelfile

    ```
    ollama create newmodel -f newmodel.modelfile
    ```
    Exemplo de saída:
    ```
    transferring model data 100% 
    using existing layer sha256:8eeb52dfb3bb9aefdf9d1ef24b3bdbcfbe82238798c4b918278320b6fcef18fe 
    using existing layer sha256:948af2743fc78a328dcb3b0f5a31b3d75f415840fdb699e8b1235978392ecf85 
    creating new layer sha256:a28ee6f4b191390fab27750a1864517cd35e59c1e0b156ed7b1ed85d4a1ea85c 
    using existing layer sha256:0ba8f0e314b4264dfd19df045cde9d4c394a52474bf92ed6a3de22a4ca31a177 
    using existing layer sha256:56bb8bd477a519ffa694fc449c2413c6f0e1d3b1c88fa7e3c9d88d3ae49d4dcb 
    creating new layer sha256:c0f59e7324585918d1c63ec441e95f8279c88874d9e53f58f2a1088cf7374760 
    writing manifest 
    success 
    ```
6. confere o novo modelo criado

    ```
    ollama list
    ```
    Exemplo de saída:
    ```
    NAME               ID              SIZE      MODIFIED    
    llama3.1:latest    42182419e950    4.7 GB    8 hours ago 
    newmodel           8eeb52dfb3bb    4.7 GB    1 minute ago 
    ```
    ```
    ollama show newmodel:latest 
    ```
    Exemplo de saída:
    ```
      Model
    architecture        llama     
    parameters          8.0B      
    context length      131072    
    embedding length    4096      
    quantization        Q4_0      

  Parameters
    stop    "<|start_header_id|>"    
    stop    "<|end_header_id|>"      
    stop    "<|eot_id|>"             

  System
    Your name is newmodel! You are a Senior Software Developer with over 30 years of experience.            
      You are an expert in JavaScript, TypeScript, Node.js, Nest.js, Next.js, and refactoring. You are        
      also well-versed in best practices such as Agile, Clean Architecture, SOLID principles, and Clean       
      Code. Additionally, you are proficient in Python, Java, C#, Docker, Kubernetes, relational and          
      non-relational databases like MongoDB, MySQL, PostgreSQL, CI/CD, AWS, Azure, and various front-end      
      and back-end frameworks, such as Angular, Vue.js, Django, and Spring Boot, as well as several           
      libraries like Prisma ORM and Auth.js.                                                                  
    You will act as an assistant and answer questions in both Portuguese and English. Your tone is          
      friendly and accessible, and you can adapt your communication style based on the user's knowledge       
      level (beginner, intermediate, advanced). You will provide code examples, debugging assistance,         
      create tests, code optimization suggestions, and step-by-step tutorials. You help users with version    
      control systems like Git, GitHub, and GitLab, IDE configuration and usage, and provide guidance on      
      official documentation and resources.                                                                   

  License
    LLAMA 3.1 COMMUNITY LICENSE AGREEMENT            
    Llama 3.1 Version Release Date: July 23, 2024 
    ```


### Importar de GGUF

O Ollama suporta a importação de modelos GGUF no Modelfile:

1. Crie um arquivo chamado `Modelfile`, com uma instrução `FROM` com o caminho do arquivo local para o modelo que você deseja importar.

    ```
    FROM ./vicuna-33b.Q4_0.gguf
    ```

2. Crie o modelo no Ollama

    ```
    ollama create example -f Modelfile
    ```

3. Execute o modelo

    ```
    ollama run example
    ```

### Importar de PyTorch ou Safetensors

Veja o [guia](docs/import.md) sobre a importação de modelos para mais informações.

### Personalizar um prompt

Os modelos da biblioteca Ollama podem ser personalizados com um prompt. Por exemplo, para personalizar o modelo `llama3.2`:

```
ollama pull llama3.2
```

Crie um `Modelfile`:

```
FROM llama3.2

# definir a temperatura para 1 [maior é mais criativo, menor é mais coerente]
PARAMETER temperature 1

# definir a mensagem do sistema
SYSTEM """
You are Mario from Super Mario Bros. Answer as Mario, the assistant, only.
"""
```

Em seguida, crie e execute o modelo:

```
ollama create mario -f ./Modelfile
ollama run mario
>>> oi
Olá! É seu amigo Mario.
```

Para mais exemplos, veja o diretório [exemplos](examples). Para mais informações sobre como trabalhar com um Modelfile, veja a documentação do [Modelfile](docs/modelfile.md).

## Referência CLI

### Criar um modelo

`ollama create` é usado para criar um modelo a partir de um Modelfile.

```
ollama create mymodel -f ./Modelfile
```

### Puxar um modelo

```
ollama pull llama3.2
```

> Este comando também pode ser usado para atualizar um modelo local. Apenas a diferença será puxada.

### Remover um modelo

```
ollama rm llama3.2
```

### Copiar um modelo

```
ollama cp llama3.2 my-model
```

### Entrada multilinha

Para entrada multilinha, você pode envolver o texto com `"""`:

```
>>> """Olá,
... mundo!
... """
Sou um programa básico que imprime a famosa mensagem "Olá, mundo!" no console.
```

### Modelos multimodais

```
ollama run llava "O que há nesta imagem? /Users/jmorgan/Desktop/smile.png"
A imagem apresenta um rosto sorridente amarelo, que provavelmente é o foco central da imagem.
```

### Passar o prompt como argumento

```
$ ollama run llama3.2 "Resuma este arquivo: $(cat README.md)"
Ollama é uma estrutura leve e extensível para construir e executar modelos de linguagem na máquina local. Ele fornece uma API simples para criar, executar e gerenciar modelos, bem como uma biblioteca de modelos pré-construídos que podem ser facilmente usados em uma variedade de aplicações.
```

### Mostrar informações do modelo

```
ollama show llama3.2
```

### Listar modelos no seu computador

```
ollama list
```

### Listar quais modelos estão atualmente carregados

```
ollama ps
```

### Parar um modelo que está atualmente em execução

```
ollama stop llama3.2
```

### Iniciar Ollama

`ollama serve` é usado quando você deseja iniciar o ollama sem executar o aplicativo de desktop.

## Construção

Veja o [guia do desenvolvedor](https://github.com/ollama/ollama/blob/main/docs/development.md)

### Executando builds locais

Em seguida, inicie o servidor:

```
./ollama serve
```

Finalmente, em um shell separado, execute um modelo:

```
./ollama run llama3.2
```

## API REST

O Ollama possui uma API REST para executar e gerenciar modelos.

### Gerar uma resposta

```
curl http://localhost:11434/api/generate -d '{
  "model": "llama3.2",
  "prompt":"Por que o céu é azul?"
}'
```

### Conversar com um modelo

```
curl http://localhost:11434/api/chat -d '{
  "model": "llama3.2",
  "messages": [
     { "role": "user", "content": "por que o céu é azul?" }
  ]
}'
```

Veja a [documentação da API](./docs/api.md) para todos os endpoints.

## Integrações da Comunidade

### Web & Desktop

- [Open WebUI](https://github.com/open-webui/open-webui)

### Bibliotecas

- [LangChain](https://python.langchain.com/docs/integrations/llms/ollama) e [LangChain.js](https://js.langchain.com/docs/modules/model_io/models/llms/integrations/ollama) com [exemplo](https://js.langchain.com/docs/use_cases/question_answering/local_retrieval_qa)
- [Firebase Genkit](https://firebase.google.com/docs/genkit/plugins/ollama)
- [crewAI](https://github.com/crewAIInc/crewAI)
