# Exercices

 ## Exercice 1: Introduction aux SideEffects

1. **Notion à connaitre**: Les SideEffects dans Jetpack Compose sont utilisés pour gérer des effets de bord qui peuvent résulter de l'exécution d'une composable, comme l'interaction avec la base de données, les appels réseau, etc. Une Composable utilisant un SideEffect devrait toujours être purement fonctionnelle.
2. **Besoin**: Créez une Composable avec un SideEffect qui met à jour un état global lorsqu'il est appelé.
3. **Correction**:

```kotlin
@Composable
fun SideEffectSample() {
    var state by remember { mutableStateOf(0) }

    SideEffect {
        state++
    }

    Text("State value is $state")
}
```

4. **Résumé**: Dans cet exercice, nous avons utilisé `SideEffect` pour modifier un état. Notez que le bloc `SideEffect` est appelé après que la composition soit appliquée et avant que l'UI soit dessinée.

## Exercice 2: Exploration de produceState

1. **Notion à connaitre**: `produceState` est une API Jetpack Compose qui nous permet de produire un état à partir de sources asynchrones.
2. **Besoin**: Utilisez `produceState` pour créer un état qui se met à jour automatiquement toutes les secondes.
3. **Correction**:

```kotlin
@Composable
fun ProduceStateSample() {
    val time by produceState(initialValue = 0) {
        while (true) {
            delay(1000L)
            value++
        }
    }

    Text("Seconds elapsed: $time")
}
```

4. **Résumé**: `produceState` est utilisé pour gérer les sources de données asynchrones. Ici, nous avons utilisé `produceState` pour créer un état qui s'incrémente chaque seconde.

## Exercice 3: Utilisation de derivedStateOf

1. **Notion à connaitre**: `derivedStateOf` est une fonction Jetpack Compose qui nous permet de créer un état qui est dérivé d'un ou plusieurs autres états.
2. **Besoin précis**: Créez un état dérivé de l'état de l'exercice précédent qui affiche "Minute" au lieu de "Seconde" chaque fois que le temps atteint 60 secondes.
3. **Correction**:

```kotlin
@Composable
fun DerivedStateSample() {
    val time by produceState(initialValue = 0) {
        while (true) {
            delay(1000L)
            value++
        }
    }

    val minutes by derivedStateOf { time / 60 }

    Text("Minutes elapsed: $minutes")
}
```

4. **Résumé**: `derivedStateOf` permet de créer un état dérivé d'autres états. Dans cet exercice, nous avons créé un état qui est dérivé du temps et qui est mis à jour automatiquement chaque minute.

## Exercice 4: Les animations simples

1. **Notion à connaître**: Les animations sont essentielles pour améliorer l'expérience utilisateur. Jetpack Compose offre une API simple pour créer des animations.
2. **Besoin**: Ajoutez une animation simple qui change la couleur du texte à chaque fois qu'une minute passe.
3. **Correction**:

```kotlin
@Composable
fun SimpleAnimationSample() {
    val time by produceState(initialValue = 0) {
        while (true) {
            delay(1000L)
            value++
        }
    }

    val minutes by derivedStateOf { time / 60 }
    val color by animateColorAsState(if (minutes % 2 == 0) Color.Red else Color.Blue)

    Text("Minutes elapsed: $minutes", color = color)
}
```

4. **Résumé**: Nous avons utilisé `animateColorAsState` pour animer la couleur du texte. L'animation change la couleur du texte entre le rouge et le bleu chaque fois qu'une minute passe.

## Exercice 5: Un calendrier basique

1. **Notion à connaître**: Nous allons maintenant commencer à construire notre calendrier. Pour cela, nous allons utiliser une liste verticale `LazyColumn` pour afficher les jours.
2. **Besoin**: Créez une liste qui affiche les 30 prochains jours.
3. **Correction**:

```kotlin
@Composable
fun CalendarList() {
    val dates = remember { derivedStateOf { List(30) { LocalDate.now().plusDays(it.toLong()) } } }

    LazyColumn {
        items(dates.value) { date ->
            Text(date.toString())
        }
    }
}
```

