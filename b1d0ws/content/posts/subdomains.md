---
title: "Ferramentas Para Enumeração de Subdomínios"
date: 2022-03-02T15:20:50-03:00
---

Olá, pessoal!

No texto de hoje estarei mostrando as principais ferramentas para enumeração de subdomínios, técnica muito utilizada para colher informações e possíveis alvos para aumentar a superfície de ataque de um alvo.

A enumeração é uma das primeiras fases de um CTF, pentest ou análise de vulnerabilidade de um sistema web. Algo que os atacantes enumeram são os subdomínios que estão relacionados ao domínio principal da vítima.

Hoje vou demonstrar algumas ferramentas que são utilizadas nesse processo e ensiná-los a baixar e utilizar esses auxiliares.

Para as instalações abaixo funcionarem, é necessário que você tenha python3 e GO instalado na sua máquina.

## **sublist3r**

#### Instalação

```bash
git clone https://github.com/aboul3la/Sublist3r.git
cd sublist3r
sudo pip install -r requirements.txt
```

#### Uso
```bash
python3 sublist3r.py -h # Ver ajuda
python3 sublist3r.py -d $url # Definir o domínio a ser enumerado
python3 sublist3r.py -v -d $url # Enumerar no modo verboso
python3 sublist3r.py -b -d $url # Brute-force nos domínios
```

#### Exemplo

![sublist3r](/sublist3r.png)

## **amass**

#### Instalação

OBS: O comando `mv [ferramenta] /usr/local/bin` move o arquivo para a pasta /usr/local/bin fazendo com que a ferramenta possa ser invocada de qualquer diretório.

```bash
wget https://github.com/OWASP/Amass/releases/download/v3.17.0/amass_linux_arm64.zip
unzip amass_linux_arm64.zip
cd amass_linux_arm64
mv amass /usr/local/bin
```

#### Uso
```bash
amass enum -h # Ver ajuda
amass enum -d $url # Definir o domínio a ser enumerado
amass enum -silent -d $url # Não exibe a saída no terminal
amass enum -o exemplo.txt -d $url # Joga a saída para o arquivo exemplo.txt
```

#### Exemplo

![amass](/amass.png)

## **assetfinder**

#### Instalação

```bash
go get -u github.com/tomnomnom/assetfinder
# Localize aonde a ferramenta foi instalada.
# Como eu instalei com o usuário root, ela está em /root/go/bin/
mv /root/go/bin/assetfinder /usr/local/bin
```

#### Uso
```bash
assetfinder --subs-only $url # Definir o domínio (target) a ser enumerado
```

#### Exemplo

![assetfinder](/assetfinder.png)

## **findomain**

#### Instalação
```bash
wget https://github.com/findomain/findomain/releases/latest/download/findomain-linux
chmod +x findomain-linux
mv /root/go/bin/findomain-linux /usr/local/bin
```

#### Uso
```bash
findomain-linux -h # Ver ajuda
findomain-linux -t $url # Definir o domínio (target) a ser enumerado
findomain-linux -q -t $url # Não exibe a saída no terminal
findomain-linux -v -t $url # Modo verboso
findomain-linux -o -t $url # Joga a saída para um arquivo. O nome do arquivo será o nome do domínio no formato txt
```

#### Exemplo

![findomain](/findomain.png)

## **subfinder**

#### Instalação

```bash
GO111MODULE=on go get -v github.com/projectdiscovery/subfinder/v2/cmd/subfinder
mv /root/go/bin/subfinder /usr/local/bin
```

#### Uso

```bash
subfinder -d $url # Definir o domínio a ser enumerado
subfinder -silent -d $url # Não exibe a saída no terminal
subfinder -o exemplo.txt -d $url # Joga a saída para o arquivo exemplo.txt
```

#### Exemplo

![subfinder](/subfinder.png)

## **Script**

Para facilitar o uso dessas ferramentas eu criei um script que junta algumas tools citadas nesse artigo que encontra-se [nesse link](https://github.com/b1d0ws/enumSubs).

Note que esse é um script criado para uso pessoal, então ele conta com ferramentas que precisam estar pré-instaladas e acessíveis globalmente para funcionar corretamente, sendo elas:

- assetFinder
- findomain-linux
- subfinder
- amass
- anew
- httpx

Basicamente o script roda todas as ferramentas de enumeração e cria um arquivo para cada resultado, além de criar um arquivo com todos os subdomínios encontrados e outro com todos os que estão acessíveis.

![script](/script.png)

## **Conclusão**

Nesse artigo foram apresentadas 5 ferramentas para enumeração de subdomínios, sendo essa uma parte muito importante na hora de realizar um pentest, análise de vulnerabilidades, entre outras atividades que envolvem o mapeamento de um alvo.

Com a realização dessa enumeração, a superfície que pode sofrer ataques e possuir vulnerabilidades muitas vezes aumenta consideravelmente.

Até mais ver =).