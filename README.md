# assinando-app-cordova
Tutorial de criação e asssinatura de um aplicativo com Apache Cordova para fazer upload na PlayStore

## Como utilizar

### Crie um novo projeto Cordova:

     cordova create appassinado br.com.appassinado AppAssinado
    
### Entre no diretório do projeto:

     cd appassinado

### Instale a plataforma Android:

    cordova platform add android
    
### Execute seu código usando a flag --release para gerar o apk:

    cordova build android --release

### Proximo passo vamos gerar a chave:

#### Sintaxe padrão:

    keytool -genkey -v -keystore <keystoreName>.keystore -alias <Keystore AliasName> -keyalg <Key algorithm> -keysize <Key size> -validity <Key Validity in Days>
  
### No exemplo que utilizamos ficaria assim:

    keytool -genkey -v -keystore appassinado.keystore -alias appassinado -keyalg RSA -keysize 2048 -validity 10000

### Em seguida será requisitado algumas informações:

    keystore password? : xxxxxxx
    What is your first and last name? :  xxxxxx 
    What is the name of your organizational unit? :  xxxxxxxx
    What is the name of your organization? :  xxxxxxxxx
    What is the name of your City or Locality? :  xxxxxxx
    What is the name of your State or Province? :  xxxxx
    What is the two-letter country code for this unit? :  xxx

### Exemplo da saída quando gerada a chave (Usando as informações que foram requisitadas acima)

    Informe a senha da área de armazenamento de chaves: xxxxxxxxx 
    Informe novamente a nova senha: xxxxxxxxx
    Qual é o seu nome e o seu sobrenome?
      [Unknown]:  App Assinado
    Qual é o nome da sua unidade organizacional?
      [Unknown]:  App Assinado
    Qual é o nome da sua empresa?
      [Unknown]:  App Assinado
    Qual é o nome da sua Cidade ou Localidade?
      [Unknown]:  Ponta Grossa
    Qual é o nome do seu Estado ou Município?
      [Unknown]:  Parana
    Quais são as duas letras do código do país desta unidade?
      [Unknown]:  BR
    CN=App Assinado, OU=App Assinado, O=App Assinado, L=Pont Grossa, ST=Parana, C=BR Está correto?
    [não]:  sim

    Gerando o par de chaves RSA de 2.048 bit e o certificado autoassinado (SHA256withRSA) com uma validade de 10.000 dias
	para: CN=App Assinado, OU=App Assinado, O=App Assinado, L=Ponta Grossa, ST=Parana, C=BR
    Informar a senha da chave de <appassinado>
	(RETURN se for igual à senha da área do armazenamento de chaves):  xxxxxxxxx
    Informe novamente a nova senha: xxxxxxxxx
    [Armazenando appassinado.keystore]

### Depois de gerada a chave, vamos assinar o apk com a ferramenta jarsigner utilizando esta chave:

#### Sintaxe padrão:

    jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 -keystore <keystorename <Unsigned APK file> <Keystore Alias name>

### No exemplo que utilizamos ficaria assim:

    jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 -keystore appassinado.keystore platforms/android/build/outputs/apk/android-release-unsigned.apk appassinado
 
### Vai pedir a senha da assinatura:

    Enter KeyPhrase as 'xxxxxxxx'
 
### Finalmente vamos gerar o apk assinado utilizando a ferramenta zipalign do seu sdk:
 
    /seu-caminho-para-o-zipalign/zipalign -v 4 /seu-caminho-para-o-apk-nao-assinado/android-release-unsigned.apk /seu-caminho-de-saida-para-o-apk-assinado/appassinado.apk

### Seu apk está pronto para ser colocado na PlayStore

## Opcional:

### Podemos especificar um build.json com o armazenamento das chaves para rodar o debug e release mais facilmente

> Alternativamente, você pode especificá-los em um arquivo de configuração de compilação (build.json) Usando o argumento --buildConfig para os mesmos comandos. Aqui está um exemplo de um arquivo de configuração que deve ser adicionado em seu projeto, eu particularmente adicionei dentro da minha pasta js.

```javascript
{
    "android": {
        "debug": {
            "keystore": "../../appassinado.keystore",
            "storePassword": "123456789",
            "alias": "appassinado",
            "password": "123456789",
            "keystoreType": ""
        },
        "release": {
            "keystore": "../../appassinado.keystore",
            "storePassword": "123456789",
            "alias": "appassinado",
            "password": "123456789",
            "keystoreType": ""
        }
    }
}

```
### Execute o comando para ler a configuração:

    cordova build android --release --buildConfig

Para mais informações de como configurar a assinatura do app [Assinando um aplicativo](http://cordova.apache.org/docs/en/latest/guide/platforms/android/index.html#signing-an-app)


## Mais Informações

Para mais informações de como configurar o Apache Cordova acesse a documentação [Documentação Apache Cordova](http://cordova.apache.org/docs/en/latest/guide/cli/index.html)

Para mais informações dos plugins acesse [Guia de desenvolvimento de plugins](http://cordova.apache.org/docs/en/latest/guide/hybrid/plugins/index.html)

