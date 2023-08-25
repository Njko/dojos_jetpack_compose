# Proposition Exercice 1

```kotlin
@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun Greeting(name: String, modifier: Modifier = Modifier) {
    Scaffold {
        val padding = it
        Box(
            modifier = Modifier.fillMaxSize(),
        ) {
            Text(
                text = "Hello $name!",
                modifier = modifier.align(Alignment.Center),
                style = MaterialTheme.typography.titleLarge,
            )

            Text("Bienvenue chez nous")
        }
    }
}
```
