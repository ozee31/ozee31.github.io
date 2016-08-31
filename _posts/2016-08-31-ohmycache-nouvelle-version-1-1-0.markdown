---
layout:     post
title:      "[js] OhMyCache - Nouvelle version 1.1.0"
subtitle:   "Une nouvelle version de la librairie OhMyCache viens d'être mise en ligne. Au programme vous aurez accès à 2 nouvelles fonctions keys() et getAll() ainsi que l'ajout d'un nouveau paramètres facultatif à la fonction clear()"
date:       2016-08-31 08:00:00
comments: true
header-img: "img/js.png"
hearder-bg-color: "#323330"
---

# OhMyCache - Nouvelle version 1.1.0

Une nouvelle version de la librairie OhMyCache viens d'être mise en ligne. Au programme vous aurez accès à 2 nouvelles fonctions `keys()` et `getAll()` ainsi que l'ajout d'un nouveau paramètres facultatif à la fonction `clear()`

## keys()

Cette fonction retourne l'ensemble des clés stockées en cache. Petite particularité, elle ne nettoiera pas les éléments expirés.

[Voir la documentation](http://flavienbeninca.fr/ohmycache/keys)

## getAll()

Cette fonction retournera l'ensemble des données stockées en cache sour forme d'objet "clé valeur". Contrairement à la fonction `keys()` les données expirées ne seront pas retournées.

[Voir la documentation](http://flavienbeninca.fr/ohmycache/getAll)

## Clear()

Le paramètre `onlyExpired` a été ajouté, s'il est passé à `true` seul les données expirées seront supprimées (cela permet de libérer de l'espace de stockage).
Par défaut il est égal à `false` et toutes les données seront supprimées (y compris celles en lecture seule)

[Voir la documentation](http://flavienbeninca.fr/ohmycache/clear)