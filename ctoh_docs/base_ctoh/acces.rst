Moyens d'accès à la base CTOH
=============================


L'accès à la base de donnée CTOH se fait exclusivement depuis les serveurs
du LEGOS (santo et malo) ou depuis les serveurs du CTOH (gctoh, cctoh et boto).
          
Il y a deux manières d'accéder aux données:
    - par accès direct aux répertoires des produits
    - à l'aide des outils PyCTOH:
        * en ligne de commande
        * API de la bibliothèque


.. _tree_strust_access_ref:

Accès direct à l’arborescence  
------------------------------

Le contenu des produits de la base CTOH se trouve dans :
    
.. code-block:: text
    
        /ctoh/data/db/products
        
    
La liste des répertoires de produits disponibles en base est la suivante:

.. code-block:: text
    
    cs2_b_gop_c_gdr/            ja1_a_cnes_e_gdr/       s3a_a_lan_%_sgdr/
    cs2_b_ice_d_gdr/            ja1_b_cnes_e_gdr/       s3a_a_wat_%_sgdr/
    env_a_ctoh_v0210_gdr/       ja1_c_cnes_e_gdr/       s3b_a_lan_%_sgdr/
    env_a_esa_v0300_sgdr/       ja2_a_cnes_d_gdr/       s3b_a_wat_%_sgdr/
    env_b_ctoh_v0210_gdr/       ja2_b_cnes_d_gdr/       s3b_b_lan_%_sgdr/
    env_b_esa_v0300_sgdr/       ja3_a_cnes_%_sgdr/      s3b_b_wat_%_sgdr/
    ers2_a_ctoh_v0100_gdr/      ja3_a_cnes_f_sgdr/      srl_a_cnes_%_sgdr/
    ers2_a_reaper_v0108_gdr/    ja3_a_cnes_%_sigdr/     srl_b_cnes_%_sgdr/
    ers2_a_reaper_v0108_sgdr/                           srl_b_cnes_%_sigdr/
                                                        tpx_a_cash_v0100_gdr/

Chaque produit est répertorié dans un répertoire identifié par un nom unique comportant 5 champs détaillés ici:
    
    :mission: Le nom de la mission (en abrégé: ja1 pour jason-1, etc)
    :phase: phase d'orbite de la mission (on commence par a, puis b si elle change, etc)
    :fournisseur: Nom de l'organisme ayant traité/fournis les données (cnes,esa,ctoh), cela peut être aussi le nom d'un projet (cash, reaper).
    :version: version du produit si homogène ("e" pour GDR-E ou "v0210" pour version 2.1) sinon le caractère % est utilisé (mélange de version).
    :type: le type de produit d'origine (gdr, sgdr, sigdr).
    
    
Le contenu du produit est organisé par cycle et par sous produit. 
Il existe plusieurs types de sous produit. Le principal est de type gdr, sgdr ou sigdr. Ils fournissent
les paramètres originaux de la mesure altimètrique. Les autres types sont secondaires et complétent
la mesure altimètrique avec des corrections ou paramètres complémentaires. Voici les types secondaire que l'on peut trouver:
    
    :CTOH1: rassemble des paramètres atmosphériques et de marée calculés à 1 Hz,
    :CTOH2: regroupe des paramètres interpolés provenant de données grillées (distance à la cote, géoïde, mdt,mss, etc).
    :GPD: pour la correction de troposphère humide et sèche fournie par J Fernandez de l’université de Porto.
    :Normpass: correspond aux paramètres d'indexation calculés le long de la trace, permettant la création de la structure de trace normalisée pour l'analyse colinéaire de données altimétrique
    :Meanpass: traces moyennes du produit calculées avec les traces normalisées

.. warning::
    Les fichiers de données sont au format NetCDF. **Mais ils ne sont pas tous de la même version : netCDF3, netCDF4 classic model ou netCDF4/HDF-5.**




Accès par la bibliothèque PyCTOH 
--------------------------------

Le module PyCTOH est une bibliothèque de programmes en ligne de commande 
permettant l'accès rapide et facile au contenu de la base de données CTOH.

Cette bibliothèque est accessible uniquement sur les serveurs du LEGOS 
(santo et malo) et sur les serveurs du CTOH (gctoh, cctoh et boto). 
De plus, il faut appartenir au groupe usrcto.

Le chargement de cette bibliothèque se fait, comme ci-dessous, à l'aide de la commande *module*. 

.. code-block:: bash
    
    > module purge        # Remove all loaded module
    > module load pyctoh  # Load PyCTOH module


Une fois que le module chargé, vous avez accès aux outils PyCTOH grace à la commande : **alt**

Voici ci dessous les 3 fonctionnalitées de cette commande:

    :info: fourni des informations sur la base de données.
    :tracks: fourni des informations sur les traces des missions.
    :extract: permet d'extraire des données altimètriques de la base de données.


Info
~~~~

Cette fonctionalité permet d'afficher des informations de la base de données et de son contenu.
Elle est accessible à l'aide de la sous-commande ``info`` de la commande ``alt``. 
Ci dessous, l'affichage de l'aide (option -h) pour la sous-commande ``info``:

.. code-block:: bash
    
    ❯ alt info -h
    usage: alt info [-h] {env,mission,product} ...
    
    optional arguments:
      -h, --help            show this help message and exit
    
    info:
      information on environment configuration
    
      {env,mission,product}
        env                 environment information
        mission             information on missions
        product             information on products


