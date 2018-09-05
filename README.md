# Modèle Opale

Le modèle Opale est utilisé pour prévoir les principales variables macroéconomiques (croissance du PIB, consommation, investissement...) à horizon d’un à deux ans dans le cadre des exercices des projets de loi de finance et des Programmes de stabilité. Ainsi, dans le Programme de stabilité d’avril 2018, il a permis de faire la prévision macroéconomique pour les années 2018 et 2019. 
La documentation économique est décrite en détail dans le document de travail de la DG Trésor intitulé « la maquette de prévision Opale » publié en mai 2017 sous le numéro 2017/06. Il est disponible [ici](https://www.tresor.economie.gouv.fr/Articles/2017/05/19/la-maquette-de-prevision-opale-2017)

## Quel rôle joue un modèle pour faire des prévisions ?

Le rôle d’un modèle est multiple : il permet de s’assurer que les équilibres comptables de la comptabilité nationale sont respectés ; il permet de décrire les interdépendances entre les différentes variables macroéconomiques ; il permet d’évaluer les évolutions de chacune des variables d’intérêt à l’aune de leur comportement moyen sur le passé. 

En revanche, même avec un modèle, l’exercice de prévision reste un exercice discrétionnaire, et donc en aucun cas automatique. En effet, pour chaque trimestre en prévision, le prévisionniste doit choisir pour chacune des variables d’intérêt (consommation, investissement, etc.) de quelle manière elle pourrait dévier du comportement passé inscrit dans le modèle : il doit donc choisir la part de la variable d’intérêt « inexpliquée » par l’équation définissant son comportement passé. Par exemple, au moment du projet de loi de finances de septembre 2017, les comptes trimestriels étaient disponibles jusqu’au deuxième trimestre 2017, il fallait ainsi pour chaque variable faire une prévision sur six trimestres et donc définir 6 composantes inexpliquées (du troisième trimestre 2017 au quatrième trimestre 2018). Pour les principales variables macroéconomiques d’intérêt, qui sont de l’ordre d’une vingtaine, c’est donc plus d’une centaine de choix discrétionnaires que le prévisionniste doit faire pour déterminer son scénario économique en prévision. 

## Guide d'utilisation du modèle Opale

L'utilisation du modèle Opale se fait à l'aide du logiciel économétrique TROLL commercialisé en Europe par la société Hendyplan (http://www.hendyplan.com).
Pour des raisons contractuelles les codes sources sont publiés au format texte : il faut donc changer l’extension des fichiers du dossier ‘outils’ en .src sauf prepa_outils.txt à renommer en .inp ; et tous les autres fichiers texte en .inp. 

### Nomenclature utilisée pour les variables du modèle

Chaque série économique du modèle Opale dispose d'un identifiant unique inspiré par la nomenclature de la comptabilité nationale.

`TD_PIB7_CH` désigne par exemple le niveau du PIB en volume à prix chaînés, `TD_PIB5_CH` le déflateur du PIB et `TD_PIB3` le PIB en valeur.

Un dictionnaire des variables du modèle est disponible sous le dossier `/doc/dictionnaire_variables.txt`.


### Démarrage rapide
L’exécution du modèle nécessite au préalable de générer les bases de données au format .frm à partir de fichiers au format .csv, dans le dossier `inputs`, en exécutant le programme `prepa_inputs.inp`.
Ensuite, les outils nécessaires à l’exécution des programmes de simulation doivent être compilés, en exécutant la fonction `prepa_outils.inp` dans le dossier `outils`.
Le modèle est défini et estimé en lançant dans TROLL les programmes `macro1ch.inp`, `macro2ch.inp` et `macro3ch.inp`.

```shell
INPUT macro1ch; // importation et préparation des données
INPUT macro2ch; // définition du cadre comptable
INPUT macro3ch; // définition et estimation des équations de comportement
```

Une fois le modèle estimé, le programme `contrib.inp` permet de calculer les contributions trimestrielles et annuelles des déterminants des principales équations de comportement du modèle.

```shell
INPUT contrib;
```

Le programme crée alors différents fichiers CSV qui contiennent les sorties du modèle pour les principales équations de comportements (consommation des ménages, investissement des entreprises, emploi, importations, exportations). Pour chaque équation, deux fichiers CSV sont créés, un pour les contributions annuelles, un pour les contributions trimestrielles. Par exemple pour la consommation des ménages : `p3md7ctribann.csv` contient les contributions annuelles des déterminants de l'équation de consommation et `p3md7ctribtrim.csv` les contributions trimestrielles.

Les colonnes des fichiers de sortie du modèle reprennent la nomenclature définie dans `/doc/dictionnaire_variables.txt`. Les équations sont par ailleurs présentées en détail dans le document de travail  « la maquette de prévision Opale » disponible [ici](https://www.tresor.economie.gouv.fr/Articles/2017/05/19/la-maquette-de-prevision-opale-2017). Par exemple, pour l'équation de consommation des ménages, le fichier `p3md7ctribann.csv` donne pour chaque année (voir page 12 du document de travail) :
* `DLOG_OBS_ANN` : série observée
* `DLOG_SIMUL_ANN` : série simulée
* `CONTRIB_P3MD7_RDB_ANN` : contribution du revenu disponible brut (RDB)
* `CONTRIB_P3MD7_TCHO_ANN` : contribution du taux de chômage
* `CONTRIB_P3MD7_INDICAUTO_ANN` : contributions des indicatrices destinées à isoler les effets des mesures temporaires relatives aux achats de véhicules neufs
* `CONTRIB_P3MD7_TEMP_ANN` : contribution des écarts des températures à leur moyenne
* `CONTRIB_P3MD7_CALE_ANN` : contribution de l'inexpliqué		

### Actualiser les données du modèle
Les fichiers de données ou de préparation des données se trouvent dans le dossier `inputs` :
* `public10B.frm` : base TROLL que l’on crée grâce à `prepa_inputs.inp` à partir du fichier `ctrim.csv`, contenant les comptes trimestriels de l'Insee (résultats détaillés du 4ème trimestre 2017, qui sont les comptes disponibles au moment de la réalisation du Programme de stabilité d'avril 2018). 
 NB : ces données sont en base 2010, comme celles avec lesquelles le modèle Opale a été estimé pour la publication du document de travail en mai 2017. 
 Les données sur la période d'estimation du modèle ont été très légèrement révisées entre les deux (en raison des révisions de la correction des variations saisonnières, en particulier), ce qui peut conduire à des coefficients d'équations estimés légèrement différents de ceux publiés dans le document de travail du modèle Opale.
 Les comptes trimestriels publiés par la suite, à partir de mai 2018, sont en base 2014, et comportent donc des révisions plus importantes.
 
* `baseHypo.frm` : base TROLL que l’on crée à partir de `hypo.csv`, contenant l’environnement international (demande mondiale adressée à la France, prix du pétrole...) utilisé pour le Programme de stabilité 2018. Les données pour 2017 utilisées par le modèle ne sont pas toutes disponibles à date de publication.
* `ipcsj.csv` : fichier csv contenant les séries trimestrielles d'indice de prix à la consommation et des données relatives aux prix
* `stockcapital.csv` : données annuelles issues des comptes nationaux sur le stock de capital
* `autresinputs.csv` : diverses séries trimestrielles utiles pour le modèle comme le taux d'ouverture des pays de l'OCDE, l'effet des politiques de l'emploi ou les différentes indicatrices utilisées dans les équations.

## Présentation détaillée du programme


`macro1ch.inp` : ce programme TROLL importe les comptes trimestriels (public10B.frm), crée toutes les variables macroéconomiques utiles pour Opale qui n’existent pas directement dans les comptes trimestriels (les regroupements BMNA et DIM, le SMPT, le CSU) et ajoute aux comptes trimestriels toutes les autres variables externes utilisées, notamment les hypothèses internationales, l’indice des prix à la consommation.
 
`macro2ch.inp` : ce programme TROLL définit le cadre comptable du modèle et vérifie la cohérence des comptes trimestriels. Il assure l’équilibre comptable, en soldant sur certaines variables car les comptes trimestriels sont donnés au million près et peuvent ne pas être parfaitement équilibrés à l’euro près.

`macro3ch.inp` : ce programme TROLL construit le modèle Opale. Il ajoute l’ensemble des équations de comportement au cadre comptable du modèle, il estime les dynamiques de court terme des équations (les périodes d’estimation sont précisées en bas du programme), il inverse le modèle de façon à récupérer les cales de toutes les équations. Enfin, il vérifie que le modèle reproduit l’observé sur le passé.

`contrib.inp` : ce programme TROLL calcule les sorties du modèles pour les principales équations de comportement (séries simulées et contributions annuelles et trimestrielles des déterminants des équations) et les sauvegarde dans des fichiers CSV sous le dossier `/contrib`.

Le dossier `/contrib` contient des programmes TROLL appelés par `contrib.inp` pour chaque équation (`emploi.inp`, `p3md7ch.inp`, `p6dim7ch.inp`, `p7dim7ch.inp`, `p51sdhfz7ch.inp`). C'est aussi dans ce dossier que sont sauvegardées les sorties du modèle au format CSV.

Le dossier `/doc` contient le dictionnaire des variables du modèle ainsi qu'un fichier de correspondance entre la nomenclature des variables du modèle Opale et les codes "IdBank" utilisés par l'Insee (voir https://www.insee.fr/fr/information/2862759). 

Le dossier `/inputs` contient (voir rubrique "Actualiser les données du modèle") :
* les fichiers de données utilisés par Opale (`ctrim.csv`, `public10B.frm` une fois créé, `hypo.csv`, `baseHypo.frm` une fois créé, `ipcsj.csv`, `stockcapital.csv`, `autresinputs.csv`) ;
* les fichiers intermédiaires de construction des données (`prepa_inputs.inp`, `hypo.csv`, `ctrim.csv`).

Le dossier `/outils` contient des fonctions TROLL définies spécifiquement pour le modèle Opale (par exemple pour calculer des contributions, prolonger une série, calculer un acquis de croissance).

## Licence

Ce logiciel est régi par la licence CeCILL V2.1 soumise au droit français et respectant les principes de diffusion des logiciels libres. Vous pouvez utiliser, modifier et/ou redistribuer ce programme sous les conditions de la licence CeCILL V2.1. Le texte complet de la licence CeCILL V2.1 est dans le fichier `LICENSE`.

Les données diffusées dans le fichier `hypo.csv` sont régies par la « Licence Ouverte / Open License » version 2.0.





