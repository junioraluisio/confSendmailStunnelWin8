 Sendmail e Stunnel :: Enviando e-mail pelo Gmail utilizando a função mail() do PHP no Windows 8 e 10
======================================================================================================

No Windows 8 é necessário a utilização do software Stunnel para fazer o Sendmail funcionar corretamente com o Gmail uma vez que este retirou o suporte para v2 como é utilizado pelo sendmail, acompanhe o passo-a-passo abaixo e acredito que terás sucesso:

Você irá precisar:

    WAMP Servidor;
    Sendmail;
    Stunnel;
    Uma conta de e-mail do Gmail (lógico).

Depois de instalado o WAMP crie dentro do diretório principal do WAMP uma pasta com o nome "sendmail", por exemplo D:\wamp\sendmail, caso tenha instalado o WAMP na unidade "D:".

Coloque os arquivos do sendmail na nova pasta criada, procure pelo arquivo de configuração sendmail.ini e abra-o para editar conforme abaixo.

Altere os seguintes valores:

    smtp_server=localhost
    smtp_port=25
    smtp_ssl=none
    auth_username=seu_email@gmail.com
    auth_password=sua_senha

Salve e feche o arquivo sendmail.ini

Configurado o Sendmail é hora de configurar o php.ini alterando os seguintes valores:

    smtp_port = 465
    sendmail_path = "D:\wamp\sendmail\sendmail.exe -t"
    Lembrando que o caminho "D:\wamp\sendmail\sendmail.exe"
    pode ser diferente dependendo da unidade onde você instalou o WAMP.

Salve e feche o arquivo php.ini e reinicie os serviços do WAMP.

Agora vem a dica para funcionar no Windows 8.

    Baixe o Stunnel (https://www.stunnel.org/downloads.html) e instale no seu computador;
    Na pasta do Stunnel procure pelo arquivo stunnel.conf, deverá estar em
    C:\Arquivos de Programas\stunnel (64bits) ou
    C:\Program Files (x86)\stunnel (32bits);

    DICA: Para editar este arquivo você precisará de permissões especiais,
    portanto será necessário copiá-lo para a Área de Trabalho para só então
    editá-lo e depois de editadosobrescrevê-lo na pasta de origem.

Após copiar o arquivo abra-o e edite as seguintes configurações:

    cert = stunnel.pem
    socket = l:TCP_NODELAY=1
    socket = r:TCP_NODELAY=1
    key = stunnel.pem
    [ssmtp]
    accept  = 465
    connect = 25
    [gmail-smtp]
    client = yes
    accept = 127.0.0.1:25
    connect = smtp.gmail.com:465
    // Para verificar os registros, você pode habilitar a opção de debug
    // que fica no início do arquivo
    debug = 7
    output = stunnel.log

    DICA: Caso habilite a opção de debug será necessário criar o arquivo stunnel.log
    se ele não existir e dar as devidas permissões do seu usuário.

Salve, feche o arquivo stunnel.conf e execute o "Stunnel.exe".

Rode o seu script de envio de e-mail PHP e verifique se deu certo!

Boa sorte!!!