Trois types d'informations sont disponibles:

    - **env** : renvoi des informations sur les variables d'environnements de la base de données

    .. code-block:: bash

            ❯ alt info env
            CTOH Database configuration:
            --------------------------------------------------------------------------------
             Catalogue : /ctoh/data/db/catalogue_yml/ctoh_products_base.yml
             ALT_CATALOGUE : /ctoh/data/db/catalogue_yml/ctoh_products_base.yml
             CTOH_SHARED_DATA : /ctoh/data/shared
             Mode : yml
            --------------------------------------------------------------------------------


    - **mission** : renvoi des informations sur les missions altimètriques disponibles en base
    
        * La liste des missions

        .. code-block:: bash

            ❯ alt info mission
            Missions list available in the data base :
            ['Cryosat-2',
             'ERS-2',
             'Envisat',
             'Jason-1',
             'Jason-2',
             'Jason-3',
             'Saral',
             'Sentinel-3A',
             'Sentinel-3B',
             'Sentinel-6A',
             'Topex']
             



        * Détailles des produits dispinibles pour la mission ``Jason-1``

        .. code-block:: bash

            ❯ alt info mission jason-1
             Missions list and products available in the CTOH database:
            -----------------------------------------------------------------------------------------------------------------
                | Mission Name | Product Name              | Product version         | Orbit | GDR/SGDR/SIGDR and CTOH addons
                |              |                           |                         | phase |
            -----------------------------------------------------------------------------------------------------------------
              1 | Jason-1      | ja1_a_cnes_e_gdr          | GDR-E, GDR              | A     | ['gdr', 'normpass', 'ctoh1', 'ctoh2', 'gpd', 'fes14', 'l2p']
              2 | Jason-1      | ja1_b_cnes_e_gdr          | GDR-E, GDR              | B     | ['gdr', 'ctoh2', 'normpass', 'gpd']
              3 | Jason-1      | ja1_c_cnes_e_gdr          | GDR_E, GDR              | C     | ['gdr', 'ctoh2']

            For more details about the mission and its products : alt -v info mission <mission_name>
                 or even more details: alt -vv info mission <mission_name>
            For all mission : alt info mission ?

            The products documentations are available : https://groupes.renater.fr/wiki/ctoh_news
         


        * Avec plus de détailles : option ``-v``

        .. code-block:: bash

            ❯ alt -v info mission jason-1
             Missions list and products available in the CTOH database:
            ------------------------------------------------------------------------------------------------------------------------------------------------------
                | Mission Name | Product Name              | Provider             | Product version         | Orbit | Orbit ref.  | GDR/SGDR/SIGDR and CTOH addons
                |              |                           |                      |                         | phase |             |
            ------------------------------------------------------------------------------------------------------------------------------------------------------
              1 | Jason-1      | ja1_a_cnes_e_gdr          | CNES                 | GDR-E, GDR              | A     | topex       | ['gdr', 'normpass', 'ctoh1', 'ctoh2', 'gpd', 'fes14', 'l2p']
              2 | Jason-1      | ja1_b_cnes_e_gdr          | CNES                 | GDR-E, GDR              | B     | topex_inter | ['gdr', 'ctoh2', 'normpass', 'gpd']
              3 | Jason-1      | ja1_c_cnes_e_gdr          | CNES                 | GDR_E, GDR              | C     | jasong      | ['gdr', 'ctoh2']
            The products documentations are available : https://groupes.renater.fr/wiki/ctoh_news
         


        * Ou encore, la liste des produits pour les missions ``jason``

        .. code-block:: bash

            ❯ alt info mission jason
             Missions list and products available in the CTOH database:
            -----------------------------------------------------------------------------------------------------------------
                | Mission Name | Product Name              | Product version         | Orbit | GDR/SGDR/SIGDR and CTOH addons
                |              |                           |                         | phase |
            -----------------------------------------------------------------------------------------------------------------
              1 | Jason-1      | ja1_a_cnes_e_gdr          | GDR-E, GDR              | A     | ['gdr', 'normpass', 'ctoh1', 'ctoh2', 'gpd', 'fes14', 'l2p']
              2 | Jason-1      | ja1_b_cnes_e_gdr          | GDR-E, GDR              | B     | ['gdr', 'ctoh2', 'normpass', 'gpd']
              3 | Jason-1      | ja1_c_cnes_e_gdr          | GDR_E, GDR              | C     | ['gdr', 'ctoh2']
              4 | Jason-2      | ja2_a_cnes_d_gdr          | GDR-D, GDR              | A     | ['gdr', 'ctoh1', 'ctoh2', 'ctoh_hr', 'gpd', 'normpass']
              5 | Jason-2      | ja2_b_cnes_d_gdr          | GDR-D, GDR              | B     | ['gdr', 'ctoh2', 'gpd']
              6 | Jason-3      | ja3_a_cnes_%_sgdr         | GDR-T, GDR-D, SGDR      | A     | ['gdr', 'ctoh2', 'normpass', 'gpd', 'ales', 'rads']
              7 | Jason-3      | ja3_a_cnes_%_sigdr        | GDR-T, GDR-D, SIGDR     | A     | ['gdr', 'ctoh2']
              8 | Jason-3      | ja3_a_cnes_f_sgdr         | GDR-F, SGDR             | A     | ['gdr']

            For more details about the mission and its products : alt -v info mission <mission_name>
                 or even more details: alt -vv info mission <mission_name>
            For all mission : alt info mission ?

            The products documentations are available : https://groupes.renater.fr/wiki/ctoh_news
         
    

        *  Et enfin, toute la liste des missions et des produits

        .. code-block:: bash

            ❯ alt info mission ?
             Missions list and products available in the CTOH database:
            -----------------------------------------------------------------------------------------------------------------
                | Mission Name | Product Name              | Product version         | Orbit | GDR/SGDR/SIGDR and CTOH addons
                |              |                           |                         | phase |
            -----------------------------------------------------------------------------------------------------------------
              1 | Cryosat-2    | cs2_b_gop_c_gdr           | GOP, Baseline C, GDR    | B     | ['gdr']
              2 | Cryosat-2    | cs2_b_ice_d_gdr           | ICE, Baseline D, GDR    | B     | ['gdr']
              3 | Envisat      | env_a_ctoh_v0210_gdr      | v2.1, GDR               | A     | ['gdr', 'ctoh1', 'ctoh2', 'ctoh_hf', 'ctoh_hf_corr', 'normpass', 'gpd', 'fes14', 'gpdv2']
              4 | Envisat      | env_a_esa_v0300_sgdr      | v3.0, SGDR              | A     | ['sgdr', 'ctoh2']
              5 | Envisat      | env_b_ctoh_v0210_gdr      | v2.1, GDR               | B     | ['gdr', 'ctoh1', 'ctoh2', 'ctoh_hf']
              6 | Envisat      | env_b_esa_v0300_sgdr      | v3.0, SGDR              | B     | ['sgdr', 'ctoh2']
              7 | ERS-2        | ers2_a_ctoh_v0100_gdr     | v1.0, GDR               | A     | ['gdr', 'ctoh1', 'ctoh2', 'ctoh3', 'ctoh_hr', 'ers2.r', 'normpass']
              8 | ERS-2        | ers2_a_reaper_v0108_gdr   | v1.08, GDR              | A     | ['gdr', 'gpd', 'ctoh2', 'normpass']
              9 | ERS-2        | ers2_a_reaper_v0108_sgdr  | v1.08, SGDR             | A     | ['gdr']
             10 | Jason-1      | ja1_a_cnes_e_gdr          | GDR-E, GDR              | A     | ['gdr', 'normpass', 'ctoh1', 'ctoh2', 'gpd', 'fes14', 'l2p']
             11 | Jason-1      | ja1_b_cnes_e_gdr          | GDR-E, GDR              | B     | ['gdr', 'ctoh2', 'normpass', 'gpd']
             12 | Jason-1      | ja1_c_cnes_e_gdr          | GDR_E, GDR              | C     | ['gdr', 'ctoh2']
             13 | Jason-2      | ja2_a_cnes_d_gdr          | GDR-D, GDR              | A     | ['gdr', 'ctoh1', 'ctoh2', 'ctoh_hr', 'gpd', 'normpass']
             14 | Jason-2      | ja2_b_cnes_d_gdr          | GDR-D, GDR              | B     | ['gdr', 'ctoh2', 'gpd']
             15 | Jason-3      | ja3_a_cnes_%_sgdr         | GDR-T, GDR-D, SGDR      | A     | ['gdr', 'ctoh2', 'normpass', 'gpd', 'ales', 'rads']
             16 | Jason-3      | ja3_a_cnes_%_sigdr        | GDR-T, GDR-D, SIGDR     | A     | ['gdr', 'ctoh2']
             17 | Jason-3      | ja3_a_cnes_f_sgdr         | GDR-F, SGDR             | A     | ['gdr']
             18 | Sentinel-3A  | s3a_a_lan_%_sgdr          | Baseline D, LAND        | A     | ['.sgdr', 'sgdr', 'ctoh2']
             19 | Sentinel-3A  | s3a_a_wat_%_sgdr          | Baseline D, WATER       | A     | ['.sgdr', 'sgdr', 'ctoh2']
             20 | Sentinel-3B  | s3b_a_lan_%_sgdr          | Baseline D, LAND        | B     | ['.sgdr', 'sgdr', 'ctoh2']
             21 | Sentinel-3B  | s3b_a_wat_%_sgdr          | Baseline D, WATER       | A     | ['.sgdr', 'sgdr', 'ctoh2']
             22 | Sentinel-3B  | s3b_b_lan_%_sgdr          | Baseline D, LAND        | B     | ['.sgdr', 'sgdr', 'ctoh2']
             23 | Sentinel-3B  | s3b_b_wat_%_sgdr          | Baseline D, WATER, SGDR | B     | ['.sgdr', 'sgdr', 'ctoh2']
             24 | Sentinel-6A  | s6a_a_hr_%_gdr            | F01, F02, (SAR)         | A     | ['gdr']
             25 | Sentinel-6A  | s6a_a_hr_%_igdr           | F01, F02, (SAR, IGDR)   | A     | ['igdr']
             26 | Sentinel-6A  | s6a_a_lr_%_gdr            | F01, F02, (LRM)         | A     | ['gdr']
             27 | Sentinel-6A  | s6a_a_lr_%_igdr           | F01, F02, (LRM, IGDR)   | A     | ['igdr']
             28 | Saral        | srl_a_cnes_%_sgdr         | GDR-E, GDR-F, SGDR      | A     | ['gdr', 'ctoh1', 'ctoh2', 'fes14', 'normpass', 'gpd', 'gpdv2']
             29 | Saral        | srl_b_cnes_%_sgdr         | GDR-D, GDR-F, SGDR      | B     | ['gdr', 'ctoh2']
             30 | Saral        | srl_b_cnes_%_sigdr        | GDR-D, GDR-F, SIGDR     | B     | ['gdr', 'ctoh2']
             31 | Topex        | tpx_a_cash_v0100_gdr      | v1.0, GDR               | A     | ['gdr']

            For more details about the mission and its products : alt -v info mission <mission_name>
                 or even more details: alt -vv info mission <mission_name>
            For all mission : alt info mission ?

            The products documentations are available : https://groupes.renater.fr/wiki/ctoh_news        
            


