#### OSINT : cas d'étude d'une vidéo YouTube

Cet investisseur immobilier nous montre son potentiel futur bien qu'il va acquérir, pour celà il nous fait un état des lieux en se rendant directement à l'adresse du bien indiquée, puis nous fera une séance improvisée d'urbex

On constatera que celui qui a monté la vidéo, a voulu flouter les informations pour ne pas faciliter la localisation du dit bien, toutefois, malgré ses précautions, vous constaterez qu'elles sont vaines.

La vidéo en question est celle-ci : https://www.youtube.com/watch?v=6f0H0imlokQ

Vidéo assez longue d'un peu moins d'une heure dont voici la miniature

<p align="center"><img src="http://i.imgur.com/eB1rZ8D.png"/></p>

Un premier visionnage rapide, me permet de trouver les passages intéressants pour collecter un maximum d'informations afin de déterminer la localisation de son bien immobilier

Vers le début de la vidéo, on remarquera qu'il sort de chez lui, passant devant le panneau d'une rue perpendiculaire à la sienne. Je n'ai pas pu déterminer tout de suite avec certitude le nom de la voirie

<p align="center"><img src="http://i.imgur.com/rCA9kjq.png"></p>


Tout ce que je peux savoir à ce moment là, c'est que la rue se termine par Alexandre, ce n'est qu'après que je comprendrais que le mot au dessus d'Alexandre désignait tout simplement le type de voie


<p align="center"><img src="http://i.imgur.com/lhpxu9H.png"></p>


A noter qu'on a une première indication rien qu'avec ce type d'architecture qui est typique des habitations du Nord constituées de briques rouges, avec parfois des contours en pierre blanche


<p align="center"><img src="http://i.imgur.com/nHxVff3.png"></p>


Sur plusieurs voitures, on peut deviner le numéro du département dans lequel il se trouve. Une des voitures affiche d'ailleurs clairement le numéro

A noter que pour extraire certaines informations ci-dessous, il m'a fallu ralentir la vitesse de la vidéo à son minimum (x0.25)

Vers le milieu de la vidéo, le vidéaste nous montre une partie de son cadastre en prenant là aussi le soin de ne pas révéler le nom des rues 

<p align="center"><img src="http://i.imgur.com/trBmS6i.png"  width="500px"/></p>

Nous avons aussi une petite portion d'une carte Google Map, en plein milieu se trouve un terrain de football ce qui nous avance à pas grand chose. Quant morceau du lieu dit se terminant par 'VEAUCOURT', malgré cette info précieuse, je ne trouve toujours pas

<p align="center"><img src="http://i.imgur.com/7TKDrec.png"  /></p>


J'ai tenté aussi de deviner les départementales qui cadrent son secteur, là aussi sans succès. On peut juste déduire qu'il y a au moins deux départementales signalées par les carrés jaunes. Un avec deux chiffres et l'autre route avec trois chiffres, mais les possibilités étant trop nombreuses j'abandonne l'idée de localiser avec ces éléments là..

Voyez d'ailleurs ici les nombreuses routes dans l'Oise https://routes.fandom.com/wiki/Liste_des_routes_d%C3%A9partementales_de_l%27Oise_(60)


A ce stade là, malgré les nombreuses données collectées, je ne parviens toujours pas à déterminer l'adresse exacte

J'en reviens finalement au premier élément que j'ai trouvé, à savoir ce panneau où il y est écrit 'Alexandre', je sais avec plus ou moins de certitude que nous sommes dans le département 60 qui est l'Oise..

Je décide alors de trouver sur internet une base de données regroupant toutes les rues de France, et plus précisément du département 60

Je tombe alors sur ce site https://www.lesruesdefrance.com/

Base de données très intéressante, je fouille un peu dessus avant de trouver où il trouve ses sources d'informations

Finalement je trouve un index d'archives regroupant toutes les rues par départements https://adresse.data.gouv.fr/data/ban/adresses/latest/csv/

Les données sous format CSV, et contient beaucoup de colonne ainsi que de très nombreux doublons. Ce qui est normal puisqu'une rue possède plusieurs numéros

Je décide alors de filtrer les données en supprimant les doublons, et ne m'intéressant qu'à deux colonnes : le nom de la voie et le code postal. J'utilise ce script pour parvenir à mes fins 

    import csv
    import json
    
    def red(filename):
        with open(filename, 'r') as f:return f.read()
    
    def ouate(data, filename):
        with open(filename,'w') as f:f.write(data)
    
    data = {}
    with open('data', mode='r') as csv_file:
        csv_reader = csv.DictReader(csv_file,delimiter=';')
        line_count = 0
    
        for row in csv_reader:
            a=row['nom_voie']
            b=row['code_postal']
    
            data[f'{a} {b}'] = ''
    
    data = [x for x in data.keys()]
    print(len(data))
    ouate(json.dumps(data,indent=4),"data.json")

J'obtiens une liste que je stocke en faisant un dump pour le sauvegarder dans un fichier data.json. J'ai une liste nettoyée et filtrée désormais sur laquelle je vais pouvoir effectuer mes recherches


<p align="center"><img src="http://i.imgur.com/7x9F9RO.png" width="350px"/></p>


Il ne me reste plus qu'à trouver le nom de la voie qui contient le mot Alexandre. Sachant que le panneau indiquait 'mot inconnu Alexandre', je tombe rapidement sur Sentier Alexandre..

A partir de là, je n'avais plus qu'à chercher sur Google Map la rue perpendiculaire à celle-ci

Et le tour était joué !

<p align="center"><img src="http://i.imgur.com/cxp3NCH.png"></p>

