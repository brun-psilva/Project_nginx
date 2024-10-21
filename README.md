# Verificação de Status do Nginx



Este script em Bash verifica o status do serviço Nginx, armazena logs do status e gera um relatório em HTML que pode ser servido pelo Nginx.

## Sumário
- [Requisitos](#requisitos)
- [Descrição do Script](#descrição-do-script)
- [Variáveis e Funções](#variáveis-e-funções)
- [Como Usar](#como-usar)
- [Automatização com Cron](#automatização-com-cron)
- [Observações](#observações)
- [Possíveis Problemas na Geração do HTML](#possíveis-problemas-na-geração-do-html)

## Requisitos

- Servidor Linux com Bash.
- Serviço Nginx instalado e configurado.
- Permissão de escrita no diretório `/var/www/html`.

## Descrição do Script

O script realiza as seguintes ações:
1. Verifica a data e hora atual e armazena em uma variável.
2. Cria um diretório para armazenar os logs caso não exista.
3. Realiza a verificação do status do Nginx usando `systemctl`.
4. Salva o status do serviço em arquivos de log separados para status ativo e inativo.
5. Gera um arquivo HTML com o resumo do status atual do Nginx e exibe o conteúdo dos logs.
6. Move o arquivo HTML gerado para o diretório do Nginx (`/var/www/html`).

## Variáveis e Funções

### Variáveis

- **`log_dir`**: Diretório onde os logs serão armazenados.
  `/home/bruno/projeto_nginx/logs`
- **`log_ativo`**: Caminho do arquivo que armazena os logs quando o Nginx está ativo.
  `$log_dir/nginx_status_ativo.log`
- **`log_inativo`**: Caminho do arquivo que armazena os logs quando o Nginx está inativo.
  `$log_dir/nginx_status_inativo.log`
- **`html_output`**: Caminho do arquivo HTML que será gerado.
  `$log_dir/status_nginx.html`

### Funções

- **`check_nginx`**: Verifica o status do Nginx e armazena uma mensagem nos logs correspondentes (ativo ou inativo). Exibe a mensagem no console com a data e hora.
  - Entrada: Nenhuma.
  - Saída: Log do status com data e hora no console e no arquivo de log.

- **`generate_html`**: Gera um arquivo HTML com os logs armazenados, que é atualizado a cada 5 minutos (configurado com a tag `<meta>`). Inclui o conteúdo dos logs diretamente no HTML.
  - Entrada: Nenhuma.
  - Saída: Arquivo HTML com os logs e a mensagem de status.

## Como Usar

1. Copie o script em um arquivo com permissão de execução, por exemplo:
   ```bash
   nano verificar_nginx.sh
   chmod +x verificar_nginx.sh
   ```

2. Execute o script:
   ```bash
   ./verificar_nginx.sh
   ```

3. Verifique o relatório gerado em:
   ```
   http://<seu_ip_ou_dominio>/status_nginx.html
   ```

## Automatização com Cron

Para automatizar a verificação do status do Nginx e a geração do HTML, adicione uma tarefa no cron:

1. Edite o crontab:
   ```bash
   crontab -e
   ```

2. Adicione a seguinte linha para executar a cada 5 minutos:
   ```
   */5 * * * * /caminho/para/seu/script/verificar_nginx.sh

## Observações

- O arquivo HTML é sobrescrito a cada execução do script.
- Os arquivos de log são atualizados de forma contínua, armazenando o histórico das verificações.
- Certifique-se de que o usuário que executa o script tenha permissão para criar arquivos no diretório de logs e mover o HTML para `/var/www/html`.

## Possíveis Problemas na Geração do HTML

Possíveis Problemas na Geração do HTML
Caso a geração do arquivo HTML falhe, verifique os seguintes pontos:

# Permissões de Escrita:

Certifique-se de que o usuário que executa o script tenha permissões de escrita no diretório de destino dos logs (/home/user/projeto_nginx/logs) e no diretório onde o HTML é armazenado `(/var/www/html)`.

- Ajuste as permissões com:
```
sudo chmod -R 755 /home/user/projeto_nginx/logs
sudo chmod -R 755 /var/www/html
```
# Existência dos Diretórios:

- Verifique se os diretórios de destino existem. Caso contrário, crie-os com:

```
mkdir -p /home/user/projeto_nginx/logs
mkdir -p /var/www/html
```
## Verificação de Erros no Script:

Execute o script manualmente para identificar erros:
```
./verificar_nginx.sh
```
Isso pode ajudar a identificar problemas na geração do HTML ou ao mover o arquivo.

## Logs Vazios:

Caso os arquivos de log estejam vazios, o HTML gerado pode estar sem conteúdo. Verifique os arquivos `nginx_status_ativo.log` e `nginx_status_inativo.log.`
Problemas de Acesso ao `systemctl`:

- Certifique-se de que o usuário tenha permissão para executar systemctl sem sudo. Configure isso usando:
```
sudo visudo
```
- Adicione a seguinte linha, substituindo pelo seu nome de usuário:
```
User ALL=(ALL) NOPASSWD: /bin/systemctl
```
## Log de Erros:

Para capturar possíveis erros, redirecione a saída de erro do script para um log:
```
./verificar_nginx.sh 2>> /home/user/projeto_nginx/logs/error.log
```
Isso ajudará a analisar as mensagens de erro geradas durante a execução.
