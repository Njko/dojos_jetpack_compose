# Exercice 1

## Préambule

Ce premier exercice a été conçu pour vous familiariser avec l'environnement de travail. L'objectif n'est pas encore de réaliser une interface cohérente et jolie car ce sera l'objet des exercices et niveaux suivants.

Concentrez vous sur les questions suivantes:
- Comment Android Studio présente-t-il les previews Compose?
- Comment puis-je tirer parti des Previews pour voir en direct le résultat des modifications que j'apporte à un écran?
- Quelles sont les premières briques de construction d'un écran?
- Comment, à l'aide de ces premières briques, puis-je positionner un élément visible à l'écran comme un texte?

## Composables utilisés pour cet exercice

* [Column / Row / Box](https://developer.android.com/jetpack/compose/layouts/basics?hl=fr)
* [Text](https://developer.android.com/jetpack/compose/text?hl=fr)

Optionnel:
* [Scaffold](https://developer.android.com/jetpack/compose/layouts/material?hl=fr#scaffold)

## Notions supplémentaire à découvrir

* [Les Modificateurs (ou Modifier)](https://developer.android.com/jetpack/compose/modifiers?hl=fr)

## Références

Si vous souhaitez d'avantage de références, reportez vous au fichier [suivant](../Notions.md) qui contient toutes les notions de bases de Compose.

## Etapes à suivre

1. Créer un nouveau projet Android avec Compose.

    Fichier > Nouveau Projet > Empty Activity (Logo Compose) > (Changer nom projet et package)

1. Changer "Android" pour votre prénom

1. Ligne 34 ajouter un style au texte pour mettre en titre (paramètre style avec la valeur MaterialTheme.typography.titleLarge)

1. Ligne 31 inscrire le Composable Text dans une Column puis inscrire la Column dans un Scaffold


1. Ajoutez un modifier à la Column et appliquer le modifier fillMaxWidth

1. Ajoutez un horizontalAlignment à la Column et centrer le contenu

1. Ajoutez un deuxième Text dans la colonne

1. Ajoutez un verticalArrangement à la colonne pour espacer les deux textes

1. Changez le Column en Row

1. Ajustez les Arrangement et Alignment de la sorte pour le Row: paramètre verticalAlignment (Alignment.CenterVertically) et horizontalArrangement (Arrangement.SpaceBetween)

1. Changez le Row en Box

1. Retirez les Arrangement et Alignement en paramètres de Box

1. Ajoutez le modifier sur Text "Hello" et ajoutez le paramètre .align(Alignment.Center)

1. Testez d'autres modifier sur le Text

=> 3 sous exercices pour éviter de modifier des composables déjà écrit