.. _alt_info_product_ref:


    - **product** : renvoi les informations sur les produits disponible en base

        * Toute la liste des produits

        .. code-block:: bash

            ❯ alt info product
             Products list available in the CTOH database:
            -----------------------------------------------------------------
                | Prod. Name                |     Cycle     |     Date      |
                |                           | start |   end | start |   end |
            -----------------------------------------------------------------
              1 | cs2_b_gop_c_gdr           |     1 |    11 |
              2 | cs2_b_ice_d_gdr           |     2 |    10 |
              3 | env_a_ctoh_v0210_gdr      |     6 |    94 |
              4 | env_a_esa_v0300_sgdr      |     6 |    94 |
              5 | env_b_ctoh_v0210_gdr      |    95 |   113 |
              6 | env_b_esa_v0300_sgdr      |    95 |   113 |
              7 | ers2_a_ctoh_v0100_gdr     |     1 |    90 |
              8 | ers2_a_reaper_v0108_gdr   |     1 |    85 |
              9 | ers2_a_reaper_v0108_sgdr  |     1 |    85 |
             10 | ja1_a_cnes_e_gdr          |     1 |   259 |
             11 | ja1_b_cnes_e_gdr          |   262 |   374 |
             12 | ja1_c_cnes_e_gdr          |   500 |   537 |
             13 | ja2_a_cnes_d_gdr          |     0 |   303 |
             14 | ja2_b_cnes_d_gdr          |   305 |   327 |
             15 | ja3_a_cnes_%_sgdr         |     0 |   170 |
             16 | ja3_a_cnes_%_sigdr        |   166 |   202 |
             17 | ja3_a_cnes_f_sgdr         |     0 |   196 |
             18 | s3a_a_lan_%_sgdr          |     1 |    74 |
             19 | s3a_a_wat_%_sgdr          |     1 |    74 |
             20 | s3b_a_lan_%_sgdr          |     3 |    19 |
             21 | s3b_a_wat_%_sgdr          |     2 |    19 |
             22 | s3b_b_lan_%_sgdr          |    19 |    54 |
             23 | s3b_b_wat_%_sgdr          |    19 |    54 |
             24 | s6a_a_hr_%_gdr            |    19 |    21 |
             25 | s6a_a_hr_%_igdr           |    23 |    26 |
             26 | s6a_a_lr_%_gdr            |    19 |    21 |
             27 | s6a_a_lr_%_igdr           |    23 |    26 |
             28 | srl_a_cnes_%_sgdr         |     1 |    35 |
             29 | srl_b_cnes_%_sgdr         |   100 |   150 |
             30 | srl_b_cnes_%_sigdr        |   135 |   150 |
             31 | tpx_a_cash_v0100_gdr      |    10 |   359 |

            For more details about the products : alt -v info  product <product_name>
            For all mission : alt info product

            The products documentations are available : https://groupes.renater.fr/wiki/ctoh_news
      


        * Avec plus de détailles : option ``-v``

        .. code-block:: bash
            
            ❯ alt -v info product
             Products list available in the CTOH database:
            ------------------------------------------------------------------------------------------------------------------
                | Prod. Name                | Mission     | Orbit | Supp./ | Prod.   | Prod. |     Cycle     |     Date      |
                |                           |             | Phase | Proc.  | version | type  | start |   end | start |   end |
            ------------------------------------------------------------------------------------------------------------------
              1 | cs2_b_gop_c_gdr           | Cryosat-2   | b     | gop    | c       | gdr   |     1 |    11 |
              2 | cs2_b_ice_d_gdr           | Cryosat-2   | b     | ice    | d       | gdr   |     2 |    10 |
              3 | env_a_ctoh_v0210_gdr      | Envisat     | a     | ctoh   | v0210   | gdr   |     6 |    94 |
              4 | env_a_esa_v0300_sgdr      | Envisat     | a     | esa    | v0300   | sgdr  |     6 |    94 |
              5 | env_b_ctoh_v0210_gdr      | Envisat     | b     | ctoh   | v0210   | gdr   |    95 |   113 |
              6 | env_b_esa_v0300_sgdr      | Envisat     | b     | esa    | v0300   | sgdr  |    95 |   113 |
              7 | ers2_a_ctoh_v0100_gdr     | ERS-2       | a     | ctoh   | v0100   | gdr   |     1 |    90 |
              8 | ers2_a_reaper_v0108_gdr   | ERS-2       | a     | reaper | v0108   | gdr   |     1 |    85 |
              9 | ers2_a_reaper_v0108_sgdr  | ERS-2       | a     | reaper | v0108   | sgdr  |     1 |    85 |
             10 | ja1_a_cnes_e_gdr          | Jason-1     | a     | cnes   | e       | gdr   |     1 |   259 |
             11 | ja1_b_cnes_e_gdr          | Jason-1     | b     | cnes   | e       | gdr   |   262 |   374 |
             12 | ja1_c_cnes_e_gdr          | Jason-1     | c     | cnes   | e       | gdr   |   500 |   537 |
             13 | ja2_a_cnes_d_gdr          | Jason-2     | a     | cnes   | d       | gdr   |     0 |   303 |
             14 | ja2_b_cnes_d_gdr          | Jason-2     | b     | cnes   | d       | gdr   |   305 |   327 |
             15 | ja3_a_cnes_%_sgdr         | Jason-3     | a     | cnes   | ---     | sgdr  |     0 |   170 |
             16 | ja3_a_cnes_%_sigdr        | Jason-3     | a     | cnes   | ---     | sigdr |   166 |   202 |
             17 | ja3_a_cnes_f_sgdr         | Jason-3     | a     | cnes   | f       | sgdr  |     0 |   196 |
             18 | s3a_a_lan_%_sgdr          | Sentinel-3A | a     | lan    | ---     | sgdr  |     1 |    74 |
             19 | s3a_a_wat_%_sgdr          | Sentinel-3A | a     | wat    | ---     | sgdr  |     1 |    74 |
             20 | s3b_a_lan_%_sgdr          | Sentinel-3B | a     | lan    | ---     | sgdr  |     3 |    19 |
             21 | s3b_a_wat_%_sgdr          | Sentinel-3B | a     | wat    | ---     | sgdr  |     2 |    19 |
             22 | s3b_b_lan_%_sgdr          | Sentinel-3B | b     | lan    | ---     | sgdr  |    19 |    54 |
             23 | s3b_b_wat_%_sgdr          | Sentinel-3B | b     | wat    | ---     | sgdr  |    19 |    54 |
             24 | s6a_a_hr_%_gdr            | Sentinel-6A | a     | hr     | ---     | gdr   |    19 |    21 |
             25 | s6a_a_hr_%_igdr           | Sentinel-6A | a     | hr     | ---     | igdr  |    23 |    26 |
             26 | s6a_a_lr_%_gdr            | Sentinel-6A | a     | lr     | ---     | gdr   |    19 |    21 |
             27 | s6a_a_lr_%_igdr           | Sentinel-6A | a     | lr     | ---     | igdr  |    23 |    26 |
             28 | srl_a_cnes_%_sgdr         | Saral       | a     | cnes   | ---     | sgdr  |     1 |    35 |
             29 | srl_b_cnes_%_sgdr         | Saral       | b     | cnes   | ---     | sgdr  |   100 |   150 |
             30 | srl_b_cnes_%_sigdr        | Saral       | b     | cnes   | ---     | sigdr |   135 |   150 |
             31 | tpx_a_cash_v0100_gdr      | Topex       | a     | cash   | v0100   | gdr   |    10 |   359 |
            The products documentations are available : https://groupes.renater.fr/wiki/ctoh_news
         


        * Seulement avec les produits ``ja1``

        .. code-block:: bash

            ❯ alt -v info product ja1
             Products list available in the CTOH database:
            ------------------------------------------------------------------------------------------------------------------
                | Prod. Name                | Mission     | Orbit | Supp./ | Prod.   | Prod. |     Cycle     |     Date      |
                |                           |             | Phase | Proc.  | version | type  | start |   end | start |   end |
            ------------------------------------------------------------------------------------------------------------------
              1 | ja1_a_cnes_e_gdr          | Jason-1     | a     | cnes   | e       | gdr   |     1 |   259 |
              2 | ja1_b_cnes_e_gdr          | Jason-1     | b     | cnes   | e       | gdr   |   262 |   374 |
              3 | ja1_c_cnes_e_gdr          | Jason-1     | c     | cnes   | e       | gdr   |   500 |   537 |
            The products documentations are available : https://groupes.renater.fr/wiki/ctoh_news
         


