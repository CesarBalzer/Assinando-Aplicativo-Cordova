# assinando-app-cordova
Criando um aplicativo com Apache Cordova e assinando para subir na PlayStore

## Como utilizar

###Crie um novo projeto Cordova:

     cordova create appassinado br.com.appassinado AppAssinado
    
Entre no diretório do projeto:

     cd appassinado

Instale a plataforma Android:

    cordova platform add android
    
Execute seu código usando a flag --release para gerar o apk:

    cordova run android --release

Proximo passo vamos gerar a chave:

Sintaxe padrão:

    keytool -genkey -v -keystore <keystoreName>.keystore -alias <Keystore AliasName> -keyalg <Key algorithm> -keysize <Key size> -validity <Key Validity in Days>
  
No exemplo que utilizamos ficaria assim:

    keytool -genkey -v -keystore appassinado.keystore -alias appassinado -keyalg RSA -keysize 2048 -validity 10000

Em seguida será requisitado algumas informações:

    keystore password? : xxxxxxx
    What is your first and last name? :  xxxxxx 
    What is the name of your organizational unit? :  xxxxxxxx
    What is the name of your organization? :  xxxxxxxxx
    What is the name of your City or Locality? :  xxxxxxx
    What is the name of your State or Province? :  xxxxx
    What is the two-letter country code for this unit? :  xxx

Depois de gerada a chave, vamos assinar o apk com a ferramenta jarsigner utilizando esta chave:

Sintaxe padrão:

    jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 -keystore <keystorename <Unsigned APK file> <Keystore Alias name>

No exemplo que utilizamos ficaria assim:

    jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 -keystore /seu-caminho-para-o/appassinado.keystore /seu-caminho-para-o-apk-nao-assinado/android-release-unsigned.apk appassinado
 
Vai pedir a senha da assinatura:

    Enter KeyPhrase as 'xxxxxxxx'
 
Finalmente vamos gerar o apk para subir na playstore utilizando a ferramenta zipalign do seu sdk:
 
    /seu-caminho-para-o-zpalign/zipalign -v 4 /seu-caminho-para-o-apk-nao-assinado/android-release-unsigned.apk /seu-caminho-de-saida-para-o-apk-assinado/appassinado.apk

Agora você já poderá fazer o upload do apk na PlayStore!!!



## Mais Informações

Para mais informações de como configurar o Apache Cordova acesse a documentação [Documentação Apache Cordova](http://cordova.apache.org/docs/en/latest/guide/cli/index.html)

Para mais informações dos plugins acesse [Guia de desenvolvimento de plugins](http://cordova.apache.org/docs/en/latest/guide/hybrid/plugins/index.html)

