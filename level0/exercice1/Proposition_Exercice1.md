# Proposition Exercice 1

```kotlin
@Composable
fun MainScreen() {
    Column(
        modifier = Modifier.fillMaxSize().padding(16.dp),
        verticalArrangement = Arrangement.spacedBy(16.dp)
    ) {
        ImageCard(Modifier.height(200.dp).fillMaxWidth(), "Carte 1")
        ImageCard(Modifier.height(150.dp).fillMaxWidth(), "Carte 2")
    }
}

@Composable
fun ImageCard(imageModifier: Modifier, title: String) {
    Column {
        Box(
            modifier = imageModifier
        ) {
            Modifier.background(Color.Blue)
            Text(text = "Superposé", modifier = Modifier.align(Alignment.Center), color = Color.White)
        }

        Text(text = title, style = typography.h6, modifier = Modifier.align(Alignment.CenterHorizontally))

        Row(
            modifier = Modifier.fillMaxWidth().padding(16.dp),
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

**Récapitulatif:**

- **Column**: Utilisé pour disposer les composables verticalement.
- **Row**: Dispose les composables horizontalement, avec diverses options pour l'arrangement des enfants.
- **Box**: Permet la superposition de composables.
- **Modifier**: Permet de "styler" ou d'ajuster les composables, en définissant des aspects tels que la taille, la couleur, l'espacement, etc.
