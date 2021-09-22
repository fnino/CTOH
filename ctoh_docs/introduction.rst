Introduction
============

La documentation des outils CTOH est centralisé ici, en tant que point d'entrée à
l'inventaire de l'existant.

On utilise *Sphinx* parce que c'est un outil standard fournit avec Python, et aussi pérenne 
que le langage lui-même, qui est utilisé pour la plupart des développements récents.
Le format étant texte, il est indépendant du type de machine utilisée (Windows/Mac/Linux) et
tout le monde peut contribuer, suivre les modifications (c'est sur contrôle de versions) et même
le télécharger chez soi facilement (en faisant un clone mercurial).

Outils de base pour l'utilisation de la documentation

   - une version Python CTOH officielle (selon les specifications Anaconda du projet ctoh_environnement)
   - Une version mercurial récente et compatible avec celle utilisée au Legos (v3.1.2 au 2020/07/02).
   - pour générer du pdf avec `make latexpdf`: une installation de texlive (`module load texlive`)

Ce faisant, vous aurez une distribution Sphinx appropriée, et vous pourrez construire la documentation
en faisant simplement:

.. code-block:: bash

   $ make html      # pour une version web
   Sphinx v3.0.3 en cours d'exécution
   Chargement des traductions [fr]... fait
   chargement de l'environnement pickled... fait
   construction en cours [mo]:cibles pour les fichiers po 0 qui sont périmées
   construction [html]:cibles pour les fichiers sources 0 qui sont périmées
   mise-à-jour de l'environnement :0 ajouté, 0 modifié, 0 supprimé
   recherche des fichiers périmés... aucun résultat
   aucune cible n'est périmée.
   la compilation a réussi.

   Les pages HTML sont dans _build/html .



A noter qu'en faisant cela, les sorties seront créés dans le répertoire `_build` comme indiqué 
à la fin de la commande.

Il est possible aussi de créer des sorties pdf pour lecture en ligne ou impression.

.. code-block:: text

   $ make latexpdf  # pour une version pdf
   Sphinx v3.0.3 en cours d'exécution
   Chargement des traductions [fr]... fait
   création du répertoire de sortie... fait
   chargement de l'environnement pickled... fait
   construction en cours [mo]:cibles pour les fichiers po 0 qui sont périmées
   construction [latex]:all documents
   mise-à-jour de l'environnement :0 ajouté, 0 modifié, 0 supprimé
   recherche des fichiers périmés... aucun résultat
   traitement en cours ctoh.tex... index toto subproj1/index subproj1/toto2
   résolution des références...
   fait
   enregistrement... fait
   copie des fichiers de support TeX... copie des fichiers de support TeX...
   fait
   la compilation a réussi.

   Les fichiers LaTex se trouvent dans _build/latex.
   Exécuter 'make' dans ce répertoire pour les soumettre à (pdf)latex
   (ou 'make latexpdf' directement ici pour l’automatiser).
   latexmk -pdf -dvi- -ps-  'ctoh.tex'
   Latexmk: This is Latexmk, John Collins, 25 October 2018, version: 4.61.
   Latexmk: applying rule 'pdflatex'...<Paste>

   ...

   Latexmk: Index file 'ctoh.idx' was written
   Latexmk: Log file says output to 'ctoh.pdf'
   Latexmk: All targets (ctoh.pdf) are up-to-date


La encore, il y a une ligne qu'indique où se trouve le fichier de sortie (ligne 15 dans cet exemple),
mais il est perdu au milieu de beaucoup de texte. Dans tous les cas, avec sphinx toutes les sorties
sont dans le répertoire `_build/` et un sous-répertoire en fonction de ce qu'on a demandé. Pour 
latexpdf, la sortie sera `_build/latex/ctoh_documentation.pdf`.

Bien que beaucoup d'autres types de sortie sont possibles (tapez simplement `make` pour voir la liste), 
au CTOH on n'utilisera que ces deux là: html et latexpdf.
