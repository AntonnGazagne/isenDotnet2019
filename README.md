# Prérequis
* Installer Visual studio Code
* Installer .Net Core SDK 3.0

# Préparation de la structure de la solution
* mkdir isen.DotNet
* cd isen.DotNet
* git init
* touch .gitignore(Ou créer un fichier à partir de VSC)
* puis récupérer un gitignore spécifique à .Net Core

Créer un repository sur GitHub:
* git remote add origin https://github.com/AntonnGazagne/isen-.NetCore.git
* faire un commit initial de nos sources:
** git add .
** git commit -m "initial commit"
** git push origin master

Créer un projet Console, dans un sous-dossier src:
* Créer le dossier src/ et naviguer dedans
* Dans le dossier src, créer isen.DotNet.ConsoleApp et naviguer dedans
* Créer le projet console : dotnet new console

Créer le fichier solution(.sln):
* naviguer vers la racine du projet
* Créer le fichier .sln: dotnet new sln
* ajouter les différents éléments de la solution à ce projet(projet console)
** dotnet sln add src/isen.DotNet.ConsoleApp/

Créer un dossier src/isen.DotNet.Library et naviguer dedans
* Avec la CLI .Net (dont l'interface en ligne de commande,que l'on utilise depuis le début), créer un projet de type 'librairie de classe': 
** dotnet new classlib

Référencer ce nouveau projet dans le fichier de solution (.sln)
* Depuis la racine: dotnet sln add src/isen.DotNet.Library

Ajouter le projet Library comme référence du projet ConsoleApp:
* Naviguer dans le dossier projet console
* dotnet add reference ../isen.DotNet.Library

#  Le C#
## Création d'une classe Hellow
Supprimer la classe autogénérée (class1.cs)
Créer un fichier Hello.cs et coder la classe

## Création d'une classe MyCollection
Cette classe aura pour but de manipuler une liste de string dans un premier temps.
* Créer dans le projet Library un sous-dossier Lists,
une classe MyCollection

## Ajouter un projet de test unitaires
* A la racine de la solution, créer un dossier 'tests' et un sous-dossier isen.DotNet.Library.Tests
* Naviguer dans ce dossier
* dotnet new xunit
* Ajouter ce projet au sln. depuis la racine: dotnet sln add tests/isen.DotNent.Library.Tests
* Revenir dans le dossier du projet tests
* Référencer le projet Library dans le projet test: dotnet add reference ../../src/isen.DotNet.Library
* Renommer la classe générée automatiquement de test par MyCollectionTest
* Coder un test de la méthode Add
* Lancer le test: dotnet test
* Coder les accesseur Indexeurs
* Coder les méthodes de test du get et du set 

## Refactoring de la classe MyCollection en classe générique
* Réécriture de la classe 'MyCollection' en classe générique
* Modification de la classe 'MyCollection' en 'MyCollection<string>'
* Dupliquer MyCollectionStringTest en MyCollectionCharTest

## Implémentation de l'interface IList<T>

* Indiquer l'implémentation de l'héritage de l'interface IList<T>
* Utiliser l'ampoule pour générer le using manquant et implémenter les méthode de l'interface
* Coder ensuite les méthodes et leurs tests

### Aparté sur les types nullables

``` csharp

    //Person est un type de référence
    Person person;//null
    person = new Person();//Pas null
    person = null;//null

    //int est un Value Type
    //Les types primitifs (bool, int, long, float...)
    //sont des types valeur

    bool b; //pas null, il vaut sa valeur par défaut(false)
    b = true; //true
    // b = null // interdit

    //bool? est un bool nullable(type référence)
    // bool? != bool
    //bool? == Nullable<bool>
    bool? nb = null;//null
    Nullable<bool> nbb; //null aussi
    var hasValue = nb.HasValue;//false
    var val = nb.Value; //true

```

## Migration vers une entité

Ajouter un champ Id (Int) et un champ Name.
Pour le champ Name, On va déclarer explicitement le champ du backup privé _name

### Modele City

Créer dans models une classe City avec Id et Name + ZipCode

Dans Person, ajouter un champ BornIn de type City

### Refactoring

Déplacer Id et Name vers une classe de base ,abstraite : 'BaseMode'.

Faire dériver City et Person de BaseModel.
Noter les champs Id et Name comme des override(puisque les champs sont virtuals)

Implémenter un mode d'affichage de base (string), overridable.

Compléter ce mécanisme afin d'ajouter le Zipcode à l'affichage des City

Puis reprendre l'affichage d'une Person.

## Création de Repositories

Créer cette arbo:
[Library]
    /Repositories
    /Repositories/Base (Repo abstrait)
    /Repositories/Interfaces (Base et interface spécifiques)
    /Repositories/InMemory(implémentation InMemory)

Créer la classe InMemoryCityRepository dans InMemory
Implémenter une liste test(ModelCollection).

Ajouter 2 méthodes Single : Recherche par Id et recherche par Name.
Ecrire des tests unitaires pour tester les méthodes