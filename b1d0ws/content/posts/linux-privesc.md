---
title: "O Básico de Elevação de Privilégios em Linux"
date: 2022-03-10T10:30:50-03:00
---

Olá, pessoal!

No artigo de hoje estarei mostrando a vocês algumas técnicas simples para escalação de privilégio em Linux.

Esse texto aborda algumas táticas extremamente fáceis que vão pelo menos te direcionar em alguns CTFs básicos. Se você já estudou algo sobre, provavelmente já terá conhecimento de tudo que mostrarei aqui, então você pode utilizar isso como uma revisão.

Mas antes de mais nada, o que é elevação de privilégios? Não tem muito segredo, é basicamente o que o nome diz! É você escalar seus privilégios dentro de uma máquina, passando de um usuário comum para um usuário com mais permissões, passando de um usuário comum para root por exemplo.

## **Enumeração do Sistema**

Como um bom pentester, você sabe que tudo começa com uma boa enumeração. Então é sempre recomendado coletar algumas informações do sistema com os comandos abaixo.

*Ver informações do sistema*  
uname -a  
cat /proc/version  
cat /etc/issue  

*Ver informações dos usuários*  
id  
cat /etc/passwd  

*Ver variáveis de ambiente*  
env  

*Procurar por arquivos interessantes como:*  
~/.bash_history  
/home/user/.ssh/id_rsa  
/var/shadow.bak  

## **Ferramentas Automatizadas**

Após a enumeração manual do sistema, é muito recomendado que você utilize pelo menos uma ferramenta automatizada para procurar por possíveis caminhos para a escalação de privilégios. Abaixo eu listo algumas ferramentas que fazem esse processo, sendo o LinPeas a mais utilizada e a que eu mais recomendo também.