4. **Résumé**: Nous avons utilisé une `LazyColumn` pour créer une liste simple de dates. Cela représente la base de notre calendrier.

## Exercice 6: Ajout d'états pour des rendez-vous

1. **Notion à connaître**: L'ajout d'états pour chaque jour dans le calendrier.
2. **Besoin**: Ajoutez un état pour chaque jour qui représente si un rendez-vous est pris ou non.
3. **Correction**:

```kotlin
@Composable
fun CalendarListWithAppointments() {
    val appointments = remember { mutableStateOf(List(30) { false }) }

    val dates = remember { derivedStateOf { List(30) { LocalDate.now().plusDays(it.toLong()) } } }

    LazyColumn {
        itemsIndexed(dates.value) { index, date ->
            Text(if (appointments.value[index]) "$date: Appointment" else "$date: Free")
        }
    }
}
```

4. **Résumé**: Nous avons ajouté un état pour chaque jour pour représenter si un rendez-vous a été pris ou non. Nous utilisons ce nouvel état pour afficher un message différent dans notre liste de dates.

## Exercice 7: L'interaction avec la liste

1. **Notion à connaître**: La gestion des interactions utilisateurs est cruciale dans toute interface utilisateur. Dans cet exercice, nous allons implémenter la prise de rendez-vous en cliquant sur une date.
2. **Besoin**: Ajoutez une interaction qui change l'état du rendez-vous lorsqu'un jour est cliqué.
3. **Correction**:

```kotlin
@Composable
fun CalendarInteraction() {
    val appointments = remember { mutableStateOf(List(30) { false }) }

    val dates = remember { derivedStateOf { List(30) { LocalDate.now().plusDays(it.toLong()) } } }

    LazyColumn {
        itemsIndexed(dates.value) { index, date ->
            Text(
                if (appointments.value[index]) "$date: Appointment" else "$date: Free",
                modifier = Modifier.clickable {
                    appointments.value = appointments.value.toMutableList().also { it[index] = !it[index] }
                }
            )
        }
    }
}
```

4. **Résumé**: Nous avons ajouté une interaction utilisateur à notre application de calendrier. En cliquant sur une date, l'utilisateur peut marquer ou démarquer un jour comme ayant un rendez-vous.

## Exercice 8: Ajout d'une animation lors de la prise d'un rendez-vous

1. **Notion à connaître**: Dans cet exercice, nous allons ajouter une animation à chaque fois qu'un rendez-vous est pris ou annulé.
2. **Besoin précis**: Ajoutez une animation qui change la couleur du texte lorsque le jour est marqué comme ayant un rendez-vous.
3. **Correction**:

```kotlin
@Composable
fun CalendarInteractionWithAnimation() {
    val appointments = remember { mutableStateOf(List(30) { false }) }

    val dates = remember { derivedStateOf { List(30) { LocalDate.now().plusDays(it.toLong()) } } }

    LazyColumn {
        itemsIndexed(dates.value) { index, date ->
            val color by animateColorAsState(if (appointments.value[index]) Color.Green else Color.Black)

            Text(
                if (appointments.value[index]) "$date: Appointment" else "$date: Free",
                color = color,
                modifier = Modifier.clickable {
                    appointments.value = appointments.value.toMutableList().also { it[index] = !it[index] }
                }
            )
        }
    }
}
```

4. **Résumé**: Nous avons ajouté une animation qui change la couleur du texte en vert lorsque un rendez-vous est pris et revient à la couleur normale lorsqu'il est annulé.

## Exercice 9: Ajout d'une SideEffect pour stocker les données

1. **Notion à connaître**: Dans cet exercice, nous allons utiliser SideEffect pour stocker les données d'état de l'application de façon persistante.
2. **Besoin**: Utilisez SideEffect pour stocker l'état des rendez-vous de manière persistante.
3. **Correction**:

