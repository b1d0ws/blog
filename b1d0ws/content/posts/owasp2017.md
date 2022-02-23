---
title: "Um resumo sobre a OWASP TOP 10 [2017]"
date: 2021-06-24T15:20:50-03:00
---

## O que é?

OWASP é um projeto aberto que disponibiliza artigos e documentos, gratuitamente feitos pela comunidade, que foca em segurança da informação, área da TI que vem se expandindo cada vez mais.

OWASP top 10 é uma lista das vulnerabilidades mais perigosas encontradas em aplicações web, sendo a última lista elaborada em 2017. Também existe as versões pra mobile e API que são menos conhecidas.

Todo profissional de segurança ou desenvolvedor deveria saber pelo menos o básico sobre essas vulnerabilidades. Como esse artigo abrange a lista inteira, não vou entrar em muitos detalhes em cada tópico, mas você pode facilmente encontrar materiais sobre eles por aí, inclusive no próprio site da OWASP.

OWASP → https://owasp.org/
OWASP top 10 → https://owasp.org/www-project-top-ten/
     
## 1. Injection

Falhas de Injeção como injeções SQL, NoSQL, OS e LDAP, ocorrem quando dados não confiáveis são enviados para um interpretador como parte de um comando ou query.

O dado enviado pelo atacante pode enganar o interpretador para executar comandos não intencionais ou acessar dados sem a autorização apropriada.

### Exemplos

SQL Injection — Quando conseguimos injetar um trecho de código por um input que afetará o banco de dados.

**Uma situação simples para explicar essa técnica é usar o trecho:**  
password' OR 1=1  
Como resultado, o banco rodará a seguinte query:  
SELECT id FROM users WHERE username='username' AND password='password' OR 1=1'

### Impactos

Injeções podem resultar em perda e corrupção de dados, negação de acesso, acessos não autorizados e até mesmo perda total do sistema. Tudo depende do qual é a aplicação e seus dados.

### Prevenção

Usar uma lista de controle que retorna se o caractere é seguro ou não. Se não for seguro, o servidor retornará como erro.

Stripping Input — Remover os caracteres perigosos antes de processar o input inserido.

### Referências
https://owasp.org/www-project-top-ten/2017/A1_2017-Injection https://www.acunetix.com/websitesecurity/sql-injection/

## 2. Broken Authentication

Funções de aplicação relacionadas à autenticação e gerenciamento de sessões são frequentemente implementadas incorretamente, permitindo atacantes a comprometer senhas, chaves ou tokens de sessão, ou explorar outras falhas de implementação para assumir a identidade de outros usuários.

### Exemplos

Ataques de Força Bruta — Se o site usa usuário e senha, a chance de brute force é existente.

Credenciais Fracas — Aplicações web devem colocar políticas de senhas fortes para evitar ataques de força bruta ou até mesmo adivinhações de senha.

Sessões de Cookies Fracas — Se o site usa cookies previsíveis e não controlados, o atacante pode manipular seus cookies para conseguir acessos indevidos.

Timeout de sessões não configuradas também não são ideais, pois alguém mal intencionado por vir a usar o usuário de outra pessoa e ter acesso ao login dela por estar salvo no navegador.

### Impactos

Atacantes podem ganhar acesso a algumas contas de usuário ou apenas uma conta de administrador que pode comprometer todo o sistema. Dependendo da aplicação do sistema, isso pode permitir lavagem de dinheiro, fraude social, roubo de identidade e divulgação de informação sensíveis.

### Prevenção
Para evitar força bruta, coloque limite de tentativas para login e determine uma política forte de senha.

Para evitar adivinhações, requisite senhas fortes na criação do usuário. Outra maneira fortíssima de evitar sessões indevidas é colocar autenticação em dois fatores.

Não mantenha sessões ativas por muito tempo.

### Referências
https://owasp.org/www-project-top-ten/2017/A2_2017-Broken_Authentication

## 3. Sensitive Data Exposure