.. _alt_info_product_param_ref:


        * Recherche d'information sur des paramètres d'un produit. Exemple du produit ``ja1_a_cnes_e_gdr`` et les paramètres de ``range``. La recherche des paramètres se fait avec l'option ``-p`` et la chaine de caractère ``*range*``:

        .. code-block:: bash

            ❯ alt info product ja1_a_cnes_e_gdr -p*range*
            ################################################################################
                 Number of occurences found for '*range*' :
                    - Product Attributes  : 0
                    - Product Variables : 19
                    - Product Description : 0

            ################################################################################
            -------------------------------Variables product--------------------------------
            ['gdr',
             ['ice_range_20hz_c',
              'ice_range_20hz_ku',
              'net_instr_corr_range_c',
              'net_instr_corr_range_ku',
              'qual_alt_1hz_range_c',
              'qual_alt_1hz_range_ku',
              'qual_inst_corr_1hz_range_c',
              'qual_inst_corr_1hz_range_ku',
              'range_20hz_c',
              'range_20hz_ku',
              'range_c',
              'range_ku',
              'range_numval_c',
              'range_numval_ku',
              'range_rms_c',
              'range_rms_ku',
              'range_used_20hz_c',
              'range_used_20hz_ku']]
            ################################################################################
      

        * Pour plus de détailles sur le paramètre ``range_20hz_ku`` du produit ``ja1_a_cnes_e_gdr`` : option ``-v``

        .. code-block:: bash

            ❯ alt -v info product ja1_a_cnes_e_gdr -prange_20hz_ku
            ################################################################################
                 Number of occurences found for 'range_20hz_ku' :
                    - Product Attributes  : 0
                    - Product Variables : 1
                    - Product Description : 0

            ################################################################################
            -------------------------------Variables product--------------------------------
            ['gdr',
             [['range_20hz_ku',
               {'_FillValue': '2147483647',
                'add_offset': '1300000.0',
                'comment': 'All instrumental corrections included, i.e. distance '
                           'antenna-COG (cog_corr), USO drift correction (uso_corr), '
                           'internal path correction (internal_path_delay_corr_ku), '
                           'Doppler correction (doppler_corr_ku), modeled instrumental '
                           'errors correction (modeled_instr_corr_range_ku) and system '
                           'bias. range_20hz updated to account for reference plane and '
                           'internal path delay ',
                'coordinates': 'lon_20hz lat_20hz',
                'long_name': '20 Hz Ku band corrected altimeter range',
                'scale_factor': '0.0001',
                'standard_name': 'altimeter_range',
                'units': 'm'}]]]
            ################################################################################
     



