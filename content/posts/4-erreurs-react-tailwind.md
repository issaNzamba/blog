---
title: "Les 4 erreurs React et Tailwind CSS qui m'ont fait perdre du temps (et comment les éviter)"
date: 2026-07-24T08:00:00+00:00
draft: false
tags: ["React", "Tailwind CSS", "TanStack Router", "CSS", "Frontend", "Génie Logiciel"]
categories: ["Développement Web"]
summary: "Retour d'expérience pratique sur 4 pièges fréquents en React et Tailwind CSS (TanStack Router, débordement Flexbox, grilles rigides, tables réactives) et leurs solutions."
showToc: true
TocOpen: true
---

Quand on débute en tant qu'ingénieur logiciel, on passe souvent plus de temps à corriger des bugs d'interface qu'à développer de nouvelles fonctionnalités. C'est normal. L'important est de comprendre pourquoi ces problèmes apparaissent pour ne plus les reproduire.

Voici quatre erreurs que j'ai rencontrées sur un projet React avec Tailwind CSS, ainsi que les solutions qui m'ont permis de les résoudre définitivement.

---

## 1. Oublier de mettre à jour les routes TanStack Router

Avec **TanStack Router**, un fichier nommé `routeTree.gen.ts` est généré automatiquement. Son rôle est de maintenir un système de routage fortement typé.

Le piège est simple : après avoir créé ou modifié une page, beaucoup de développeurs ne commitent que leur nouveau composant et oublient ce fichier généré.

### Résultat :
* Erreurs de typage pendant la compilation ;
* Routes introuvables en production ;
* Comportement différent entre les machines des développeurs.

### La bonne habitude
À chaque modification des routes, ajoute toujours les deux fichiers dans ton commit :
1. Le nouveau composant ou la route ;
2. `routeTree.gen.ts`.

Tu éviteras ainsi des bugs difficiles à comprendre.

---

## 2. Quand la sidebar cache une partie du contenu

Sur une page de facturation, une partie du texte disparaissait sous la barre latérale. Par exemple, *« Facturation complète »* devenait *« ...turation complète »*.

Le problème ne venait pas de React, mais du fonctionnement de **Flexbox**. Par défaut, un élément flex possède `min-width: auto`. Si son contenu devient trop large,il refuse de se réduire et finit par passer sous une sidebar fixe.

### La solution
Il suffit d'ajouter les classes Tailwind suivantes sur le conteneur principal :

```html
<main class="min-w-0 w-full">
  <!-- Contenu de la page -->
</main>
```

Cette petite modification (`min-w-0 w-full`) permet au composant de respecter l'espace disponible et empêche le contenu de passer sous la sidebar.

---

## 3. Des colonnes de tableau trop rigides

Une autre erreur fréquente consiste à définir les colonnes d'une grille avec des largeurs fixes.

**Exemple problématique :**
```css
grid-template-columns: 130px 200px 220px 110px;
```

Au premier abord, tout semble fonctionner. Mais sur des écrans intermédiaires, le tableau devient plus large que son conteneur et casse complètement la mise en page.

### Une meilleure approche
À la place, utilise des unités relatives (`fr`) :

```css
grid-template-columns: 1.2fr 1.4fr 1fr;
```

Les colonnes s'adaptent automatiquement à la taille de l'écran tout en conservant un bon équilibre visuel.

---

## 4. Oublier le défilement horizontal des tableaux

Les tableaux contenant beaucoup de colonnes posent souvent problème sur les petits écrans. Sans protection, ils élargissent toute la page. Les cartes, l'en-tête et même la sidebar se retrouvent décalés.

### La solution
Encapsule le tableau dans un conteneur réactif avec :

```html
<div class="overflow-x-auto w-full">
  <table class="min-w-[600px] w-full">
    <!-- Contenu du tableau -->
  </table>
</div>
```

Ainsi, seul le tableau défile horizontalement sur mobile. Le reste de l'interface reste parfaitement stable.

---

## Ce qu'il faut retenir

Ces quatre problèmes sont très courants sur les projets React et Tailwind CSS, surtout lorsqu'on débute. Si tu prends l'habitude de :

1. **Committer `routeTree.gen.ts`** avec chaque nouvelle route ;
2. **Utiliser `min-w-0`** dans les layouts Flexbox ;
3. **Privilégier les unités `fr`** plutôt que des largeurs fixes ;
4. **Encapsuler les grands tableaux** avec `overflow-x-auto`,

tu gagneras un temps précieux et tu développeras des interfaces plus robustes, plus réactives et plus agréables à utiliser.

> Ce sont de petits détails techniques, mais ils font souvent la différence entre une application qui fonctionne *"à peu près"* et une application prête pour la production.