Muitas aplicações web e APIs não protegem dados sensíveis, como finanças, saúde e PII (Informação pessoalmente identificável), de maneira apropriada.

Dados confidenciais sem proteção extra, como criptografia, podem ser comprometidos, e requerem precauções especiais quando trocados com o navegador.

### Exemplos

A falha mais comum desse tipo é a não encriptação de dados sensíveis. Uma aplicação criptografa números de cartão de crédito para armazená-los em um banco de dados. Contudo, esse dado é descriptografado quando retornado, permitindo um SQL Injection para retornar os números de cartão em texto puro.

Um atacante conseguiu acesso à base de dados com as senhas. Para aplicar os hashes, não foi usado Salt/Pepper e isso é um problema, pois agora ele pode comparar as senhas hasheadas com uma tabela rainbow para descobrir quais são elas.

Um site não usa TLS (Transport Layer Security) para todas as páginas ou usa criptografias fracas. Um atacante pode monitorar o tráfego de rede, transformar conexões HTTPS em HTTP, interceptar as requisições e roubar o cookie de sessão do usuário. Fazendo isso, ele terá acesso ao perfil do mesmo.

### Impactos

Nos últimos anos, esse vem sendo o ataque de impacto mais comum. Atacantes podem roubar ou modificar tais dados para conduzir fraudes de cartão de crédito, roubo de identidade, entre outros crimes.

### Prevenção

Classificar os dados processados e tratá-los com a segurança necessária.

Não armazenar dados sensíveis sem necessidade.

Verificar se não existem dados transmitidos em texto puro e algoritmos de criptografias fracos/antigos.

Armazenar senhas usando hashs adaptadas com uso de salt e funções como Argon2 e PBKDF2.

### Referências
https://owasp.org/www-project-top-ten/2017/A3_2017-Sensitive_Data_Exposure

## 4. XML External Entities (XXE)

Processadores XML antigos ou mal configurados validam referências de entidades externas dentro de documentos XML. Entidades externas podem ser usadas para incluir conteúdos maliciosos, assim explorando vulnerabilidades de código, dependências e integrações.

XML é uma linguagem de marcação legível tanto para humanos quanto para máquinas e serve para armazenar e transportar dados.

### Exemplos

Abaixo segue um exemplo de XML que pode extrair dados do arquivo /etc/passwd:
<?xml version="1.0" encoding="ISO-8859-1"?>
<!DOCTYPE foo [
<!ELEMENT foo ANY >
<!ENTITY xxe SYSTEM "file:///etc/passwd" >]>
<foo>&xxe;</foo>

### Impactos

Essa vulnerabilidade pode ser usada para leitura e compartilhamentos de arquivos internos, escaneamento de portas, execução remota de códigos, negação de serviço e diversos outros ataques.

### Prevenção

Sempre que possível use formatos de dados menos complexos como JSON e evite serialização de dados sensíveis.

Serialização é o processo de converter objetos em formatos mais simples e compatível para transmiti-los entre sistemas de maneira mais prática.

Implemente validações ou filtros de input no servidor (whitelisting).
Desabilite entidades externas em todos os parsers XML da aplicação.

### Referências

https://owasp.org/www-project-top-ten/2017/A4_2017-XML_External_Entities_(XXE)

## 5. Broken Access Control

Restrições sobre o que o usuário tem acesso para fazer são frequentemente não apropriadas. Atacantes podem explorar essas falhas para acessar funcionalidades ou dados não autorizados, como acessar contas de outros usuários, ver dados sensíveis, modificar dados alheios e direitos de acesso, etc.

### Exemplos

Usuários não autorizados acessam páginas que deveriam ser restritas como /admin.

IDOR, Insecure Direct Object Reference — Ato de explorar um configuração mal feita de referência de objeto.
Ex: https://example.com/bank?account_number=1234. Se eu trocar o account_number eu acessarei a conta de outro usuário.

### Impactos

