# Guia de Execução do DistFileSync

## 1. Java Puro (vários terminais)

### Executar um peer
java Application <meu_endereco> "<peers_com_virgula>" "<diretorio>"

### Exemplo concreto
java Application localhost:5001 "localhost:5002,localhost:5003" "C:/Users/User/eclipse-workspace/DistFileSync/tmp/shared1"

Testando eventos

Criar um arquivo:

echo "teste peer1" > teste1.txt


Modificar o arquivo:

echo "modificação" >> teste1.txt


Deletar o arquivo:

rm teste1.txt


Cada ação será detectada pelo DirectoryWatcher e enviada para os outros peers, que replicarão os arquivos.

-- --

## 2. Docker Compose
2.1 Subir todos os peers
docker-compose build
docker-compose up -d

2.2 Ver logs dos peers
docker-compose logs -f

2.3 Parar e remover containers (caso precise reiniciar)
docker-compose down -v

## 3. Acessando um peer específico (Docker)
Entrar no container
docker exec -it peer1 /bin/bash

Dentro do container
cd /tmp/shared1

---
#### Criar um arquivo
echo "teste peer1" > teste1.txt

---

#### Modificar o arquivo
echo "modificação" >> teste1.txt

---

#### Deletar o arquivo
rm teste1.txt


Os eventos criados dentro do container serão monitorados e enviados automaticamente para os outros peers.


---

## 4. Resumo dos Peers e Comandos

| Peer  | Endereço        | Diretório Monitorado | Comando Java Exemplo |
|-------|----------------|--------------------|--------------------|
| peer1 | localhost:5001 | shared1            | java Application localhost:5001 "localhost:5002,localhost:5003" "C:/.../tmp/shared1" |
| peer2 | localhost:5002 | shared2            | java Application localhost:5002 "localhost:5001,localhost:5003" "C:/.../tmp/shared2" |
| peer3 | localhost:5003 | shared3            | java Application localhost:5003 "localhost:5001,localhost:5002" "C:/.../tmp/shared3" |

---

## 5. Observações Finais

- Cada peer precisa ter seu próprio diretório de monitoramento (`shared1`, `shared2`, `shared3`).  
- Sempre que criar, modificar ou deletar arquivos, os eventos serão replicados automaticamente.  
- Para testes rápidos, pode-se criar arquivos diretamente dentro dos diretórios mapeados pelo Docker ou localmente, caso use Java puro.  
- Use `docker-compose logs -f` para monitorar em tempo real os eventos e mensagens de broadcast.
