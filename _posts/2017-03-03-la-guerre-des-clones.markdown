---
layout:     post
title:      "[js] La guerre des clones"
subtitle:   "Cloner un objet ou un array en javascript n'ai pas aussi simple qu'un bonjour. Voici comment faire."
date:       2017-03-03 19:00:00
comments: true
header-img: "img/js.png"
hearder-bg-color: "#323330"
---

# La guerre des clones

Cloner un objet ou un array en javascript n'ai pas aussi simple qu'un bonjour. Voici comment faire.

## Explication

En javascript le "=" d'un objet ou d'un array ne va le cloner mais copier le pointeur de celui-ci.

La preuve est ci-dessous :

{% highlight js %}
var obj = {test: 'obj1'}
var obj2 = obj
obj2.test = 'obj2'

console.log(obj) // {test: 'obj2}
console.log(obj2) // {test: 'obj2}

var arr = [1]
var arr2 = arr
arr2.push(2)

console.log(arr) // [1,2]
console.log(arr2) // [1,2]
{% endhighlight %}

Bien sûr le plus souvent cela ne vous posera pas de problème mais si vous ne faites pas attention vous pourriez avoir des problèmes.

## Les objets

Comme les créateurs du javascript sont des méchants, ils ne nous ont pas fournis de fonction de clonage. Ils ont préféré se dire "ah ah on va les laisser se débrouiller, on va bien se marrer"

Heureusement il existe au moins 2 méthodes afin de cloner un objet.

### L'ami json

La première solution est de transformer notre objet en chaîne JSON puis de le retransformer en objet. On aurait donc logiquement 2 objets distinct.

{% highlight js %}
var obj = {test: 'obj1'}
var obj2 = JSON.parse(JSON.stringify(obj))
obj2.test = 'obj2'

console.log(obj) // {test: 'obj1}
console.log(obj2) // {test: 'obj2}
{% endhighlight %}

### Assign

La méthode Object.assign() est utilisée afin de copier les valeurs de toutes les propriétés directes (non héritées) d'un objet qui sont énumérables sur un autre objet cible.

Elle ne sert donc pas qu'à cloner un objet mais à merger des objets entre eux vers le premier paramètre. Pour cloner un objet il faut donc écrire :

{% highlight js %}
var obj = {test: 'obj1'}
var obj2 = Object.assign({}, obj)
obj2.test = 'obj2'

console.log(obj) // {test: 'obj1}
console.log(obj2) // {test: 'obj2}
{% endhighlight %}

## Les array

Pour les array la méthode la plus simple et d'utiliser la fonction `slice`

Mais que fait slice : "La méthode slice() renvoie un objet tableau, contenant une copie superficielle (shallow copy) d'une portion du tableau d'origine, la portion est définie par un indice de début et un indice de fin (exclus). Le tableau original ne sera pas modifié."

Donc si on fait un "slice" à partir de 0 jusqu'à la fin, on a un clone (youpi)

{% highlight js %}
var arr = [1]
var arr2 = arr.slice(0)
arr2.push(2)

console.log(arr) // [1]
console.log(arr2) // [1,2]
{% endhighlight %}

## Like a boss

Vous allez me dire "il sont bien beau tes slice et tes assign mais quand on va relire le code dans 2 mois on ne va pas forcément comprendre pourquoi on a fait ça", et vous avez parfaitement raison.

Et pour ceux qui sont partis avant, vous êtes ~~sale~~

### La méthode générique qui coupe la moutarde

{% highlight js %}
var clone = function (source) {
  if (Array.isArray(source)) {
  	return source.slice(0)
  } else if (typeof source === 'object') {
  	return Object.assign({}, source)
  }
  return source
}

var obj = {test: 'obj1'}
var obj2 = clone(obj)
obj2.test = 'obj2'

console.log(obj) // {test: 'obj1}
console.log(obj2) // {test: 'obj2}

var arr = [1]
var arr2 = clone(arr)
arr2.push(2)

console.log(arr) // [1]
console.log(arr2) // [1,2]
{% endhighlight %}

### Les prototypes

{% highlight js %}
Array.prototype.clone = function() {
	return this.slice(0)
}

Object.prototype.clone = function() {
	return Object.assign({}, this)
}

var obj = {test: 'obj1'}
var obj2 = obj.clone()
obj2.test = 'obj2'

console.log(obj) // {test: 'obj1}
console.log(obj2) // {test: 'obj2}

var arr = [1]
var arr2 = arr.clone()
arr2.push(2)

console.log(arr) // [1]
console.log(arr2) // [1,2]
{% endhighlight %}