Tracks
~~~~~~

Cette fonctionalité permet trouver pour chaqu'un des produits les numéros de trace dans une région donnée.
Elle est accessible à l'aide de la sous-commande ``tracks`` de la commande ``alt``.
Ci dessous, l'affichage de l'aide (option -h) pour la sous-commande ``tracks``:

.. code-block:: bash

    ❯ alt tracks -h
    usage: alt tracks [-h] product_name {region,nearest} ...

    positional arguments:
      product_name      product referred to

    optional arguments:
      -h, --help        show this help message and exit

    product:
      product whose tracks are queried

      {region,nearest}
        region          Tracks crossing the given rectangular region
        nearest         Nearest track to given point


Deux possibilitées pour la détermination des numéros de trace : 

    - **region** : renvoi les numéros de trace d'un produit passant dans une région qui est définie par des coordonnées géographique min et max d'un rectangle.

        * Numéros de trace de la mission Jason-1 à l'interieure du rectangle lon/lat min/max:

        .. code-block:: bash

            ❯ alt tracks ja1_a_cnes_e_gdr region -G1.55,1.75  -L42.0,44.0
            tracks region ja1_a_cnes_e_gdr
            #### Jason-1 topex 1.55 1.75 42.0 44.0
            /ctoh/data/shared/tracks topex
            >>>>>>> path :  /ctoh/data/shared/tracks/topex
            2 tracks crossing the region : {70, 111}
     


        * Avec plus de détailles : option ``-v``

        .. code-block:: bash

            ❯ alt -v tracks ja1_a_cnes_e_gdr region -G1.55,1.75  -L42.0,44.0
            tracks region ja1_a_cnes_e_gdr
            #### Jason-1 topex 1.55 1.75 42.0 44.0
            /ctoh/data/shared/tracks topex
            >>>>>>> path :  /ctoh/data/shared/tracks/topex
            Segment 1 intersects track 111 at (lat,lon)=(43.372822,1.735552)
            Segment 1 intersects track 70 at (lat,lon)=(42.547038,-358.235236)
            Segment 3 intersects track 111 at (lat,lon)=(43.125436,1.532267)
            Segment 3 intersects track 70 at (lat,lon)=(42.794425,-358.433343)
            2 tracks crossing the region : {70, 111}

    - **nearest** : renvoi le numéro de trace d'un produit passant le plus proche d'un point définit par ces coordonnées géographiques.

        * Quelle est la trace du produit Jason-1 (ja1_a_cnes_e_gdr) passant la plus près de Toulouse (1.439706 43.607888)?

        .. code-block:: bash

            ❯ alt -v tracks ja1_a_cnes_e_gdr nearest 1.439706 43.607888
            tracks nearest ja1_a_cnes_e_gdr
            /ctoh/data/shared/tracks topex
            >>>>>>> path :  /ctoh/data/shared/tracks/topex
            The nearest track to point lat=43.607888, lon=1.439706 is 111
     


        * Quelle est la trace **ascendante** du produit Jason-1 (ja1_a_cnes_e_gdr) passant la plus près de Toulouse (1.439706 43.607888)?

        .. code-block:: bash

            ❯ alt tracks ja1_a_cnes_e_gdr nearest 1.439706 43.607888 --asc
            tracks nearest ja1_a_cnes_e_gdr
            /ctoh/data/shared/tracks topex
            >>>>>>> path :  /ctoh/data/shared/tracks/topex
            111
     


     
        * Quelle est la trace **descendante** du produit Jason-1 (ja1_a_cnes_e_gdr) passant la plus près de Toulouse (1.439706 43.607888)?

        .. code-block:: bash

            ❯ alt tracks ja1_a_cnes_e_gdr nearest 1.439706 43.607888 --dsc
            tracks nearest ja1_a_cnes_e_gdr
            /ctoh/data/shared/tracks topex
            >>>>>>> path :  /ctoh/data/shared/tracks/topex
            70


