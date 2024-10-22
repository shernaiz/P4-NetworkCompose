# P4-NetworkCompose

# *Docker Compose*

# 1-Crea unha rede en docker

Para crear unha rede en docker teremos que lanzar o comando `docker network create -d bridge mi-red-puente` onde a opción `-d`indica qué controlador da rede se vai empregar, neste caso especificase `bridge`, este controlador permite que varios contenedores que estén conectados na misma rede "bridge" se comuniquen entre eles.

# 2-Crea dous contenedores unidos a esa rede

Para crear os dous contenedores teremos que lanzar o comando de creación destos indicando a rede a que se unen, `docker run -itd --network=mi-red-puente busybox`. Con este comando crearemos o contenedor na rede que indicamos. Faremos estos o mismo comando para lanzar o outro contenedor.

# 3-Comprobar que os contenedores están na rede

Para comprobar que os contenedores están na rede teremos que usar o coamndo `docker network inspect mi-red-puente` isto mostrarános un montón de información sobre os contenedores que se atopan a rede, a nos neste paso só nos interesará ver que efectivamente están os nomes dos contenedores que lanzamos.

# 4-Comprobar que os contenedores poden verse entre eles

Para comprobar que os contenedores poden encontrarse entre eles teremos que entrar dentro dun deles e faer o comando `ping` ao outro. 

Para entrar dentro dun contenedor teremos que usar o comando `docker exec -it sad_stonebraker sh` onde *sad_stonebraker* e o nome do contenedor o que accederemos. Unha vez dentro solo teremos que lanzar o comando `ping affectionate_dirac` sendo *affectionate_dirac* o nome do outro contenedor ao que lle faremos ping.

# 5-Listar os contenedores conectados a red

Para listar os contenedores conectados a rede podremos lanzar o comando de antes `docker network inspect mi-red-puente` e diranos os contenedores que se atopan na rede. Sin embargo se queremos filtrar a información para que só mostre o nome e sexa mais claro podremos concatenar o comando para filtrar a información do seguinte xeito `docker network inspect mi-red-puente | jq -r '.[0].Containers[] | .Name'`.

# 6-Listar as propiedades da rede 

Para listar as propiedades da nosa rede teremos que volver a lanzar o comando `docker network inspect mi-red-puente`. No despliegue de información que me dará o comando podremos ver as propiedades da rede como no nome, a fecha da creación, o tipo de driver, se ten activada a IPv6 entre outras cousas.

# 7-Crear outra rede

Para crear outra rede lanzaremos o comando de antes pero con outro nome. Neste caso será `docker network create -d bridge red2-redemption`. isto crearanos a rede.

# 8-Lanzar dous contenedores novos conectados a esa nova rede

Para esto repetiremos o do anterior paso de lanzar os contenedores conectadoas a rede co seguinte comando `docker run -itd --network=red2-redemption busybox`. Lanzaremos o comando dúas veces para que lance os dous contenedores.

# 9-Comproba as posibles conexións entre os 4 contenedores

Para comprobar as posibles conexións entraremos dentro dun contenedor e logo faremos ping aos contenedores que temos activos. Cando entremos no contenedor veremos que só nos deixa facer ping aos contenedores que se encotnren na misma rede que este, aos que non se atopen nesa rede saldrá a seguinte mensaxe : *ping: bad address 'clever_herschel'*

# *Docker Compose*

# 1-Segue os pasos da guía de iniciación de docker-compose, e explica coas túas palabras os pasos que segues e que fan

Docker Compose  é unha ferramenta que nos permite xestionar aplicacións multi contenedor de forma sinxela. Isto faino mediante un archivo *YAML* que vería sendo como un archivo de configuración onde aparecen especificadaos os servicios volúmenes e redes necesarias para a nosa aplicación.

Ademáis, unha vez feito permítenos controlar mediante un comando a creación e a execución de todos os contenedores que se encontran definidos no archivo YAML.

**PRIMEIROS PASOS**

Para utilizar esta ferramenta o primeiro será ter instalado o docker-compose, neste caso xa temos feito este paso, así que o seguinte sería crear unha carpeta onde meteremos o noso proyecto, e nesta carpeta crearemos algúns arquivos:

