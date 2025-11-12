# Tutorial: Redirecionamento de Rotas com Nginx + Docker + React

Este guia ensina como configurar o **redirecionamento de rotas** em uma aplicação **React** hospedada em um **container Docker** com **Nginx**.  
Isso é essencial para evitar erros 404 ao recarregar páginas que usam **React Router**.

---

## Repositório do Projeto

O projeto base utilizado neste tutorial está disponível em:

🔗 [https://github.com/rhavymaia/cafeteria-web](https://github.com/rhavymaia/cafeteria-web)

---

## ⚙️ 1. Clonando e Buildando a Aplicação

Clone o repositório e instale as dependências:

```bash
git clone https://github.com/rhavymaia/cafeteria-web.git
cd cafeteria-web
npm install
npm run build
```

## 2. Criando o Container Nginx Manualmente
```bash
docker run -itd \
  -p 80:80 \                             # mapeia a porta do host para o container
  --name web-nginx \                     # nome do container
  -v /home/jefferson/cafeteria-web/dist:/usr/share/nginx/html \   # mapeia a pasta local com o conteúdo buildado
  nginx:1.29.3-alpine
```
## 3. Acessando o Container

```bash 
docker exec -it web-nginx sh
```
Substitua web-nginx pelo nome ou ID do container, se necessário.

## 4. Configurando o Nginx
Acesse o diretório de configuração:
```bash
cd /etc/nginx/conf.d
vi default.conf
```
### Comandos úteis no editor `vi`

Se você não está acostumado com o `vi`, aqui vão os comandos básicos para editar o arquivo dentro do container:

| Comando | Ação |
|----------|------|
| `i` | Entra no **modo de inserção** (permite digitar e editar o texto). |
| `Esc` | Sai do **modo de inserção** e volta ao **modo de comando**. |
| `:w` | **Salva** o arquivo (*write*). |
| `:q` | **Sai** do editor (*quit*). |
| `:wq` | **Salva e sai** do editor ao mesmo tempo. |
| `:q!` | Sai **sem salvar** as alterações. |

Substitua o conteúdo do arquivo por:
```bash
server {
    listen       80;
    listen  [::]:80;
    server_name  localhost;

    location / {
        root   /usr/share/nginx/html;
        index  index.html index.htm;
        try_files $uri $uri/ /index.html;
    }
}
```
O que faz o try_files: Ela garante que qualquer rota (por exemplo, /login, /sobre) redirecione para o index.html, permitindo que o React Router gerencie o roteamento internamente.

## 5. Testando e Reiniciando o Nginx
```bash
 nginx -t # testa o nginx
 nginx -s reload
 ```

## 6. Acessando a Aplicação
👉 [http://localhost](http://localhost)