.. _extract_ref:

Extract
~~~~~~~

Cette fonctionalité permet extraire des paramètres altimétriques de la base de données.
Elle est accessible à l'aide de la sous-commande ``extract`` de la commande ``alt``.
Ci dessous, l'affichage de l'aide (option -h) pour la sous-commande ``extract``:

.. code-block:: bash

    ❯ alt extract -h
    usage: alt extract [-h] [-t TRACKS] [-c CYCLES] [--cfg_file CFG_FILE]
                       [-p PARAMS] [-o OUTPUT_DIR] [--prefix PREFIX] [--serial]
                       [--no_groups]
                       product {region,shapefile} ...

    positional arguments:
      product               extract product

    optional arguments:
      -h, --help            show this help message and exit
      -t TRACKS, --tracks TRACKS
                            Tracks list
      -c CYCLES, --cycles CYCLES
                            Cycles list
      --cfg_file CFG_FILE   Configuration file (yaml) with parameters list (tag
                            label : params)
      -p PARAMS, --params PARAMS
                            Parameters list
      -o OUTPUT_DIR, --output_dir OUTPUT_DIR
                            Output directory
      --prefix PREFIX       Prefix file
      --serial              Switch off multiprocess extraction.
      --no_groups           Force NetCDF4 without groups.

    Area:
      Area whose tracks are queried

      {region,shapefile}
        region              Tracks crossing the given rectangular region
        shapefile           Tracks crossing the given shapefile polygon


