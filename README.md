# Simulando-um-Ataque-de-Brute-Force

    Este projeto e a documentação de um ataque simulado em uma rede interna com o intuito de reforçar politicas de segurança como senhas complexas, configuração de firewalls e monitoramento de logs.

    As ferramentas utilizadas foram os sistemas operacionais `kali linux` e o `metasploitable 2` que é um sistema operacional linux propositalmente vulneravel para testes de penetração. As ferramentas de enumeração e brute force utilizadas foram respectivamente o `nmap` e o `medusa`.

## Brute force em protocolo FTP

    Neste exemplo iremos criar dois arquivos txt um contendo o uma lista de possiveis usuarios e um com possiveis senhas, estas listas são obtidas atraves de levantamento de hipoteses estatisticas ou vazamentos previos, iremos criar arquivos txt com poucos itens apenas para demonstração mas normalmente listas de de senhas e usuarios possuem milhares ou milões de itens. Em seguida utilizaremos o medusa com estes arqivos de texto para realizar um ataque de força bruta e descobrir o login através do protocolo FTP.

### Criando listas

    Para criar a lista de usuarios utilizaremos no terminal do nosso Kali linux utilizamos o comando:
    `echo -e "user\nmsfadmin\nadmin\nroot" > pass.txt`
    O que são os parametros do codigo acima?
    
    `echo -e` echo é representa a saida padrao do terminal geralmente utilizado  para exibir mensagens na tela, neste caso esta sendo utilizado com a flag `e` para interpretar qubras de linha `\n`
    `parametros entre aspas "admin\.."` texto para ser encaminhado para a saida padrao do terminal, lembre se o `\n` sera intepretado como quebra de linha.
    `|` tambem conhecido como pipe serve para passarmos a saida do ultimo comando como parametro para comando seguinte
    `>` salva a saida anterior no arquivo txt `users.txt`

    acabamos de criar um arquivo de texto chamado `users.txt` como seguinte conteudo:
    ```
    user
    msfadmin
    admin
    root
    ```
    Seguindo a mesma logica, iremos criar um arquivo pass.txt para criar uma lista de senhas utilizando o seguinte codigo:
    `echo -e "123456\npassword\nquerty\nmsfadmin" > pass.txt`

### Utilizando o medusa para descobrir o login

    No terminal do Kali iremos rodar o `medusa` com o seguinte comando:
    `medusa -h 192.168.123.456 -U users.txt -P pass.txt -M ftp -t 6`
    `medusa` inicia o aplicativo medusa
    `-h` indica o alvo, neste caso o IP ficticio (192.168.123.456)
    `-U` parametro para indicar o arquvivo contendo a lista de usuarios
    `-P` parametro para indicar o arquvivo contendo a lista de senhas
    `-M` Modo, indica o modo utilizado, neste caso o ftp
    `-t` indica o numero de threads que sera utilizado para o teste de força bruta

    Ao terminarmos de rodar o comando obtivemos o seguinte resultado:
    (IMAGEM AQUI FUTURAMENTE)
    podemos ver que temos um login com sucesso e a combinação utilizada foi user:msfadmin senha:msfadmin, agora que sabemos a senha podemos testar conexao utilizando:
    `ftp 192.168.123.456`
    `ftp` irá indicar a conexao que queremos realizar 
    ele ira pedir senha e login, entao vamos inserir a que conseguimos utilizando o medusa, user:msfadmin senha:msfadmin. Sucesso, estamos logados!!!

## Brute force em formulario web

## Password spraying em SMB e enumeração de usuarios