O atacante pode ter acessos a conta de outro usuário ou até mesmo de administrador e a partir disso fazer o que bem entender com os privilégios alcançados, principalmente atualizando e deletando registros.

### Prevenção

Tokens JWT devem ser invalidados no servidor após o logout.

Implementar mecanismos de controle de acesso e reusá-lo em toda aplicação, incluindo minimizar o uso de CORS.

Ter logs de falha de acessos e alertar os administradores quando apropriado (falhas repetidas).

### Referências

https://owasp.org/www-project-top-ten/2017/A5_2017-Broken_Access_Control

## 6. Security Misconfiguration

Configurações incorretas são as vulnerabilidades mais comum entre as citadas. Isso acontece por uma variedade de configurações padrões inseguras, armazenamentos em nuvem abertos, cabeçalhos HTTP incorretos e mensagens de erro contendo informações sensíveis.

Não só todos os sistemas operacionais, frameworks, bibliotecas e aplicações devem ser seguramente configuradas, mas também corrigidas e atualizadas em tempo hábil.

### Exemplos

A aplicação do servidor vem com outras aplicações de amostra que não foram removidos do servidor de produção. Essas amostras têm vulnerabilidades conhecidas, oferecendo um potencial de comprometimento do servidor.

Senhas padrão mantidas em aplicações que estão disponível a todos.

### Impactos

Essas vulnerabilidades frequentemente dão aos atacantes acessos não autorizados a dados ou funcionalidades do sistema. Ocasionalmente, elas podem resultar no comprometimento completo da aplicação.

### Prevenção

Não use sistemas com credenciais padrão, especialmente para contas de admin.

Um sistema de backup deve existir para rapidamente subir um novo ambiente para substituir o que está atualmente em produção caso algo aconteça.

A plataforma de aplicação deve ter o mínimo necessário, sem recursos e componentes desnecessários.

Deve existir a prática de revisar e atualizar as configurações de todas as medidas de segurança.

Os softwares e processos usados devem ser instalados de maneira compatível com a aplicação.

### Referências

https://owasp.org/www-project-top-ten/2017/A6_2017-Security_Misconfiguration

## 7. Cross-Site Scripting (XSS)

Falhas de XSS ocorrem quando uma aplicação inclui dados não confiáveis em uma nova página web sem a validação apropriada. XSS permite atacantes executarem scripts no navegador da vítima que podem roubar sessões de usuário, desfigurar sites ou redirecionar outros clientes para sites maliciosos.

### Exemplos

A forma mais clássica de se testar um XSS é colocando em algum formulário o código abaixo.
<script>alert("XSS")</script>
Se um alerta for exibido na tela, a aplicação está vulnerável a XSS. Se o alerta não for exibido, não podemos confirmar que não existem outras brechas para XSS no site.

### Impactos

XSS refletidos e DOM XSS tem impactos moderados. XSS armazenados são severos com execução remota de códigos no navegador da vítima, como roubo de credenciais e sessões ou entregando um malware para ela.

### Prevenção

A melhor prevenção para XSS é validar todo tipo de entrada e tratá-las corretamente limitando o tamanho do campo a ser inserido, definindo o conjunto de caracteres aceitos ou estabelecendo uma expressão regular para os dados.

Existem frameworks que já “escapam” os códigos inseridos através de XSS.

### Referências

https://owasp.org/www-project-top-ten/2017/A7_2017-Cross-Site_Scripting_(XSS)

## 8. Insecure Deserialization

Serialização é o processo de converter objetos em formatos mais simples e compatível para transmiti-los entre sistemas de maneira mais prática.

Deserialização é o processo inverso e atacantes podem usar de processos internos se essa for mal configurada.

### Exemplos

Um site PHP usa deserialização de objeto para salvar um super cookie, contento informações do usuário como ID, função, senha hashada e estado:  
a:4:{i:0;i:132;i:1;s:7:"Mallory";i:2;s:4:"user"; i:3;s:32:"b6a8b3bea87fe0e05022f8f3c88bc960";}

