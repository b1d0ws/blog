---
title: "Minha experiência com a DCPT"
date: 2022-05-23T09:20:50-03:00
---

![Banner DCPT](/dcpt/banner.png)

Olá, pessoas, como vão? No texto de hoje irei relatar minha experiência com uma das certificações de segurança mais conhecidas no Brasil, a DCPT da Desec.

## Quem Sou Eu?

Primeiramente, deixe-me ambientar vocês falando um pouco sobre mim. Meu nome é Eduardo Bido, estou cursando o 5° semestre de Ciências da Computação na IMED | Passo Fundo e atualmente trabalho como Analista de Segurança de Software, em home office.

## A Certificação

A DCPT (Desec Certified Penetration Tester) é uma certificação prática que tem por objetivo provar que o profissional/estudante possui os conhecimentos de um Pentester. Eu acredito que ela seja válida para um nível de início de carreira parelho a senioridade Júnior.

A certificação é obtida após realizar uma prova que tem como objetivo final comprometer 5 máquinas vulneráveis em 24 horas e depois realizar um relatório técnico, também no período de 24 horas. Ou seja, você tem dois dias inteiros para finalizar o exame, porém limitado a 24 horas para cada etapa. Caso você não consiga comprometer os 5 hosts no primeiro dia, você já estará reprovado e não precisa prosseguir para o dia seguinte.

Para cada máquina você tem um flag para alcançar e essa flag deve ser inserida enquanto a prova está sendo realizada. Também é necessário evidenciar a obtenção delas no relatório.

## Estudos

Para estudo, a DESEC recomenda que você complete o curso Novo Pentest Profissional e o Pentest Experience para ter o conhecimento suficiente para o exame.

O curso Novo Pentest é bem extenso, envolvendo várias aulas de diversos tópicos, desde redes até ataques internos em infraestrutura e também possui a parte prática, envolvendo laboratórios que acompanham os conteúdos lecionados nas videoaulas. Para finalizar essa parte, eu demorei cerca de 10 meses não contínuos, ou seja, quando cheguei em 40% das aulas eu dei uma longa pausa para focar em outros conteúdos.

Nessa pausa eu gastei boa parte do tempo na faculdade, nos estudos da Security+ e também no final de 2021 eu comecei a fazer CTFs no TryHackMe e posso dizer que o aprendi nessa plataforma foi de extrema importância para facilitar meu aprendizado quando retornei para a DESEC.

Em abril eu terminei o Novo Pentest e adquiri o Pentest Experience. Essa segunda parte consta de 9 desafios práticos que tendem a ser semelhantes às máquinas que aparecerão no exame. Em maio eu finalizei esses laboratórios e garanto que eles são bem mais difíceis que os hosts da prova.

Assim que finalizei o Experience, agendei meu exame. É obrigatório que você conclua todos os labs para liberar o agendamento e então você pode escolher o dia em que vai realizar a prova final. Você não pode marcar na mesma data que outro aluno, mas na hora de agendar vai aparecer um calendário exibindo as datas disponíveis.

## Custos

Sendo claro e direto, os custos envolvidos para tirar a DCPT foram:

R$1500 para o Novo Pentest Profissional  
R$500 para o Pentest Experience

Dei uma pesquisada e vi que os valores estão um pouco mais caros em 2022. Recomendo você falar com a sua empresa para ver se eles fazem o apoio financeiro na realização dessa certificação. Como a DESEC é Brasileira talvez seja mais fácil.

## Exame

Marquei meu exame para às 08:00 horas do dia 16 de maio, então eu teria até dia 18 nesse mesmo horário para fazer a entrega do relatório.

Acordei umas 7:30h, tomei um café para acordar e me preparei para ficar as próximas horas sentado na cadeira. Chegada às 8 horas, recebi os arquivos para fazer a conexão com a VPN da DESEC e iniciei meu exame.

Em cada host você tem uma leve introdução do que se trata a máquina alvo, então isso pode direcionar você sobre o que procurar no sistema. Lembre-se também de evidenciar tudo que você fizer. Isso vai fazer muita diferença na hora de produzir o relatório e a falta de evidência pode fazer você reprovar caso não consiga comprovar como realizou os ataques.

O primeiro host era o Buffer Overflow. Se você sabe o básico do básico de BoF, você vai conseguir explorar essa máquina tranquilamente. Como meu conhecimento nessa área de binário e executáveis é bem escasso, eu apanhei um pouco e demorei cerca de uma hora e meia para conseguir a flag. Acredito que dê pra fazer em muito menos tempo.

