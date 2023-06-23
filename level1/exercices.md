# LEVEL1 : Exercices

## Exercice 1 : Local State

1. *Notion à connaitre* : Jetpack Compose utilise le concept de Local State pour représenter les données qui sont mutables à l'intérieur d'un composable. Ces données peuvent changer au fil du temps, ce qui peut entraîner la recomposition de votre composable.

2. *Besoin* : Créez un composable "CalendarDay" représentant un jour dans le calendrier avec une valeur booléenne représentant si le jour est sélectionné ou non. Vous pouvez considérer que tous les mois font 30 jours

3. *Correction* :

```kotlin
@Composable
fun CalendarDay(day: Int) {
    var isSelected by remember { mutableStateOf(false) }

    Box(modifier = Modifier
        .padding(4.dp)
        .background(if (isSelected) Color.Green else Color.White)
        .clickable { isSelected = !isSelected }
    ) {
        Text(text = day.toString(), color = Color.Black)
    }
}
```

4. *Résumé* : Dans cet exercice, nous avons appris comment utiliser un état local pour gérer l'état de sélection d'un jour dans notre composable. Nous utilisons `mutableStateOf()` pour créer une valeur mutable et `remember` pour garder cette valeur à travers les recompositions.

## Exercice 2 : State Hoisting

1. *Notion à connaitre* : Le State Hoisting est un concept dans Jetpack Compose qui fait référence au déplacement de l'état d'un composable à ses appelants. Cela permet une meilleure réutilisation et une séparation claire des responsabilités.

2. *Besoin* : Refactorez le composable "CalendarDay" pour utiliser le State Hoisting, c'est-à-dire que l'état de sélection doit être géré par le parent.

3. *Correction* :

```kotlin
@Composable
fun CalendarDay(day: Int, isSelected: Boolean, onSelectedChange: (Boolean) -> Unit) {
    Box(modifier = Modifier
        .padding(4.dp)
        .background(if (isSelected) Color.Green else Color.White)
        .clickable { onSelectedChange(!isSelected) }
    ) {
        Text(text = day.toString(), color = Color.Black)
    }
}
```

4. *Résumé* : En utilisant State Hoisting, nous avons permis au composable "CalendarDay" de devenir purement déclaratif. L'état est désormais géré par le parent, ce qui rend notre composable plus réutilisable.

## Exercice 3 : Recomposition

1. *Notion à connaitre* : Dans Jetpack Compose, la recomposition est le processus par lequel l'UI se met à jour en réponse à des changements d'état. Seuls les composables qui dépendent de cet état sont recomposés, ce qui rend le processus efficace.

2. *Besoin* : Ajoutez un compteur de clics qui se met à jour à chaque fois qu'un jour est sélectionné. Observez comment seul le composable lié au compteur se recompose lorsque l'état change.

3. *Correction* :

```kotlin
@Composable
fun Calendar() {
    var clickCount by remember { mutableStateOf(0) }

    Column {
        CalendarDay(day = 1, isSelected = false) { isSelected ->
            clickCount++
        }

        Text("Click count: $clickCount")
    }
}
```

4. *Résumé* : Nous avons observé que lorsque l'état de clickCount change, seul le composable Text se recompose et se met à jour, pas le composable CalendarDay. Cela démontre l'efficacité de la recomposition dans Jetpack Compose.

## Exercice 4 : Recomposition Avancée

1. *Notion à connaitre* : Jetpack Compose est efficace dans sa recomposition grâce à la possibilité de ne recomposer que les parties de l'interface utilisateur qui dépendent d'un état modifié. Cependant, des recompositions inutiles peuvent se produire si nous ne structurons pas correctement nos composables.

2. *Besoin* : Créez un composable "CalendarMonth" qui contient 30 instances de "CalendarDay". Chaque jour doit se mettre à jour individuellement lorsqu'il est sélectionné sans recomposer l'ensemble du mois.

3. *Correction* :

