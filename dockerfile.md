# 📦 Dockerfile - Guia Completo de Comandos

Este documento contém os principais comandos utilizados em um Dockerfile, com explicações e exemplos práticos.
***IMPORTANTE***
Se roda o docker build dentro do diretório raiz da aplicação!

---

## 🧱 Estrutura Básica

```dockerfile
FROM ubuntu:22.04
LABEL maintainer="seu-email@exemplo.com"
RUN apt-get update && apt-get install -y curl
WORKDIR /app
COPY . .
CMD ["bash"]
```

---

## 🔑 Comandos do Dockerfile

## 1. FROM

Define a imagem base para a construção da imagem.

```dockerfile
FROM node:18
```

- Deve ser o primeiro comando (exceto `ARG` antes dele)
- Pode usar imagens oficiais ou customizadas

---

## 2. ARG

Define variáveis usadas durante o build.

```dockerfile
ARG VERSION=1.0
```

- Disponível apenas em tempo de build
- Pode ser sobrescrito com `--build-arg`

---

## 3. ENV

Define variáveis de ambiente persistentes no container.

```dockerfile
ENV NODE_ENV=production
```

- Disponível em runtime
- Pode ser usada pela aplicação

---

## 4. RUN

Executa comandos durante o build da imagem. // Antes da imagem estar criada!

```dockerfile
RUN apt-get update && apt-get install -y nginx
```

- Cada `RUN` cria uma nova camada
- Use `&&` para reduzir camadas

---

## 5. COPY

Copia arquivos do host para o container, tem dois argumentos: origem e destino.

```dockerfile
COPY . /app
```

- Simples e previsível
- Não suporta URL

---

## 6. ADD

Semelhante ao COPY, mas com recursos extras.

```dockerfile
ADD arquivo.tar.gz /app
ADD https://exemplo.com/file.txt /app
```

- Descompacta arquivos automaticamente
- Pode baixar arquivos via URL

⚠️ Prefira `COPY` quando possível

---

## 7. WORKDIR

Define o diretório de trabalho dentro do container, todos os comandos a seguir serão executados dentro dele.

```dockerfile
WORKDIR /app
```

- Equivalente ao `cd`
- Cria o diretório automaticamente

---

## 8. CMD

Define o comando padrão ao iniciar o container. // Depois que a imagem já está criada!

```dockerfile
CMD ["node", "app.js"]
```

- Pode ser sobrescrito no `docker run`
- Apenas o último CMD é considerado

---

## 9. ENTRYPOINT

Define o comando principal do container.

```dockerfile
ENTRYPOINT ["python3"]
```

- Não é facilmente sobrescrito
- Ideal para containers com comportamento fixo

---

## 10. EXPOSE

Documenta a porta utilizada pelo container.

```dockerfile
EXPOSE 3000
```

- Não publica a porta automaticamente
- Serve como documentação

---

## 11. VOLUME

Define um ponto de montagem para persistência de dados.

```dockerfile
VOLUME ["/data"]
```

- Dados persistem fora do container
- Muito usado para bancos de dados

---

## 12. USER

Define o usuário que executa o container. // Colocar no começo, ou os comandos anteriores serão rodados como root!

```dockerfile
USER node
```

- Evita rodar como root
- Melhora segurança

---

## 13. ONBUILD

Define instruções executadas quando a imagem for usada como base.

```dockerfile
ONBUILD COPY . /app
```

- Executado em builds derivados
- Uso mais avançado

---

## 14. STOPSIGNAL

Define o sinal enviado para parar o container.

```dockerfile
STOPSIGNAL SIGTERM
```

- Ajuda no shutdown correto

---

## 15. HEALTHCHECK

Verifica a saúde do container.

```dockerfile
HEALTHCHECK CMD curl --fail http://localhost:3000 || exit 1
```

- Pode marcar container como `healthy` ou `unhealthy`
- Muito usado em produção

---

## 16. SHELL

Define o shell padrão para comandos.

```dockerfile
SHELL ["/bin/bash", "-c"]
```

- Útil para customizar execução de scripts

---

## 🧠 CMD vs ENTRYPOINT

### Exemplo combinado:

```dockerfile
ENTRYPOINT ["python3"]
CMD ["app.py"]
```

Execução final:

```bash
python3 app.py
```

### Diferenças:

- CMD → comando padrão (flexível)
- ENTRYPOINT → comando fixo (mais rígido)

---

## 🚀 Exemplo Completo

```dockerfile
FROM node:18

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

EXPOSE 3000

CMD ["npm", "start"]
```

---

## ⚠️ Boas práticas

- Use imagens leves (`alpine`)
- Reduza número de camadas
- Use `.dockerignore`
- Evite rodar como root
- Prefira `COPY` ao invés de `ADD`

---

## 🔥 Multi-stage Build

```dockerfile
FROM node:18 AS builder

WORKDIR /app
COPY . .
RUN npm install && npm run build

FROM nginx:alpine
COPY --from=builder /app/dist /usr/share/nginx/html
```

- Reduz tamanho da imagem final
- Separa build de runtime

---

## 📚 Resumo

| Comando | Função |
|--------|--------|
| FROM | Imagem base |
| ARG | Variável de build |
| ENV | Variável de ambiente |
| RUN | Executa comandos |
| COPY | Copia arquivos |
| ADD | Copia com extras |
| WORKDIR | Diretório |
| CMD | Comando padrão |
| ENTRYPOINT | Comando fixo |
| EXPOSE | Porta |
| VOLUME | Persistência |
| USER | Usuário |
| ONBUILD | Trigger em build |
| STOPSIGNAL | Sinal de parada |
| HEALTHCHECK | Verificação de saúde |
| SHELL | Shell padrão |

---