L'extraction de paramètres altimétriques de la base de données requière un minimum d'arguments qui peuvent varier en fonction des besions. Ici ci-dessous, voici quelques exemples d'usage...

    - Exemple n°1 : 
        * Extraction du produit ``ja1_a_cnes_e_gdr`` dans la région toulousainne (region -G1.55,1.75  -L42.0,44.0)
        * L'option paramètre ``-p`` indique la liste de paramètres à extraire : lon,lat,time,range_20hz_ku
        * L'option cycle ``-c`` indique la liste des cycles à extraire : 50,60-63 (le cycle 50 et les cycles de 60 à 63)
        * L'option trace ``-t`` n'est ici pas necessaire car la selection par coordonnées géographiques calcul les numéros de traces à selectionner.
        * **L'option répertoire ``-o`` est ici omit. Les fichiers d'extraction sont généré dans le répertoire courant.**
     


    .. code-block:: bash

        ❯ alt extract ja1_a_cnes_e_gdr -plon,lat,time,range_20hz_ku -c50,60-63 region -G1.55,1.75  -L42.0,44.0
        extract ja1_a_cnes_e_gdr
        coord_extract {'lonmin': 1.55, 'lonmax': 1.75, 'latmin': 42.0, 'latmax': 44.0}
        Selection cycle list is : [50, 60, 61, 62, 63]
        Getting metadata...
        Getting mission information...
        Getting tracks to extract...
        mission: topex
        /ctoh/data/shared/tracks topex
        >>>>>>> path :  /ctoh/data/shared/tracks/topex

        topex
        nb tracks per cycle: 254
        nb turns per cycle : 10
        orbit inclination  : 66.04°

        Getting product file...
        Found 10 files
        Starting extraction
        Writing file : /data/tmp7j/Test/JA1_GPN_2PeP050_070_20030518_183730_20030518_193339.nc
        Writing file : /data/tmp7j/Test/JA1_GPN_2PeP061_070_20030904_202117_20030904_211726.nc
        Writing file : /data/tmp7j/Test/JA1_GPN_2PeP050_111_20030520_090218_20030520_095826.nc
        Writing file : /data/tmp7j/Test/JA1_GPN_2PeP063_070_20030924_161819_20030924_171428.nc
        Writing file : /data/tmp7j/Test/JA1_GPN_2PeP060_070_20030825_222246_20030825_231855.nc
        Writing file : /data/tmp7j/Test/JA1_GPN_2PeP061_111_20030906_104605_20030906_114214.nc
        Writing file : /data/tmp7j/Test/JA1_GPN_2PeP065_070_20031014_121521_20031014_131131.nc
        Writing file : /data/tmp7j/Test/JA1_GPN_2PeP063_111_20030926_064306_20030926_073909.nc
        Writing file : /data/tmp7j/Test/JA1_GPN_2PeP062_070_20030914_181948_20030914_191557.nc
        Writing file : /data/tmp7j/Test/JA1_GPN_2PeP060_111_20030827_124733_20030827_134340.nc
        Writing file : /data/tmp7j/Test/JA1_GPN_2PeP065_111_20031016_024010_20031016_033614.nc
        Writing file : /data/tmp7j/Test/JA1_GPN_2PeP064_070_20031004_141650_20031004_151259.nc
        Writing file : /data/tmp7j/Test/JA1_GPN_2PeP062_111_20030916_084436_20030916_094045.nc
        Writing file : /data/tmp7j/Test/JA1_GPN_2PeP066_070_20031024_101355_20031024_111004.nc
        fichier vide
        Writing file : /data/tmp7j/Test/JA1_GPN_2PeP066_111_20031026_003842_20031026_013444.nc
        Writing file : /data/tmp7j/Test/JA1_GPN_2PeP064_111_20031006_044137_20031006_053742.nc
        Writing file : /data/tmp7j/Test/JA1_GPN_2PeP067_070_20031103_081226_20031103_090837.nc
        Ending extraction
        Done processing
        Empty files removed.


    Avec les fichiers d'extraction, un fichier de description (files_desc.yml) est également généré. Il décrit le contenu de l'extraction :

    .. code-block:: bash

        ❯ more files_desc.yml
        coordinates:
          latmax: 44.0
          latmin: 42.0
          lonmax: 1.75
          lonmin: 1.55
        cycles: '[50, 60, 61, 62, 63]'
        date: Thu Aug  5 17:11:19 2021
        filenames:
          50:
            70: JA1_GPN_2PeP050_070_20030518_183730_20030518_193339.nc
            111: JA1_GPN_2PeP050_111_20030520_090218_20030520_095826.nc
          60:
            70: JA1_GPN_2PeP060_070_20030825_222246_20030825_231855.nc
            111: JA1_GPN_2PeP060_111_20030827_124733_20030827_134340.nc
          61:
            70: JA1_GPN_2PeP061_070_20030904_202117_20030904_211726.nc
            111: JA1_GPN_2PeP061_111_20030906_104605_20030906_114214.nc
          62:
            70: JA1_GPN_2PeP062_070_20030914_181948_20030914_191557.nc
            111: JA1_GPN_2PeP062_111_20030916_084436_20030916_094045.nc
          63:
            70: JA1_GPN_2PeP063_070_20030924_161819_20030924_171428.nc
            111: JA1_GPN_2PeP063_111_20030926_064306_20030926_073909.nc
        files_number: 10
        institution: CTOH/LEGOS, UMR5566 CNRS-CNES-IRD-Universite de Toulouse III
        mission: Jason-1
        orbit: topex
        output_directory: /data/tmp7j/Test/
        product_name: ja1_a_cnes_e_gdr
        pyctoh_version: hg698a2d3c4442
        shapefile: null
        source: altimetry radar
        title: GDR or SGDR dataset supplied from CTOH database


    - Exemple n°2 : 
        * Extraction  du produit ``ja1_a_cnes_e_gdr``
        * L'option paramètre ``-p`` indique la liste de paramètres à extraire : lon,lat,time,range_20hz_ku
        * **L'option cycle ``-c`` est omit. Tous les cycles sont extrait.**
        * L'option trace ``-t`` indique les numéros de trace à extraire : 100-105,180.
        * L'option répertoire ``-o`` est ici omit. Les fichiers d'extraction sont générer dans le répertoire courant.


    .. code-block:: bash

        ❯ alt extract ja1_a_cnes_e_gdr -plon,lat,time,range_20hz_ku -t100-105,180
    

    - Exemple n°3 : 
        * Extraction  du produit ``ja1_a_cnes_e_gdr``
        * L'option paramètre ``-p`` indique la liste de paramètres à extraire : lon,lat,time,range_20hz_ku
        * **L'option cycle ``-c`` est omit. Tous les cycles sont extrait.**
        * L'option trace ``-t`` indique les numéros de trace à extraire : 100-105,180.
        * L'option répertoire ``-o`` est ici omit. Les fichiers d'extraction sont générer dans le répertoire courant.


    .. code-block:: bash

        ❯ alt extract ja1_a_cnes_e_gdr -plon,lat,time,range_20hz_ku -t100-105,180
 

    - Exemple n°4 : 
        * Extraction  du produit ``ja1_a_cnes_e_gdr``
        * L'option paramètre ``-p`` indique la liste de paramètres à extraire : lon,lat,time, **l2p:range** ,wet_tropospheric_correction_model
        
            La recherche du paramètre se fait de manière prioritaire dans le produit de base (càd gdr, sgdr, sigdr) puis si le paramètre n'est pas trouvé, la racherche s'entend aux autres sous-produits (ctoh1, ctoh2, gdp, etc). Dans cet exemple les paramètres ``lon``, ``lat``, ``time`` et ``wet_tropospheric_correction_model`` sont recherché dans le produits de base GDR du produit ``ja1_a_cnes_e_gdr``. Le paramètre ``wet_tropospheric_correction_model`` n'appartient pas au produit de base. Il est donc recherché dans les autres sous-produits. Le paramètre de ``l2p:range`` est force la selection du paramètre ``range`` dans le sous-produit ``l2p``.

        * L'option cycle ``-c`` indique les cycles de 1 à 10 
        * L'option trace ``-t`` indique les numéros de trace à extraire : 100-105,180.
        * L'option répertoire ``-o`` est ici omit. Les fichiers d'extraction sont générer dans le répertoire courant.


    .. code-block:: bash

        ❯ alt extract ja1_a_cnes_e_gdr -plon,lat,time,l2p:range,wet_tropospheric_correction_model -t109 -c1-10


    - Exemple n°5 : 
        * Extraction  du produit ``ja1_a_cnes_e_gdr``
        * L'option paramètre ``-p`` indique la liste de paramètres à extraire : lon,lat,time, **l2p:/*** 
            L'expression du paramètre ``l2p:*`` permet l'extraction de tous les paramètre du sous-produit l2p

        * L'option cycle ``-c`` indique les cycles de 1 à 10
        * L'option trace ``-t`` indique les numéros de trace à extraire : 100-105,180.
        * L'option répertoire ``-o`` est ici omit. Les fichiers d'extraction sont générer dans le répertoire courant.


    .. code-block:: bash
    
        ❯ alt extract ja1_a_cnes_e_gdr -plon,lat,time,l2p:* -t109 -c1-10
    




