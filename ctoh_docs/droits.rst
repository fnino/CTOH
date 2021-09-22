Droits et ACL -  2021-01-18
============================

Droits sur /ctoh/data
---------------------

ctoh/data  appartient à ctoh. Pour que usrcto ait le droit d'y accéder
il faut ajouter un deuxième groupe.  chmod ne suffit pas:

    cd /ctoh/data
    # 2801 est le gid de usrcto
    nfs4_setfacl -a A:g:2801:rxtcy .

Ca ajoute ("a") un droit d'accès ("A"llow) pour le "g"roupe 2801 et droit de lecture
de fichiers, d'éxécution et de lecture d'attributs (tcy)

Si on voulait (on ne veut pas) donner droits d'écriture il faudrait ajouter:

    w: pour écrier
    a: pour ajouter
    D: poureffacer
    T: pour changer les attributs
    C: pour changer les ACL

comme on le voit quand on fait  getfacl:

    ❯ nfs4_getfacl /ctoh/data
    A::OWNER@:rwaDxtTcCy
    A::GROUP@:rxtcy
    A:g:2801:rxtcy
    A::EVERYONE@:tcy

Droits sur /ctoh/data/shared
----------------------------

Répértoire shared, doit être lisible par tout le monde:

    chmod -R a+rX /ctoh/data/shared
