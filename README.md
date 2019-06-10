# scraping_toolbox

## How Datadome protects website

Détection en temps réel : Critères techniques
Dans la première phase de détection, le module DataDome analyse les données techniques du visiteur. C’est un processus en temps réel ne nécessitant aucun accès ni aux disques, ni à la base de données.
L’analyse repose sur l’utilisation massive du cache en mémoire : Reverse DNS DataBase, réputation IP, et compteurs, intégralement stockés en mémoire.
Voici quelques uns des critères techniques analysés
UserAgent
Les navigateurs envoient leur nom (le UserAgent) avec chaque requête. Cet élément est purement déclaratif. Il est donc évidemment exclu de l’utiliser en whitelist (vous seriez surpris de voir le nombre de «  »GoogleBots » » qui crawlent depuis AWS…).
En revanche, s’appuyer sur le USerAgent pour constituer une blacklist est efficace pour bloquer les robots les plus basiques, qui représentent environ 20 % des bad bots. Que ce soit Nginx, Varnish, ou Apache, tous les web serveurs offrent la possibilité de définir des règles basées sur le UserAgent.
L’algorithme DataDome analyse également la validité du UserAgent. Par exemple, certains robots utilisent des générateurs de UserAgent, qui peuvent créer des combinaisons invalides (comme IE11 sur Windows XP). Un excellent moyen de les démasquer. De même, un trafic important provenant d’IE 5.5 ou Netscape est plus qu’improbable en 2018.
Réputation IP
Certains SysAdmin ont développé des outils maison ou mis en place Fail2Ban, une solution bien connue des utilisateurs de Linux, pour bloquer les adresses IP indésirables. Cependant, certains ISP utilisent une seule IP pour des dizaines, voire des centaines d’utilisateurs, ce qui peut conduire au blocage involontaire d’utilisateurs légitimes.
DataDome a constitué une base de données interne de réputation IP, tirant parti des milliards de visites que nous analysons chaque jour pour l’ensemble de nos clients. Cette base de données est mise à jour en permanence, afin que chacun de nos clients puisse bénéficier de l’intelligence collective et des connaissances acquises sur l’ensemble des sites et des API que protège la solution DataDome.
 
Propriétaire de l’adresse IP
La nature du propriétaire de l’adresse IP (ASN) et de son range (blocs CIDR) fournit également des informations précieuses. Est-ce un FAI, un hébergeur, une entreprise ou une organisation, et de quel type ? Où se situe l’adresse IP, et sa localisation est-elle cohérente avec l’audience du site ?
intégrité du Header
Chaque navigateur a sa propre implémentation HTTP. Cela nous permet de créer une base de données unique d’empreintes digitales, pour démasquer les faux navigateurs qui ne possèdent pas l’empreinte digitale parfaite.
Challenge JavaScript
Notre Challenge JavaScript présente à chaque visiteur un code JavaScript comprenant différents défis.
Les bots très basiques ne déclencheront probablement pas le JavaScript, ce qui est en soi un indice de détection. Mais nous allons bien au-delà. Notre Challenge JavaScript nous permet de détecter les technologies de crawling les plus avancées, telles que PhantomJS et même Chrome Headless.
Nous améliorons constamment nos Challenges JavaScript, afin de détecter des technologies de robots crawlers toujours plus sophistiquées.
Challenge Cookie
Dans le même esprit que le Challenge JavaScript, le Challenge Cookie envoie un cookie à chaque visiteur et demande au client web de le renvoyer. Les navigateurs légitimes vont le faire de manière transparente, tandis que de nombreux robots ne peuvent pas accepter les cookies et ne réussissent pas le test.
Détection en streaming : Données statistiques
Les hits qui passent avec succès la détection technique en temps réel sont ensuite soumis à une nouvelle analyse après les premières secondes d’activité. Cette analyse sera comparée aux normes statistiques.
Aux fins de cette analyse, DataDome capte toutes sortes de métriques dans des délais différents. Les valeurs sont ensuite comparées aux modèles standards correspondant aux comportements humains. Si un profil non standard est détecté, il est alors catégorisé comme un bot.
Voici quelques indicateurs sur lesquels repose la solution DataDome :
Nombre de hits par adresse IP : beaucoup de bots, particulièrement les web scrapers et les hackers, peuvent indexer des milliers de pages en quelques minutes, à la recherche de l’information pertinente ou des failles de sécurité.
Sessions par adresse IP : combien de sessions actives pour une adresse IP unique dans une période donnée ?
Vitesse de crawl (nombre de hits par minute) : un robot peut indexer et stocker une page de contenu en un rien de temps. Une adresse IP unique visitant un grand nombre de pages en un laps de temps court révèle fréquemment une activité frauduleuse.
Récurrence des hits : les bots suivent des règles strictes et précises en termes de fréquence des visites, vitesse de crawl etc.
Nombre de hits générant des erreurs 404 : les bots traquant les failles de sécurité logicielles génèrent des URL spécifiques à des applicatifs, essayant de détecter ainsi une brèche dans l’architecture de votre site web pour le pirater.
Bien qu’il soit rarement possible de prendre une décision éclairée en se basant uniquement sur de telles données, elles fournissent une contribution essentielle à nos algorithmes de surveillance en temps réel.
Détection comportementale
The final phase in our detection process is behavioral analysis. At this stage, only the most sophisticated bots have eschewed detection.

This analysis takes a little more time, and is performed asynchronously.

Sessions

Using cookies or session reconstitution through machine learning, session analysis provides extremely valuable insights to ensure optimal bot detection. Analyzing sessions allows us to come as close to the user as possible – and find out whether it’s man or machine.

Behavioral analysis at the session level is the most efficient criteria to define blocking patterns, as most legitimate users have a much greater data consumption than bots.

Of course, exceptions exist. Many passionate users can spend countless hours on a single forum thread, or keep track of a product listing for days to follow price evolutions and incoming comments. Used alone, session data are not sufficient.

Leveraging Big Data for optimum protection

As bots are becoming increasingly adept at imitating human users, the analysis of behavioural patterns becomes all the more important. To catch even the cleverest bots, we must go a lot further than basic pattern identification.

That’s why the DataDome bot detection solution makes use of Big Data to analyze the visitor’s path on the site.

Once set up, our solution tracks every hit your website receives. It gathers data from each individual user, human or not, and use an in-house blend of AI and machine learning for real-time comparison with our knowledge base of legitimate usage patterns.

## inspect outgoing HTTP requests of a single application

Obtain root in a terminal with

sudo -i

capture the RAW packets

sudo tcpdump -i any -w /tmp/http.log &

capture all the raw packets, on all ports, on all interfaces and write them to a file, /tmp/http.log.

Run your application. It obviously helps if you do not run any other applications that use HTTP (web browsers).

Kill tcpdump

killall tcpdump
To read the log, use the -A flag and pipe the output toless:

tcpdump -A -r /tmp/http.log | less
The -A flag prints out the "payload" or ASCII text in the packets. This will send the output to less, you can page up and down. To exit less, type Q


Some helpful flags (options):

-i Specify an interface
-i eth0

tcp port xx
tcp port 80

dst 1.2.3.4
specify a destination ip address

More informations about TCPDUMP here --> https://danielmiessler.com/study/tcpdump/
