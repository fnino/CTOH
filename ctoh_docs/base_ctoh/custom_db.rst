Base de données altimétrique CTOH personnalisée
===============================================

Objectif
--------

L'objectif d'une base de données personnalisée est de permettre à l'utilisateur
d'associer à la base de données CTOH et ces propres données altimétriques 
(gdrtype) afin d'utiliser les outils de la bibliothèque PyCTOH.

.. _db_manage_ref:

Fonctionnement de la base de données du CTOH
--------------------------------------------

Comme défini dans le chapitre :ref:`tree_strust_access_ref`, la base de données
du CTOH est une base de *produits* altimétrique. Un produit est défini par 5 
paramètres: mission, phase d'orbite, fournisseur, version produit et le type du
produit.


.. note::

    Par exemple, le produit *ja3_a_cnes_%_sgdr*. Il décrit le produit de la mission
    Jason-3 en phase A (orbite nominale) fourni par le CNES. Le symbole *%* indique
    que la version du produit n'est pas homogène (mélange GDR-D et GDR-f) et pour
    finir le type du produit : *sgdr*.


Comme défini dans le chapitre :ref:`db_structure_ref`, la base de données CTOH 
est une base de données de fichiers au format NetCDF structurée par produit 
dans une arborescence de répertoires.

.. _db_jason3_example_ref:

.. note::

    L'exemple ci dessous, montre la structure du produit *ja3_a_cnes_%_sgdr* dans 
    la base de données du CTOH.
    Le premier niveau de l'arborescence est le nom du produit, dans le deuxième 
    niveau sont les cycles d'orbite et le troisième niveau sont les paramètres de 
    type (définis par gdrtypes). Le type principale est fournis par les agences 
    spatiales : ceux sont les paramètres d'origines (gdr ou sgdr). Sans ces 
    paramètres origines, le produit ne peut exister.

    Les autres types de paramètres (gdrtypes) tels que *ctoh2* et *normpass*, sont 
    des paramètres complémentaires ajoutés par le CTOH dans sa base de données. 
    Dans ces répertoires gdrtypes se trouvent les fichiers NetCDF de 
    *paramètres type*.

    .. code-block:: text 

        ja3_a_cnes_%_sgdr   # Nom du produit dans la base de données CTOH
            ├── cycle_000   # Numéro du cycle
            │   ├── sgdr    # Produit sgdr original
            │   │   ├── JA3_GPS_2PTP000_118_20160212_020721_20160212_030333.nc
            │   │   ├── JA3_GPS_2PTP000_120_20160212_035946_20160212_045559.nc
            │   │   ├── JA3_GPS_2PTP000_122_20160212_055212_20160212_064825.nc
            │   │   └── ...
            │   │
            │   ├── ctoh2    # Paramètres CTOH complémentaires ajoutés au produit sgdr original
            │   │   ├── ctoh2_JA3_GPS_2PTP000_118_20160212_020721_20160212_030333.nc
            │   │   ├── ctoh2_JA3_GPS_2PTP000_120_20160212_035946_20160212_045559.nc
            │   │   ├── ctoh2_JA3_GPS_2PTP000_127_20160212_103316_20160212_112929.nc
            │   │   └── ...
            │   │
            │   │
            │   ├── normpass    # Paramètres CTOH complémentaires ajoutés au produit sgdr original
            │   │   ├── normpass_JA3_GPS_2PTP000_118_20160212_020721_20160212_030333.nc
            │   │   ├── normpass_JA3_GPS_2PTP000_120_20160212_035946_20160212_045559.nc
            │   │   ├── normpass_JA3_GPS_2PTP000_122_20160212_055212_20160212_064825.nc
            │   │   └── ...
            │    
            ├── cycle_001
            │   ├── ctoh2
            │   ├── gdr
            │   ├── gpd
            │   └── normpass
            ├── cycle_002
            │   ├── ctoh2
            │   ├── gdr
            │   ├── gpd
            │   └── normpass
            ├── ...
            .
            .


Base de données personnalisée
-----------------------------

*Non terminé*

La bibliothèque PyCTOH permet de compléter un produit de la base de données du
CTOH en ajoutant ses propres données complèmentaires (gdrtype). Pour cela il 
faut que :

    * le grdtype ajouté soit installé dans un espace de travail local de 
      l'utilisateur.

    * le grdtype personnelle respecte la même :ref:`structure que la base de données CTOH <db_manage_ref>`.

    * le gdrtype ait les mêmes variables dimensions et les mêmes dimensions que
      le :ref:`produit de référence <db_jason3_example_ref>` (gdr ou sgdr) mais 
      également contenir les mêmes paramètres de temps et de coordonnées.

    * le gdrtype ajouté soit décrit comme un nouveau produit (copie un fichier 
      description du produit existant et modification)

    * le nouveau produit soit ajouté dans le catalogue personnelle des produits
      de la base de données (copie du catalogue de la base et modification).

    * un fichier de structure (.dbfs) soit généré pour ce nouveau produit (alt_dbfs)

    * un fichier de métadata soit généré pour ce nouveau produit (alt_metadata)
    
    * la clé **dbfs_file** soit ajouté dans le fichier de description de 
      produit avec comme valeur le fichier dbfs du nouveau produit

    * la clé **metadata_file** soit ajouté dans le fichier de description de 
      produit avec comme valeur le fichier métadata du nouveau produit










