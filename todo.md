# TODO

* ~~Corriger bug True/'True' dans webserver.https~~
* ~~mettre install logiciel complementaire en variable list~~
* ~~Récupérer Username + User password et les afficher à l'écran~~
* ~~Lancer peertube à la fin~~

* commandes d'upgrade
  * ~~recupération version actuelle~~
  * ~~arrêt peertube~~
  * ~~backup db~~
  * ~~upgrade~~
  * test
  * si fail : 
    * mettre anc version dans peer\_version, 
    * remettre le backup de la base
    * relancer ansible avec tag: install\_peertube, config
  
* check et stopper l'install si peertube est déjà présent
  * éventuellement une variable pour forcer l'installation.