```kotlin
@Composable
fun CalendarMonth() {
    Column {
        for (day in 1..30) {
            var isSelected by remember { mutableStateOf(false) }
            CalendarDay(day = day, isSelected = isSelected) { newSelection ->
                isSelected = newSelection
            }
        }
    }
}
```

4. *Résumé* : Nous avons créé un composable qui représente un mois dans le calendrier. En utilisant correctement l'état local pour chaque jour, nous assurons que seuls les jours spécifiques sont recomposés lorsque leur état de sélection change.

## Exercice 5 : Profiler (Recomposition Count)

1. *Notion à connaitre* : Le Profileur de Jetpack Compose offre des outils pour mesurer la performance de votre application. L'une de ces métriques est le "Recomposition Count", qui indique combien de fois un composable a été recomposé.

2. *Besoin* : Utilisez le Profileur pour mesurer le nombre de recompositions de "CalendarDay" lorsque vous interagissez avec "CalendarMonth".

3. *Expérimentation* : 

Lancez votre application en mode debug, ouvrez le `Layout Inspector` et activez le `Live Updates`. Ensuite, sélectionnez un `CalendarDay` dans le `Component Tree`, vous pourrez voir le nombre de recompositions dans le `Properties Panel`.

4. *Résumé* : En utilisant le Profileur, nous pouvons avoir une idée précise de combien de fois nos composables sont recomposés. Cela peut nous aider à repérer les performances inefficaces et à améliorer la structure de nos composables.

## Exercice 6 : Debugger Layout Hierarchy

1. *Notion à connaitre* : Le Debugger de la hiérarchie de layout de Jetpack Compose vous permet de visualiser la structure de vos composables dans une interface graphique. C'est un outil précieux pour comprendre comment vos composables sont organisés et pour déboguer les problèmes de layout.

2. *Besoin* : Utilisez le Debugger de la hiérarchie de layout pour inspecter la structure de "CalendarMonth".

3. *Expérimentation* : 

Lancez votre application en mode debug, ouvrez le `Layout Inspector` et activez le `Live Updates`. Ensuite, explorez la structure de `CalendarMonth` dans le `Component Tree`.

4. *Résumé* : En utilisant le Debugger de la hiérarchie de layout, nous pouvons avoir une meilleure compréhension de la structure de nos composables et de la manière dont ils sont organisés les uns par rapport aux autres.

## Exercice 7 : Semantics

1. *Notion à connaitre* : Les Semantics dans Jetpack Compose sont utilisées pour décrire le contenu et l'état des composables d'une manière que d'autres systèmes (comme les lecteurs d'écran pour les personnes malvoyantes) peuvent comprendre.

2. *Besoin* : Ajoutez des sémantiques au composable "CalendarDay" pour indiquer si un jour est sélectionné ou non.

3. *Correction* :

```kotlin
@Composable
fun CalendarDay(day: Int, isSelected: Boolean, onSelectedChange: (Boolean) -> Unit) {
    Box(
        modifier = Modifier
            .padding(4.dp)
            .background(if (isSelected) Color.Green else Color.White)
            .clickable { onSelectedChange(!isSelected) }
            .semantics {
                this.selected = isSelected
                this.text = if (isSelected) "Selected day $day" else "Unselected day $day"
            }
    ) {
        Text(text = day.toString(), color = Color.Black)
    }
}
```

4. *Résumé* : Les sémantiques nous permettent de rendre nos composables accessibles et testables. Elles décrivent le contenu et l'état de nos composables de manière à ce que d'autres systèmes et outils puissent comprendre.

## Exercice 8 : State Hoisting Avancé

1. *Notion à connaitre* : Le State Hoisting peut être appliqué à une échelle plus large pour gérer des états plus complexes. Il peut être utile pour contrôler l'état global d'une application.

2. *Besoin* : Créez un état global de "selectedDays" qui est passé à chaque "CalendarDay" pour contrôler l'état de sélection.

3. *Correction* :