**1-** Primeiro crearemos un arquivo chamado `app.py` no que teremos que pegar un código que nos facilitan na propia páxina. 

**2-** O seguinte será un arquivo chamado `requirements.txt`, tamén no directorio do noso proxecto, e tamén teremos que copiar un código que nos proporciona a páxina.

**3-** O seguinte arquivo será `Dockerfile` onde pegaremos tamén outro código, este será o que lle dí a docker certas directrices que debe seguir como lanza a imaxe ou establece o enviroment. este arquivo non terá extensión e en caso de que se cree con extensión podrá darnos error.

**DEFINICIÓN DE SERVIZOS NUN ARQUIVO DOCKER**

Para a definición dos servizos teremos que crear o arquivo `compose.yaml` e teremos tamén que pegar un código que nos proporciona a propia páxina. Este documento o que fará será definir dous servizos, **web** e **redis**. 

O servizo web empregará unha imaxe que se construirá directamente desde o `Dockerfile` que se encontra no propio directorio.

O servizo redis emprega unha imaxe redis pública diréctamente desde o Docker Hub.

**Construir e correr a aplicación con Compose**

Agora que xa temos todo configurado, con un só comando poderiamos crear e comezar todos os servicios que temos no noso arquivo de configuración.

Primeiro teremos que arrancar a aplicación mediante o comando `docker compose up`.

O seguinte será introducir a seguinte dirección no nosos navegador: `http://localhost:8000/`. Isto levarános a unha páxina co texto de _Hola mundo_ e a cantidade de veces que se visitou a páxina. Se refrescamos a páxina o número debería aumentar.

Logo podremos acceder a una ventana do terminal onde poremos o comando `docker image ls` e mostrarános unha lista cas imaxes locales. 

Finalizaremos a aplicación co comando `docker compose down`. E con esto xa teríamos o Docker compose configurado, en caso de querer cambiar algunha cousa só teriamos que cambiar o arquivo de configuración, é decir o `compose.yaml`.

# 2-Agora que sabes algo máis de docker-compose, crear un arquivo (ou varios) de configuración que ó ser lanzados cun docker-compose up, resulten nunha rede docker á que estean conectados 3 contenedores, explica os parámetros do .yaml usado

Para modificar un arquivo `.yaml` e que nos permita lanzar tres contenedores só lanzando o comando `docker compose up` teremos que modificalo para que fose da seguinte forma, dividirémolo en partes:

**Parte 1:** A cabeceira sera o principio do documento, onde teremos que indicar a versión, e a rede e o tipo de rede que empregaremos.

**Parte 2:** A continuación pero de forma anidada teremos que indicar os contenedores por separado, un a continuación do outro. Teremos que indicar os seguintes campos do contenedor: a imaxe, o nome do contenedor, a rede á que conectamos e o porto no que se vai a aloxar.

Cando teñamos todo esto indicado no arquivo podremos lanzar o coamndo e arrincarán os tres contenderoes na rede indicada.

**NOTA**: No documento .yaml só funcionará se separamos do inicio aqueles campos que se atopan dentro de outros. Isto teremos que facelo mediante espacios e non tabulación xa que se nn daranos error.

Se todo esta correcto, ao lanzar `docker compose up` aparecerános a información dos contenedores arrincados e creados así como da rede.

# 3-Busca e proba 4 parámetros e configuracións diferentes que podes incluir no arquivo compose, explica qué fan (por exemplo diferentes cousas que facer coa opción RUN).

**restart:allways** -> En caso de que por alguna circunstacia se detenga un contenedor, este parámetro hará que se reinicie.

**depend on:** -> Este parámetro nos permite que un contendor dependa de otro, haciendo que el dependiente no se iniciará hasta que se inicie el otro.

**helathcheck** -> Este parámetro permitenos corroborar o estado dun servicio de forma periódica. É dicir, este parámetro permitiranos saber se o noso contenedor está correndo de forma correcta.

**expose** -> Este parámetro permitritanos expoñer os portos, isto implicará que os portos só estarán dispoñibles para os servicios vinvulados.