Um atacante pode modificar o objeto serializado para conseguir permissões de admin:  
a:4:{i:0;i:1;i:1;s:5:"Alice";i:2;s:5:"admin"; i:3;s:32:"b6a8b3bea87fe0e05022f8f3c88bc960";}

### Impactos

Deserialização insegura frequentemente leva em execução remota de código. Mesmo se a deserialização não resulte nisso, ela pode ser usada para executar ataques, incluindo ataques de injeção e elevação de privilégio.

### Prevenção

A maioria das linguagens de programação contêm bibliotecas que provêm deserializações seguras. Se a linguagem não conter uma, você terá que checar os inputs do usuário antes de qualquer outro processo.

### Referências

https://owasp.org/www-project-top-ten/2017/A8_2017-Insecure_Deserialization

## 9. Using Components with Known Vulnerabilities

Componentes como bibliotecas, frameworks, e outros módulos de software, rodam com os mesmos privilégios que a aplicação. Se um componente vulnerável é explorado, um ataque desses pode facilitar sérias perdas de dados ou derrubamentos de servidor.

### Exemplos

Os exemplos nesse caso são vulnerabilidades já descobertas. Um caso é a execução remota de códigos do Struts 2. Você pode encontrá-la nesse link.

### Impactos

Aplicações e APIs usando componentes com vulnerabilidades conhecidas podem enfraquecer as defesas do aplicativo e permitir vários outros ataques. Dependendo da falha encontrada o impacto pode ser desastroso.

### Prevenção

Remover componentes, arquivos, dependências e recursos não utilizados.

Apenas obter componentes de sites oficiais e links seguros.

Monitorar se bibliotecas e recursos têm versões mais atualizadas de patches de segurança.

### Referências

https://owasp.org/www-project-top-ten/2017/A9_2017-Using_Components_with_Known_Vulnerabilities

## 10. Insufficient Logging & Monitoring

Log e monitoramento insuficiente, junto com integrações ineficazes com resposta a incidentes, permite que o atacante mantenha seus ataques com mais persistência, mova-se para mais sistemas e altere ou destrua dados.

Estudos dizem que o tempo para detectar uma violação é de mais de 200 dias, normalmente detectado por partes externas em vez de processos internos de monitoramento.

### Exemplos

Um atacante usa scans para usuários com senhas comuns, assim ele pode conseguir acesso a vários perfis usando essas senhas. Para todos os outros usuários, esse scan deixa apenas um login incorreto registrado. Depois de alguns dias, isso pode ser repetido com uso de outras senhas.

### Impactos

A maioria dos ataques sucedidos vêm de investigações de vulnerabilidades. Permitir que essas investigações continuem aumentam a chance de você ser atacado quase em 100%.

Em 2016, identificar tentativas de invasão demoravam em média 191 dias, tempos suficiente para o dano ser causado.

### Prevenção

Garantir que os logs são gerados em um formato facilmente compreensível por um gerenciador de log centralizado.

Estabelecer monitoramentos e alertas que detectem casos de atividades suspeitas em tempo hábil.

Estabelecer ou adotar uma plano de recuperação ou de resposta a incidentes.

### Referências

https://owasp.org/www-project-top-ten/2017/A10_2017-Insufficient_Logging%2526Monitoring

## Conclusão

Após entendermos sobre as vulnerabilidades mais comuns, podemos entender a maioria dos ataques feitos em aplicações web.

Esse top 10 é apenas uma parte do conteúdo gigantesco que a OWASP proporciona sobre segurança da informação. Se você trabalha na área é provável que vá acompanhar esse projeto aberto em grande parte da sua carreira.

Acredito que essa lista também passe uma boa noção para os desenvolvedores que querem manter suas aplicações um pouco mais seguras. Aplicando os conceitos tratados você com certerá deixará seu desenvolvimento extremamente mais seguro.

Obrigado por ler até aqui e qualquer dúvida você pode entrar em contato comigo pelo Linkedin.

Até mais ver =).