* [LinPeas](https://github.com/carlospolop/PEASS-ng/tree/master/linPEAS)

* [LinEnum](https://github.com/rebootuser/LinEnum)

* [LES (Linux Exploit Suggester) 2](https://github.com/jondonas/linux-exploit-suggester-2) 

* [Linux Smart Enumeration](https://github.com/diego-treitos/linux-smart-enumeration)

* [Linux Priv Checker](https://github.com/sleventyeleven/linuxprivchecker)

Ao executar a ferramenta, basta analisar o output dela e procurar por falhas que permitirão a escalação de privilégios. É claro que se você ainda não viu as vulnerabilidades abaixo provavelmente não saberá como explorar os pontos indicados por essas ferramentas.

## **Kernel**

Existem diversas versões de Kernel e algumas possuem vulnerabilidades que permitem a escalação de privilégio. Se você realizou a enumeração do sistema, já deve ter identificado qual a versão do Kernel utilizado.

Com essa informação, você pode procurar como explorar a versão encontrada. Um dos ataques mais conhecidos é o Dirty Cow que afeta Kernels 2.x até 4.x, antes de 4.8.3. Se foi identificado uma dessas versões, vale a tentativa de exploração.

A ferramenta Linux Exploit Suggester analisa isso de forma automatizada, como indicado na figura abaixo. Com o output, podemos percebemos que o sistema está vulnerável ao Dirty Cow.

![LES-2](/linux-privesc/kernel-exploits.png)

Acessamos o site https://dirtycow.ninja/ para pegar o exploit e o utilizamos.

![dirty-cow](/linux-privesc/dirty-cow.png)

## **Permissões Sudo**

No Linux, existe um arquivo chamado /etc/sudoers que basicamente pode atribuir aos usuários permissões para executar comandos como root.

Observe na imagem abaixo, a primeira linha não comentada indica que o usuário root pode executar todos os comandos como sudo, sendo esse um comportamento padrão. Na segunda linha, está sendo indicado que o usuário kali pode executar o comando find como root, sem a necessidade de senha (NOPASSWD).

Para entender mais sobre a estrutura desse arquivo eu recomendo [esse artigo](https://medium.com/analytics-vidhya/how-to-add-users-to-sudoers-file-in-ubuntu-e89c24b7369d).

![/etc/sudoers](/linux-privesc/etc-sudoers.png)

A questão é: como ver quais comandos podemos executar como root se não temos acesso ao arquivo /etc/sudoers?

O comando *sudo -l* indica isso pra você como mostrado abaixo.

Assim que identificado o comando que você pode executar como root, para saber se existe alguma forma de utilizá-lo para elevação de privilégio, você pode olhar o site [GTFOBins](https://gtfobins.github.io/) e conferir se existe algum comando listado em Sudo para se tornar root.

![sudo-l](/linux-privesc/find-root.png)

![gtfo-find](/linux-privesc/gtfobins-find.png)

É importante frisar que cada comando tem um jeito para escalar privilégio e obviamente nem todos eles possuem essa capacidade. Um exemplo diferente seria ter permissão para executar o cat. Com esse comando, você pode ler qualquer arquivo do sistema, então poderia procurar por flags como /root/root.txt ou analisar o /etc/shadow em busca de hashes.

## **SUID**

Um pouco parecido com a exploração de Permissões Sudo, temos a exploração de SUID. Arquivos SUID tem uma permissão especial que determina que o arquivo/programa vai ser executado com a permissão do dono (owner).

Sendo assim, se algum comando que possui a habilidade de escalar privilégio ter o SUID vinculado ao usuário root, o sistema está vulnerável à escalação de privilégio.
Para procurar arquivos com SUID, você pode usar o comando:  
*find / -type f -perm -04000 -ls 2>/dev/null*

No começo pode ser confuso e difícil analisar qual comando pode ser utilizado, mas com a prática você terá facilidade nesse quesito.

No resultado abaixo, percebemos que */usr/bin/bash* tem o SUID para o usuário root. Agora podemos procurar esse comando no gtfobins e utilizar as sintaxes que aparecem em SUID.

![suids](/linux-privesc/suid.png)

![gtfo-bash](/linux-privesc/gtfobins-bash.png)

Para saber mais sobre SUID e outras permissões especiais, acesse [este link](https://www.redhat.com/sysadmin/suid-sgid-sticky-bit).

## **Cron Jobs**

Cron Jobs são utilizados no Linux para executar scripts em um intervalo regular de tempo e são muito úteis para programar tarefas automatizadas. Entretanto, isso por ser um fator para escalação de privilégio.

Você pode checar os cron jobs da maquina no arquivo */etc/crontab*. Na imagem abaixo percebemos que existe uma tarefa que executa o arquivo /home/kali/backup.sh a cada minuto. O intervalo de tempo é indicado pelo trecho * * * * *.

Existe uma estrutura por trás dessa sintaxe que você pode entender melhor através [desse link](https://ostechnix.com/a-beginners-guide-to-cron-jobs/).

![crontabs](/linux-privesc/crontab.png)

Analisando o arquivo, percebemos que ele faz um backup da pasta notes. A forma para explorar essa falha é editar o arquivo backup.sh com algum comando que nos dê o acesso ao root.

Nesse caso, podemos inserir o comando abaixo que abre uma conexão em nossa máquina atacante na porta 3333. Como o script será executado como root, nossa conexão também será realizada com esse usuário.

![backup.sh](/linux-privesc/backup.sh.png)

Agora basta salvar o arquivo e esperar a tarefa ser executada (a cada minuto) ouvindo a porta que você indicou no comando.

![listener](/linux-privesc/crontab-root.png)

## **PwnKit**

PwnKit é uma vulnerabilidade recém encontrada que afeta a maioria das distribuições Linux que utilizam de um serviço chamado PolKit. Ela foi registrada como a [CVE-2021-4034](https://nvd.nist.gov/vuln/detail/CVE-2021-4034) e permite o escalonamento de privilégio.

A maioria das máquinas em que eu tentei utilizar essa falha era vulnerável, pois somente as versões mais atuais das distribuições corrigem esse bug. Basta rodar alguma ferramenta automatizada das que indiquei que o output indicará se o sistema é vulnerável.

Se você pegar um CTF mais antigo poderá usar essa tática para escalar privilégio, mas provavelmente essa não foi uma falha inserida propositalmente, então não deve ser esse o caminho a ser tomado para conseguir o root da maneira proposta pelo desafio.

A exploração é extremamente simples e pode ser encontrada [aqui](https://github.com/arthepsy/CVE-2021-4034).

Abaixo você pode ver a exploração sendo realizada em uma [máquina do tryhackme](https://tryhackme.com/room/pwnkit) feita para aprender a explorar essa vulnerabilidade.

![pwnkit](/linux-privesc/pwnkit-thm.png)

Se você quiser saber mais sobre, acesse [esse link](https://blog.qualys.com/vulnerabilities-threat-research/2022/01/25/pwnkit-local-privilege-escalation-vulnerability-discovered-in-polkits-pkexec-cve-2021-4034).

## **Checklist**

* Descobrir a versão do kernel e se existe alguma exploração como Dirtyc0w;
* Procurar por credenciais, contas de usuários e diretórios com permissões más configuradas;
* Ver quais comandos você pode utilizar como root;
* Procurar por serviços ou aplicações que podem ser exploradas como SUID e PwnKit;
* Procurar por scripts automatizados como tarefas de backup no crontab.

## **Conclusão**

Nesse artigo foram passadas algumas técnicas básicas para escalação de privilégios em Linux. Apesar de simples, elas são muito utilizadas em CTFs e também aparecem em casos reais de exploração.

Existem diversas outras maneiras de explorar um sistema como uso de Capabilities, exploração de PATH, NFS, LXD, entre outras, mas que não cabem nesse guia básico.

Espero que você possa usufruir de algo que tenha aprendido aqui.

Até mais ver =).