Accès via l’API python
-----------------------

* L'API PyCTOH existe. Doc a terminer.*


Catalogues de données
=======================

Remise d’équerre des catalogues. Aujourd’hui l’implémentation du parser des fichiers de description et trop complexe car la grammaire des fichiers desc n’existe pas (à ma connaissance) et il y a beaucoup d’exceptions à prendre en compte (le cas de chemins terminant par ‘/’ ou pas, quand il y a un gdrpath défini ou pas…).  Pour ne pas modifier les fichiers existants et risquer de tout casser, on a définit une grammaire standard simple, implémenté sous forme de fichiers YAML.  

Aussi, plutôt que permettre d’avoir des catalogues n’importe où dans l’arborescence qui se font référence les uns aux autres avec des liens symboliques, on demande à ce qu’ils soient tous ensemble dans un seul et même répertoire, ça favorise la lisibilité.

On limite le nombre de catalogues à 2 :  un catalogue général pour tout le monde et un autre pour le ctoh, qui contient tous les produits en phase beta.  Chaque utilisateur (ou groupe) peut en plus avoir son propre catalogue, pour les produits en plein développement (phase alpha).

Services
=========

Les services à proposer à partir de la base sont :

-	extraction : le service essentiel. 
-	plot : une visualisation rapide du contenu d’un sous-enesemble de la base

la base doit être stable et robuste.


Droits d’accès à la base
=========================

Aujourd’hui les droits d’accès sont calqués sur la distinction de propriété unix.  Cette distinction est fictive et en réalité, tous les utilisateurs Legos devraient pouvoir avoir accès en lecture aux produits qu’on veut leur proposer.  La distinction usrcto/ctoh pour les données est donc fictive et doit pouvoir disparaître.  

On est en train d’écrire une commande d’extraction qui ne accède pas directement aux fichiers, mais via un daemon d’extraction qui, lui, a les droits d’accès usrcto et ctoh.  Ainsi, tous les produits ctoh sont disponibles à tous les utilisateurs sans avoir besoin de dupliquer les fichiers.  Seule contrainte : ça marche via des extractions.  L’accès direct à l’arborescence n’est pas possible pour un utilisateur lambda – il faut être ctoh. 


