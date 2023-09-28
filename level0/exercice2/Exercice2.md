# Exercice 2

## Préambule

Ce deuxième exercice a pour but de rentrer progressivement dans les sujets sérieux. Pour cela, nous allons créer ce qui est communément appelé une [Carte](https://m3.material.io/components/cards/overview) (ou Card en anglais). Il s'agit d'un simple cadre contenant du texte et des images.

Même si l'objectif cité ci dessous donne une maquette haute fidélité, le respect de cette maquette reste à votre discrétion. Amusez vous à tester différentes disposition de texte, d'image, de marges, etc. Les dimensions ne sont pas fournies volontairement pour ne donner aucune contrainte à votre créativité.

N'oubliez pas que les notions essentielles à ce niveau 0 sont dispo ici <mettre lien vers doc>

## Composables utilisés pour cet exercice

* [Column / Row / Box](https://developer.android.com/jetpack/compose/layouts/basics?hl=fr)
* [Text](https://developer.android.com/jetpack/compose/text?hl=fr)
* [Card*](https://www.jetpackcompose.net/jetpack-compose-card)
* [Image*](https://developer.android.com/jetpack/compose/graphics/images/loading?hl=fr)

(*) Nouveau composable par rapport aux exercices précédents

Optionnel:
* [Scaffold](https://developer.android.com/jetpack/compose/layouts/material?hl=fr#scaffold)

## Objectif

Créez un composable nommé "CardPhoto" comme l'exemple ci dessous:

![Objectif exercice](img/feedItem.png)

Pour les ressources, un dossier nommé "/ressource" contient les images à intégrer. Et une documentation est disponible pour ajouter une image dans le projet à cette adresse: <ajouter /notion.md paragraphe ajouter une image>

Voici la base :

```kotlin
@Composable
fun CardPhoto() {
   // Déssiner le cadre photo ici
}


@Preview
@Composable
fun CardPhotoPreview() {
    Column(modifier = Modifier.padding(20.dp)) {
        CardPhoto()
    }
}
```
