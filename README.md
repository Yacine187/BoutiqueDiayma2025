# TP C# / ASP.NET Core – Boutique Diayma 2025

## 1. Présentation du projet

Ce TP consiste à récupérer, exécuter, analyser et déboguer une application web développée avec ASP.NET Core.

**Dépôt GitHub personnel :**  
https://github.com/Yacine187/BoutiqueDiayma2025

**Dépôt d'origine :**  
https://github.com/devehod/BoutiqueDiayma2025

## 2. Structure de la solution

- Solution : `Diayma.sln`
- Projet : `P2FixAnAppDotNetCode`
- Fichier de configuration : `Diayma.csproj`

Le projet cible le framework `netcoreapp2.0`.

SDK .NET installé sur la machine : `10.0.403`.

## 3. Exécution de l'application

L'application a été compilée et exécutée avec succès à l'aide des commandes :

```bash
dotnet build
dotnet run
```

L'application est accessible localement à l'adresse :

`http://localhost:62929`

## 4. Identification des bugs

### Bug 1 : ajout d'une quantité supérieure au stock disponible

**Description :** L'application autorise l'ajout d'une quantité supérieure au stock disponible.

**Scénario de reproduction :**
- Le produit Xiaomi Redmi 13C possède un stock de 10 unités.
- Ajouter 11 unités au panier.

**Résultat obtenu :** Le panier accepte les 11 unités.

**Résultat attendu :** L'application devrait empêcher l'ajout d'une quantité supérieure au stock disponible et afficher un message d'information.

### Bug 2 : problème de traduction en espagnol

**Description :** Le changement de langue vers l'espagnol ne fonctionne pas correctement.

**Scénario de reproduction :**
- Sélectionner la langue espagnole.
- Valider le changement de langue.

**Résultat obtenu :** L'interface reste en français, alors que la sélection de l'anglais fonctionne.

**Résultat attendu :** L'interface devrait être traduite en espagnol.

## 5. Points d'arrêt

Les cinq points d'arrêt suivants ont été placés :

1. `CartSummaryViewComponent.cs`, ligne 12.
2. `ProductController.cs`, ligne 15.
3. `OrderController.cs`, ligne 17.
4. `CartController.cs`, ligne 15.
5. `Startup.cs`, ligne 20.

## 6. Analyse du parcours d'exécution

L'objectif est de comprendre les différentes étapes exécutées avant l'affichage des produits sur la page d'accueil.

### Étape 1 : ProductController

- Namespace : `P2FixAnAppDotNetCode.Controllers`
- Classe : `ProductController`
- Méthode : `Index()`

Cette méthode appelle `ProductService.GetAllProducts()` pour récupérer les produits.

```csharp
List<Product> products = _productService.GetAllProducts();
return View(products);
```

### Étape 2 : ProductService

- Namespace : `P2FixAnAppDotNetCode.Models.Services`
- Classe : `ProductService`
- Méthode : `GetAllProducts()`

Cette méthode appelle `ProductRepository.GetAllProducts()` et convertit le résultat en liste avec `ToList()`.

### Étape 3 : ProductRepository

- Namespace : `P2FixAnAppDotNetCode.Models.Repositories`
- Classe : `ProductRepository`
- Méthode : `GetAllProducts()`

Cette méthode récupère les produits, filtre ceux dont le stock est supérieur à zéro et les trie par ordre alphabétique de leur nom.

```csharp
List<Product> list = _products
    .Where(p => p.Stock > 0)
    .OrderBy(p => p.Name)
    .ToList();

return list.ToArray();
```

### Étape 4 : Retour vers le contrôleur

Le tableau de produits est retourné au service, converti en liste, puis transmis à la vue avec :

```csharp
return View(products);
```

### Étape 5 : Vue Product

Fichier : `Views/Product/Index.cshtml`

La vue reçoit une collection de produits :

```cshtml
@model IEnumerable<P2FixAnAppDotNetCode.Models.Product>
```

Elle utilise une boucle `foreach` pour parcourir les produits et afficher leur nom, description, prix et stock.

## 7. Différences entre les modes de débogage

### Pas à pas principal (F10)

Permet d'exécuter une instruction sans entrer dans les méthodes appelées. Il est utile pour avancer rapidement dans le code.

### Pas à pas détaillé (F11)

Permet d'entrer dans une méthode appelée afin d'observer son fonctionnement instruction par instruction.

### Pas à pas sortant (Maj + F11)

Permet de terminer l'exécution de la méthode actuelle et de revenir à la méthode appelante.

Ces trois fonctionnalités facilitent la compréhension du fonctionnement d'une application et la recherche d'erreurs.

## 8. Déploiement

Cette section sera complétée après la génération de l'exécutable Windows et son dépôt sur Google Drive ou OneDrive.

## 9. Conclusion

Ce TP a permis de découvrir la structure d'une application ASP.NET Core, de l'exécuter, d'identifier des anomalies, de placer des points d'arrêt et de suivre le parcours d'exécution à l'aide du débogueur de Visual Studio Code.