```kotlin
@Composable
fun CalendarMonth(selectedDays: MutableSet<Int>, onSelectedDaysChange: (MutableSet<Int>) -> Unit) {
    Column {
        for (day in 1..30) {
            val isSelected = selectedDays.contains(day)
            CalendarDay(day = day, isSelected = isSelected) { newSelection ->
                if (newSelection) {
                    onSelectedDaysChange(selectedDays + day)
                } else {
                    onSelectedDaysChange(selectedDays - day)
                }
            }
        }
    }
}
```

4. *Résumé* : En utilisant State Hoisting à une échelle plus large, nous avons créé un état global de "selectedDays" qui contrôle l'état de sélection de chaque jour. Cela nous donne un contrôle plus complet sur l'état de notre application.

 ## Exercice 9 : Profiler Avancé

1. *Notion à connaitre* : Le Profileur peut également être utilisé pour identifier les problèmes de performance liés à des mises à jour d'état inutiles.

2. *Besoin* : Utilisez le Profileur pour identifier les recompositions inutiles dans "CalendarMonth" lorsqu'un jour est sélectionné.

3. *Expérimentation* : 

Même démarche que dans l'exercice 5, mais cette fois, surveillez le nombre de recompositions de "CalendarMonth".

4. *Résumé* : En utilisant le Profileur, nous pouvons identifier les problèmes de performance liés à des recompositions inutiles. Cela peut nous aider à optimiser notre code pour éviter ces mises à jour inutiles.

## Exercice 10 : Semantics Avancé

1. *Notion à connaitre* : Les Semantics peuvent être utilisées pour décrire des comportements plus complexes, comme le fait d'interagir avec des éléments cliquables.

2. *Besoin* : Ajoutez des sémantiques au composable "CalendarDay" pour indiquer que c'est un élément cliquable et pour décrire l'action de clic.

3. *Correction* :

```kotlin
@Composable
fun CalendarDay(day: Int, isSelected: Boolean, onSelectedChange: (Boolean) -> Unit) {
    Box(
        modifier = Modifier
            .padding(4.dp)
            .background(if (isSelected) Color.Green else Color.White)
            .clickable { onSelectedChange(!isSelected) }
            .semantics {
                this.selected = isSelected
                this.text = if (isSelected) "Selected day $day" else "Unselected day $day"
                this.role = Role.Button
                this.onClick { onSelectedChange(!isSelected) }
            }
    ) {
        Text(text = day.toString(), color = Color.Black)
    }
}
```

4. *Résumé* : En ajoutant des sémantiques d'interaction, nous avons rendu notre composable "CalendarDay" plus accessible et nous avons décrit de manière plus précise son comportement. Les Semantics peuvent être un outil puissant pour rendre nos applications plus accessibles et plus faciles à tester.


## Conclusion

Au cours de ces exercices, nous avons exploré plusieurs concepts clés de Jetpack Compose et nous avons vu comment ils peuvent être utilisés pour créer une application de calendrier.

Nous avons commencé par apprendre à utiliser le Local State pour gérer l'état mutable dans un composable et nous avons vu comment cet état peut être utilisé pour déclencher une recomposition de l'interface utilisateur. Ensuite, nous avons introduit le concept de State Hoisting, qui permet de déplacer la gestion de l'état vers le composable parent, rendant ainsi nos composables plus réutilisables.

Nous avons ensuite approfondi notre compréhension de la recomposition en Jetpack Compose et avons appris à utiliser le Profileur pour mesurer le nombre de recompositions. Nous avons également exploré le Debugger de la hiérarchie de layout pour comprendre la structure de nos composables.

Enfin, nous avons appris à utiliser les Semantics pour rendre nos composables accessibles et testables, et nous avons vu comment elles peuvent être utilisées pour décrire le contenu et l'état de nos composables à d'autres systèmes.

Pour continuer à vous améliorer dans ces domaines, je recommande de pratiquer en créant des applications plus complexes et en expérimentant avec différents types d'état et de recomposition. Utilisez régulièrement le Profileur et le Debugger pour comprendre le comportement de vos composables et cherchez des moyens d'optimiser vos applications. Enfin, faites de l'accessibilité une priorité dès le début de la conception de vos applications en utilisant les Semantics pour décrire votre interface utilisateur.
