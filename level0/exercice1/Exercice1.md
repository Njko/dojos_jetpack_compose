# Exercice 1

## Préambule

Ce premier exercice a été conçu pour vous familiariser avec l'environnement de travail. L'objectif n'est pas encore de réaliser une interface cohérente et jolie car ce sera l'objet des exercices et niveaux suivants.

Concentrez vous sur les questions suivantes:

- Comment Android Studio présente-t-il les previews Compose?
- Comment puis-je tirer parti des Previews pour voir en direct le résultat des modifications que j'apporte à un écran?
- Quelles sont les premières briques de construction d'un écran?
- Comment, à l'aide de ces premières briques, puis-je positionner un élément visible à l'écran comme un texte?

## Composables utilisés pour cet exercice

- [Column / Row / Box](https://developer.android.com/jetpack/compose/layouts/basics?hl=fr)
- [Text](https://developer.android.com/jetpack/compose/text?hl=fr)

Optionnel:

- [Scaffold](https://developer.android.com/jetpack/compose/layouts/material?hl=fr#scaffold)

## Notions supplémentaire à découvrir

- [Les Modificateurs (ou Modifier)](https://developer.android.com/jetpack/compose/modifiers?hl=fr)

## Références

Si vous souhaitez d'avantage de références, reportez vous au fichier [suivant](../Notions.md) qui contient toutes les notions de bases de Compose.

## Etapes à suivre

**Objectif:** Créer un écran avec une liste verticale de cartes. Chaque carte contient une image superposée d'un texte et un titre en dessous. Cette structure mettra en évidence l'utilisation de `Column`, `Row` et `Box`.

### Étape 1: Préparation de l'interface principale

Créer un nouveau projet Android avec Compose.

> Fichier > Nouveau Projet > Empty Activity (Logo Compose) > (Changer nom projet et package)

Renommez le composable `Greetings` par `MainScreen`.

```kotlin
@Composable
fun MainScreen() {
    // Le contenu sera ajouté dans les étapes suivantes
}
```

Vous pouvez utiliser la preview pour voir les changements en temps réel sans le lancer sur votre smartphone

```kotlin
@Preview
@Composable
fun MainScreenPreview() {
    Scaffold {
        MainScreen()
    }
}
```

### Étape 2: Utilisation de `Column` pour une disposition verticale

**Qu'est-ce qu'un `Column`?**  
Un `Column` est un composable dans Jetpack Compose conçu pour disposer d'autres composables verticalement, les uns en dessous des autres.

**Qu'est-ce qu'un `Modifier`?**  
Un `Modifier` est un outil puissant dans Jetpack Compose qui vous permet de modifier ou d'ajouter des comportements aux composables. Pensez-y comme à une manière de "styler" ou d'ajuster comment un composable apparaît ou fonctionne.

Dans cet exemple, nous utilisons:

- `Modifier.fillMaxSize()`: Il étend le composable pour qu'il occupe tout l'espace disponible.
- `.padding(16.dp)`: Ajoute un espace tout autour du composable d'une taille de 16 unités de densité.

```kotlin
@Composable
fun MainScreen() {
    Column(
        modifier = Modifier.fillMaxSize().padding(16.dp),
        verticalArrangement = Arrangement.spacedBy(16.dp)
    ) {
        // Nous ajouterons nos cartes ici à la prochaine étape
    }
}
```

### Étape 3: Créer une carte avec `Box` pour la superposition

**Qu'est-ce qu'un `Box`?**  
`Box` est un composable qui permet de superposer plusieurs composables. Les enfants de `Box` sont dessinés dans l'ordre où ils sont définis, avec le dernier enfant étant dessiné en haut.

**Pourquoi passer le `Modifier` en paramètre?**  
Passer le `Modifier` en tant que paramètre permet une plus grande réutilisabilité du composable. Cela signifie que vous pouvez ajuster la taille, l'espacement, la couleur, etc., chaque fois que vous utilisez le composable, sans avoir à modifier le code interne du composable lui-même.

```kotlin
@Composable
fun ImageCard(imageModifier: Modifier, title: String) {
    Column {
        // Ici, nous utilisons Box pour superposer du texte sur une image
        Box(
            modifier = imageModifier
        ) {
            // Image de fond (pour l'instant, utilisons une couleur unie)
            Spacer(modifier = Modifier.fillMaxSize().background(Color.Blue))

            // Texte superposé au centre de l'image
            Text(text = "Superposé", modifier = Modifier.align(Alignment.Center), color = Color.White)
        }

        // Titre sous l'image
        Text(text = title, style = typography.h6, modifier = Modifier.align(Alignment.CenterHorizontally))
    }
}

```

### Étape 4: Utilisation de `Row` pour une disposition horizontale

**Qu'est-ce qu'un `Row`?**  
Un `Row` est un composable qui dispose d'autres composables horizontalement, côte à côte. Il est souvent utilisé pour créer des éléments de liste, des barres d'outils ou toute autre disposition où vous voulez que les éléments soient alignés horizontalement.

**Les différents `Arrangement` dans `Row`:**

- `Arrangement.Start`: Place les enfants au début de la `Row`.
- `Arrangement.End`: Place les enfants à la fin de la `Row`.
- `Arrangement.Center`: Centre les enfants horizontalement dans la `Row`.
- `Arrangement.SpaceBetween`: Maximise l'espace entre les enfants, sans espace avant le premier et après le dernier enfant.
- `Arrangement.SpaceAround`: Répartit également l'espace entre les enfants, y compris avant le premier et après le dernier.
- `Arrangement.SpaceEvenly`: Répartit également l'espace entre les enfants et autour d'eux.

``` kotlin
@Composable
fun ImageCard(imageModifier: Modifier, title: String) {
    Column {
        // ... (Code de Box d'avant)

        // Row pour les boutons
        Row(
            modifier = Modifier.fillMaxWidth(),
            horizontalArrangement = Arrangement.SpaceBetween
        ) {
            Button(onClick = { /* Action */ }) {
                Text("Bouton 1")
            }
            Button(onClick = { /* Action */ }) {
                Text("Bouton 2")
            }
        }
    }
}
```

### Étape 5: Combiner le tout

Dans `MainScreen`, ajoutez plusieurs cartes pour voir comment elles s'empilent verticalement.

```kotlin
@Composable
fun MainScreen() {
    Column(
        // ... (Code de Column d'avant)
    ) {
        ImageCard(Modifier.height(200.dp).fillMaxWidth(), "Carte 1")
        ImageCard(Modifier.height(150.dp).fillMaxWidth(), "Carte 2")
    }
}
```
