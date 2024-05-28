layout: page
title: "Pourquoi un gestionnaire de mot de passe ?"
permalink: /{{ title | slugify }}
collection: articles
---

Une explication non technique et aussi courte que possible sur les risques de hacking d'email pour justifier le recours à un gestionnaire de mots de passe au quotidien. Les gestionnaires sont recommandés par le gouvernements et sont par exemple expliqués [dans cet article de FranceNum](https://www.francenum.gouv.fr/guides-et-conseils/protection-contre-les-risques/cybersecurite/pourquoi-et-comment-utiliser-un)
```table-of-contents
```

Je précise que de nombreuses barrières techniques existent pour tempérer les risques de hacking ... mais il peut suffire d'une seule faille pour que toutes les sécurités tombent.
# Contexte

## Du point de vue de l'utilisateur
Tout le monde possède généralement :
- un compte email associé à une adresse `email`
- une multitude de comptes auprès de fournisseurs de services "FS" (banque, municipalité, musique en ligne, réseau sociaux, ...). Le compte email est un également un FS.

Dans la majorité des cas (~99% pour moi), l'inscription à un FS se fait en renseignant `email` et un mot de passe `password1`.

À titre indicatif, j'ai ouvert des comptes chez 550 FS. Même ceux qui je n'utilise plus sont important pour la suite.
## Du point de vue du fournisseur de service

Lorsqu'un utilisateur François veut se connecter à un FS, il lui indique `email` et son mot de passe `password`. Comment le FS peut-il vérifier que cette tentative de connexion est légitime ? Une méthode naïve est encore **trop répandue** chez les FS et consiste à :
- lors de l'inscription de l'utilisateur, ajouter à la base de données des utilisateur (BDU) une info type "François s'authentifie via un email `email` et un mot de passe `password1`"
- lors d'une tentative de connexion via `email` et `password1`, le FS va simplement vérifier que `password1` est bien lié à `email` dans la BDU

Il existe des méthodes bien moins naïves garantissant une sécurité bien meilleure mais elles ne sont malheureusement pas généralisées et je ne rentre pas dans leurs détails ici. 

**Comme montré plus loin, un seul FS utilisant la méthode naïve peut rendre son identité numérique vulnérable.**

# Scénario sans outil pour gérer ses mots de passe

## Le système D utilisé par beaucoup 
Sans outil particuliers, comment gérer 550 comptes différents, et donc 550 mots de passes pour chacun des 550 FS ?
La plupart des gens utilisent le même mot de passe partout : `password`. Voire de jonglent entre 3-4 mots de passes différents `password1`, `password2`, `password3`. 

Ainsi, *chaque* FS a dans sa BDU a une info du style "François s'authentifie via un email `email` et un mot de passe `password`" 
NB : j'exagère en considérant ici que chaque FS a une gestion naïve de son authentification mais nous verrons plus loin qu'il suffit d'un FS mauvais élève.

## Implications du hacking d'un fournisseur de service

Les hackers ont de nombreuses raisons de vouloir attaquer un FS : détournement des ressources de calcul (c'était le cas pendant le boom du BitCoin), rançon ... ou vol de la base de données des utilisateurs.
Pour quelle raison voler la base de données utilisateurs ? 

Si un attaquant met la main sur une base de donnée d'un FS utilisant la méthode naïve d'identification, il a dorénavant accès à la liste des infos de connexion de tous les utilisateurs, dont "François s'authentifie via un email `email` et un mot de passe `password`".

**Problème** : à partir de là, l'attaquant va faire l'hypothèse que François a utilisé le *même mot de passe* `password` chez tous ses FS ; il a ainsi les clefs pour se connecter en toute impunité *à son compte mail*. C'est ce qu'on appelle "hacking de boite mail" par abus de langage (la véritable prouesse technique arrive au moment du vol de BDU chez FS). Voir [pourquoi une boite mail compromise est grave](Pourquoi un gestionnaire de mots de passe ?#Pourquoi une boite mail compromise est grave) pour comprendre la fraude qui résulte d'un hacking de boite mail.

Un site référence toutes les DBU dont le vol est connu et permet de lister les DBU dans lesquelles un email était présent : https://haveibeenpwned.com/ ("Have I been Powned?" = "me suis-je fait avoir ?").
# Scénario avec un gestionnaire de mot de passe

Un gestionnaire de mot de passe permet de générer des mots de passes uniques pour chaque FS, sans devoir les retenir. Ainsi un utilisateur peut maintenir 550 mots de passe totalement différents pour ses 550 FS. Exemple de mots de passe générés : `hfm:iJK\@{,i(jv(5Db)SMD`, `(x[;_].U/6s.t*ui5P)YGAD` ... 
**impossibles à retenir et *ce n'est pas grave* : le gestionnaire s'en occupe !**

En reprenant les [implications du hacking d'un FS](Pourquoi un gestionnaire de mots de passe ?#Implications du hacking d'un fournisseur de service) avec ce nouveau système, on constate que l'attaquant qui vole l'info "François s'authentifie via un email `email` et un mot de passe `hfm:iJK\@{,i(jv(5Db)SMD`" *n'aura rien gagné dans l'affaire* puisque le mot de passe volé ne déverrouille aucun autre service !

# Pourquoi une boite mail compromise est grave

En tant qu'attaquant, posséder `email` et `password1` permet de s'introduire dans la boite mail de François *sans laisser de trace* : le mot de passe n'ayant pas changé, François ne se rend même pas compte qu'une autre personne va et vient dans sa boite mail ! À ma connaissance, 2 scénarios d'attaque non exclusifs sont alors possibles :
## Attaque 1 : arnaque à la maladie
Les véritables cibles sont les contacts de François. Déroulé de l'attaque :
1. Attendrir les cibles : l'attaquant va envoyer un mail (généralement larmoyant) à tous les contacts de François en se faisant passer pour lui. Ce mail est envoyé depuis la véritable adresse mail de François `francois@mail.eu`
2. Isoler les cibles : l'attaquant crée une nouvelle adresse mail qui ressemble à celle de François `francois1@mel.fr` et s'en sert pour solliciter à nouveau les contacts de François afin passer sous les radars de ce dernier. Certains contacts de François ne font pas attention à l'inconsistance de l'email utilisé et pense s'adresser à François
3. Piéger les cibles : via sa fausse adresse, hors du champ de vision de François, l'attaquant va demander de l'argent aux contacts crédules de François en détournant des systèmes type WesternUnion

## Attaque 2 : usurpation d'identité
L'adresse email est un [single point of failure](https://www.wikiwand.com/fr/Point_de_d%C3%A9faillance_unique) : la compromettre, c'est compromettre toute son identité numérique.
La véritable cible est François lui-même. L'attaquant ayant un accès total à la boite mail de François, il peut :
- Changer de mot passe pour bloquer l'accès de François à ses propres mails
- Pénétrer chez les FS de François en réalisant les étapes : 
	1. lister les FS auxquels François a souscrit en les rattachant à son email `email`
	2. aller sur chacun des FS et faire l'opération "j'ai oublié mon mot de passe"
	3. récupérer l'email de réinitialisation du FS en toute simplicité (il a un accès total aux mails de `email`)
	4. changer le mot de passe chez le FS et profiter du service au nom de François

Des attaques ont lieu en 2 temps, la capture d'un FS de François n'étant que la première étape. Exemple avec le compte LeBonCoin de François qui est authentique (le compte a plusieurs années) et sûr (il compte beaucoup de transactions réussies et a une bonne note). Un usurpateur peut alors se servir de ce compte pour démarrer une arnaque en ligne en se faisant passer pour un vendeur réglo.