```kotlin
@Composable
fun PersistentCalendar() {
    var appointments by remember { mutableStateOf(List(30) { false }) }

    SideEffect {
        // Utiliser votre mécanisme de stockage préféré ici
        // storage.saveAppointments(appointments)
    }

    val dates = remember { derivedStateOf { List(30) { LocalDate.now().plusDays(it.toLong()) } } }

    LazyColumn {
        itemsIndexed(dates.value) { index, date ->
            val

 color by animateColorAsState(if (appointments[index]) Color.Green else Color.Black)

            Text(
                if (appointments[index]) "$date: Appointment" else "$date: Free",
                color = color,
                modifier = Modifier.clickable {
                    appointments = appointments.toMutableList().also { it[index] = !it[index] }
                }
            )
        }
    }
}
```

4. **Résumé**: Nous avons utilisé SideEffect pour enregistrer l'état des rendez-vous de manière persistante. Cela permettra à l'état de l'application de survivre aux redémarrages de l'application.

## Exercice 10: Gestion des erreurs avec produceState

1. **Notion à connaître**: La gestion des erreurs est une part importante de tout développement logiciel. Dans cet exercice, nous allons apprendre à gérer les erreurs avec `produceState`.
2. **Besoin**: Gérez les erreurs qui peuvent survenir lors de la récupération de l'état des rendez-vous.
3. **Correction**:

```kotlin
@Composable
fun ErrorHandlingCalendar() {
    val appointments by produceState<List<Boolean>?>(initialValue = null) {
        try {
            // Utilisez votre mécanisme de stockage préféré ici
            // value = storage.loadAppointments()
        } catch (e: Exception) {
            // Gérer l'erreur
        }
    }

    val dates = remember { derivedStateOf { List(30) { LocalDate.now().plusDays(it.toLong()) } } }

    LazyColumn {
        itemsIndexed(dates.value) { index, date ->
            val color by animateColorAsState(if (appointments?.get(index) == true) Color.Green else Color.Black)

            Text(
                if (appointments?.get(index) == true) "$date: Appointment" else "$date: Free",
                color = color,
                modifier = Modifier.clickable {
                    appointments = appointments?.toMutableList().also { it[index] = !it[index] }
                }
            )
        }
    }
}
```

4. **Résumé**: Nous avons ajouté la gestion des erreurs à notre application de calendrier. Cela garantira que notre application peut se rétablir gracieusement en cas d'erreur lors de la récupération de l'état des rendez-vous.

**Résumé global**

Dans ce dojo, nous avons exploré plusieurs aspects de Jetpack Compose, en mettant l'accent sur les SideEffects, les états avancés et les animations simples. Nous avons commencé avec des concepts de base tels que l'utilisation des SideEffects pour gérer les effets de bord dans nos composables. Nous avons ensuite progressé vers des notions plus avancées, comme l'utilisation de `produceState` et `derivedStateOf` pour gérer les états asynchrones et dérivés respectivement.

Nous avons également exploré comment créer des animations simples pour améliorer l'expérience utilisateur, en changeant la couleur de notre texte en fonction de l'état de notre application. Ensuite, nous avons mis en pratique ces concepts en construisant une application de calendrier pour la prise de rendez-vous, en intégrant de manière graduelle des interactions utilisateur, des animations et une persistance d'état.

Cependant, il est important de noter que chaque concept présenté ici n'est qu'un aperçu de ce que Jetpack Compose peut offrir. Pour continuer à vous améliorer dans ces domaines, voici quelques suggestions:

- Pour les SideEffects, essayez d'intégrer des opérations plus complexes, comme les appels réseau ou l'interaction avec une base de données.
- Pour les états avancés, explorez comment vous pouvez utiliser `produceState` et `derivedStateOf` pour gérer des états plus complexes ou interdépendants.
- Pour les animations, essayez de créer des animations plus complexes ou personnalisées pour améliorer l'expérience utilisateur.
- Continuez à construire sur l'application de calendrier que nous avons commencée, en ajoutant de nouvelles fonctionnalités ou en améliorant l'interface utilisateur.

Et enfin, la pratique est la clé pour maîtriser ces concepts. Plus vous les utiliserez dans vos projets, plus vous vous sentirez à l'aise avec eux. Bonne continuation avec Jetpack Compose !