Nota: é de conhecimento público que uma das máquinas envolve Buffer Overflow, então posso relatar isso no artigo sem quebrar a confidencialidade da prova.

Logo em sequência fui para o segundo host e comecei muito bem. Consegui algumas informações iniciais que me levariam ao comprometimento do alvo, mas não consegui progredir em certo momento, então fui para a próxima máquina

Para resumir, do segundo ao quinto host eu realizei o reconhecimento inicial, consegui algumas informações, mas em algum momento travei. Nesse momento bateu um certo nervosismo, porque percebi que a prova não estava tão fácil e já tinham se passado 3 horas.

Voltei à terceira máquina, pois ela parecia ser mais fácil e consegui comprometê-la até 12:30h. Assim que conquistei a flag, dei uma pausa e fui almoçar. As pausas são extremamentes importantes para você refrescar a cabeça e reordenar os pensamentos.

Assim que voltei, às 13 horas, fui para o quinto host, pois ele parecia mais fácil também. Em questão de 40 minutos a máquina já estava totalmente comprometida. Nesse momento me senti bem confiante, pois não tinham passado nem 6 horas de exame e eu já havia comprometido 60% dos hosts.

A partir disso fiquei alternando entre a máquina 2 e 4 até umas 17 horas. Nessa hora encontrei mais uma informação do quarto host e decidi focar nele. Mais uma hora e meia, obtive a flag e decidi fazer mais uma pausa mais longa, dessa vez para tomar banho e jantar.

Voltando ao segundo host às 19:30h, eu tinha 12:30h para comprometer um host, então continuava confiante no processo. Contudo, o período da tarde foi bem exaustante e eu já estava sentindo sinais de cansaço, afinal eu estava quase há doze horas realizando o exame.

Eu tinha a informação de como explorar a máquina, porém não achava o modo correto de forma alguma e nessa brincadeira foram algumas horas. Eram 2 horas e 30 da manhã, faltavam 5:30h para esgotar as 24 horas da primeira etapa e eu já tinha pensado na desistência algumas vezes.

Eu estava extremamente cansado e após diversas xícaras de café a cafeína já não fazia mais efeito no meu corpo. Já tinha tentado começar do zero para ver se tinha deixado algo passar, porém o resultado sempre era o mesmo. Repentinamente tive uma ideia que nunca havia aplicado em nenhum CTF ou laboratório e ela funcionou!!

Nesse momento é uma explosão de emoção e adrenalina que você esquece que já está há mais de 18 horas realizando o exame. A partir desse ponto a exploração fluiu e consegui comprometer o host e obter a última flag!

Antes de submeter essa flag, passei pelas minhas evidências para ver se tinha capturado tudo que era necessário para o relatório, pois ao submeter todas as flags o seu acesso ao hosts é encerrado.

Finalizei esse processo de comprometer as máquinas e fui dormir. Um detalhe importante que eu não havia percebido é que as próximas 24 horas para fazer o relatório começam assim que você submete todas as flags, e não a partir do horário que você começou no dia anterior, no meu caso às 8 da manhã.

Como eu tinha compromisso pela manhã, comecei a produção do relatório na parte da tarde e finalizei até a noite. O tempo é bem tranquilo, ainda mais utilizando o template que a DESEC disponibiliza. Dei uma boa revisada e entreguei por volta das 8 horas da noite, totalizando 36 horas de exame.

Recebi um e-mail informando que receberia o resultado em até 5 dias úteis e então bastava ansiosamente aguardar. Três dias depois, o resultado veio e eu estava aprovado.

## Conclusão

A trilha da DESEC é interessantíssima para quem está começando na área de segurança ofensiva/pentest e creio ser um dos conteúdos em português mais completos nessa área.

Reforço meu pensamento que a certificação DCPT está em um nível iniciante, semelhante ao conhecimento de um Pentester Júnior, mas acho válida para todos, pois é uma experiência única.

Ela de certa forma vai testar que você, além de conhecer diversas ferramentas e ataques, tem outras habilidades como gestão de tempo e escrita com relatórios.

Caso você tenha alguma dúvida sobre o curso e a certificação ou só queira trocar uma ideia, sinta-se à vontade para entrar em contato comigo pelo [LinkedIn](https://www.linkedin.com/in/eduardo-bido-541430193/) que irei responder com muito gosto.

Até mais ver =).

