# LEVEL 3 : Exercices

## Exercice 1: Composition Locale

1. **Notion à connaitre**: La composition locale est une fonctionnalité clé de Jetpack Compose qui permet aux composants de partager des données au sein d'une composition. Cela se fait généralement à l'aide de la bibliothèque `compose-runtime`.

2. **Besoin**: Créez une fonction Composable `AppointmentCalendar` qui contient une variable locale, `selectedDate`, qui sera mise à jour lorsque l'utilisateur sélectionne une date du calendrier.

3. **Correction**:
```kotlin
import androidx.compose.runtime.*
import androidx.compose.ui.graphics.Color

@Composable
fun AppointmentCalendar() {
    var selectedDate by remember { mutableStateOf("") }

    Button(onClick = { selectedDate = "2023-06-23" }, colors = ButtonDefaults.buttonColors(backgroundColor = Color.Red)) {
        Text("Select Today")
    }
    Text("Selected Date: $selectedDate")
}
```

4. **Résumé**: Dans cet exercice, nous avons utilisé la composition locale pour gérer l'état d'une variable au sein de notre fonction Composable. Grâce à cela, nous pouvons rendre notre application plus interactive et plus dynamique.

---

## Exercice 2: Recomposition Stability / Skippable

1. **Notion à connaître**: La recomposition est le processus par lequel Compose met à jour l'interface utilisateur pour refléter l'état actuel de l'application. La stabilité de la recomposition est une caractéristique importante pour s'assurer que la recomposition n'a lieu que lorsque nécessaire. L'attribut `@Stable` est utilisé pour indiquer qu'une classe ou une fonction peut être recomposée de manière efficace.

2. **Besoin**: Améliorez la fonction `AppointmentCalendar` pour qu'elle utilise une classe `@Stable` appelée `Appointment` qui contient les détails de chaque rendez-vous, comme la date et l'heure.

3. **Correction**:
```kotlin
@Stable
data class Appointment(val date: String, val time: String)

@Composable
fun AppointmentCalendar() {
    var selectedAppointment by remember { mutableStateOf(Appointment("","")) }

    Button(onClick = { selectedAppointment = Appointment("2023-06-23", "14:00") },
            colors = ButtonDefaults.buttonColors(backgroundColor = Color.Red)) {
        Text("Select Appointment")
    }
    Text("Selected Appointment: ${selectedAppointment.date} at ${selectedAppointment.time}")
}
```

4. **Résumé**: Nous avons exploré la stabilité de la recomposition en utilisant l'annotation `@Stable`. Cela nous a permis d'optimiser le processus de recomposition en n'actualisant que lorsque c'est nécessaire.

---

## Exercice 3: Layouts intrinsectSize

1. **Notion à connaître**: En Jetpack Compose, les layouts prennent des décisions basées sur l'`IntrinsicSize` qui est la taille idéale pour un composant, sans restriction de taille externe. 

2. **Besoin**: Ajoutez un composant Composable `AppointmentCard` qui affiche les détails de chaque rendez-vous dans une carte. Cette carte doit avoir une taille intrinsèque minimale afin de s'assurer qu'elle est suffisamment grande pour afficher tous les détails de l'appointment.

3. **Correction**:
```kotlin
@

Composable
fun AppointmentCard(appointment: Appointment) {
    Card(
        modifier = Modifier
            .fillMaxWidth()
            .height(IntrinsicSize.Min),
        shape = RoundedCornerShape(8.dp),
        elevation = 4.dp
    ) {
        Column(modifier = Modifier.padding(16.dp)) {
            Text(text = "Appointment Details")
            Text(text = "Date: ${appointment.date}")
            Text(text = "Time: ${appointment.time}")
        }
    }
}

@Composable
fun AppointmentCalendar() {
    var selectedAppointment by remember { mutableStateOf(Appointment("", "")) }

    Button(onClick = { selectedAppointment = Appointment("2023-06-23", "14:00") },
            colors = ButtonDefaults.buttonColors(backgroundColor = Color.Red)) {
        Text("Select Appointment")
    }

    if (selectedAppointment.date.isNotEmpty()) {
        AppointmentCard(appointment = selectedAppointment)
    }
}
```

4. **Résumé**: Nous avons travaillé sur l'`IntrinsicSize` en Jetpack Compose. Nous avons créé une `AppointmentCard` qui utilise `IntrinsicSize.Min` pour s'assurer que la carte est toujours suffisamment grande pour afficher tous les détails de l'appointment.

---

## Exercice 4: Custom Layouts

1. **Notion à connaître**: Les "custom layouts" sont des dispositions que vous créez pour adapter les Composables à vos propres besoins. Vous pouvez les définir en utilisant le modificateur `Layout` et en spécifiant comment les enfants doivent être disposés.

2. **Besoin**: Créez un `WeekView` Composable qui affiche les sept jours de la semaine horizontalement. Utilisez un custom layout pour placer chaque jour de la semaine côte à côte.

3. **Correction**:
```kotlin
@Composable
fun WeekView(weekDays: List<String>) {
    Layout(
        content = { weekDays.forEach { Text(it, modifier = Modifier.padding(4.dp)) } }
    ) { measurables, constraints ->
        val placeables = measurables.map { measurable ->
            measurable.measure(constraints)
        }

        layout(constraints.maxWidth, constraints.maxHeight) {
            var xPosition = 0

            placeables.forEach { placeable ->
                placeable.placeRelative(x = xPosition, y = 0)
                xPosition += placeable.width
            }
        }
    }
}

@Composable
fun AppointmentCalendar() {
    val weekDays = listOf("Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat")
    var selectedAppointment by remember { mutableStateOf(Appointment("", "")) }

    WeekView(weekDays)

    Button(onClick = { selectedAppointment = Appointment("2023-06-23", "14:00") },
            colors = ButtonDefaults.buttonColors(backgroundColor = Color.Red)) {
        Text("Select Appointment")
    }

    if (selectedAppointment.date.isNotEmpty()) {
        AppointmentCard(appointment = selectedAppointment)
    }
}
```

4. **Résumé**: Dans cet exercice, nous avons appris à créer un custom layout. Nous avons utilisé le modificateur `Layout` pour créer un `WeekView` qui affiche les jours de la semaine côte à côte.

---

## Exercice 5: Custom Animations

1. **Notion à connaître**: Les animations dans Jetpack Compose peuvent être personnalisées pour rendre l'expérience utilisateur plus interactive et plus agréable. Vous pouvez utiliser `animateColorAsState`, `animateDpAsState`, etc. pour créer des animations de couleur, de taille, de position et plus encore.

2. **Besoin**: Ajoutez une animation à la `AppointmentCard` qui change la couleur de fond de la carte lorsque le rendez-vous est sélectionné.

3. **Correction**:
```kotlin
@Composable
fun AppointmentCard(appointment: Appointment, isSelected: Boolean) {
    val backgroundColor by animateColorAsState(if (isSelected) Color.LightGray else Color.White)

    Card(
        modifier = Modifier
            .fillMaxWidth()
            .height(IntrinsicSize.Min),
        backgroundColor = backgroundColor,
        shape = RoundedCornerShape(8.dp),
        elevation = 4.dp
    ) {
        Column(modifier = Modifier.padding(16.dp)) {
            Text(text = "Appointment Details")
            Text(text = "Date: ${appointment.date}")
            Text(text = "Time: ${appointment.time}")
        }
    }
}

@Composable
fun AppointmentCalendar() {
    var selectedAppointment by remember { mutableStateOf(Appointment("", "")) }

    Button(onClick = { selectedAppointment = Appointment("2023-06-23", "14:00") },
            colors = ButtonDefaults.buttonColors(backgroundColor = Color.Red)) {
        Text("Select

 Appointment")
    }

    if (selectedAppointment.date.isNotEmpty()) {
        AppointmentCard(appointment = selectedAppointment, isSelected = true)
    }
}
```

4. **Résumé**: Nous avons découvert comment personnaliser les animations dans Jetpack Compose. Nous avons ajouté une animation à notre `AppointmentCard` pour changer la couleur de fond lorsqu'un rendez-vous est sélectionné.

---

## Exercice 6: Refactoring pour la Composition Locale

1. **Notion à connaître**: À mesure que notre application se complexifie, il est crucial de garder notre code organisé et lisible. L'une des façons de le faire est de refactoriser notre code pour améliorer la composition locale.

2. **Besoin**: Refactorisez le code de `AppointmentCalendar` pour extraire le bouton de sélection et `WeekView` dans leurs propres composables.

3. **Correction**:
```kotlin
@Composable
fun SelectButton(selectedAppointment: MutableState<Appointment>) {
    Button(onClick = { selectedAppointment.value = Appointment("2023-06-23", "14:00") },
            colors = ButtonDefaults.buttonColors(backgroundColor = Color.Red)) {
        Text("Select Appointment")
    }
}

@Composable
fun AppointmentCalendar() {
    val weekDays = listOf("Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat")
    var selectedAppointment by remember { mutableStateOf(Appointment("", "")) }

    WeekView(weekDays)
    SelectButton(selectedAppointment)

    if (selectedAppointment.date.isNotEmpty()) {
        AppointmentCard(appointment = selectedAppointment, isSelected = true)
    }
}
```

4. **Résumé**: Dans cet exercice, nous avons refactorisé notre code pour améliorer la composition locale. En extrayant des parties de notre code en leurs propres composables, nous avons amélioré la lisibilité et la maintenabilité de notre code.

---

## Exercice 7: Raffinement de la Recomposition Stability

1. **Notion à connaître**: L'utilisation correcte des annotations `@Stable` est cruciale pour garantir des performances optimales. Un mauvais usage peut entraîner des recompositions inutiles et nuire aux performances.

2. **Besoin**: Raffinez le Composable `AppointmentCard` pour assurer que les recompositions sont minimisées. L'animation de couleur de fond ne doit se déclencher que lorsque l'état `isSelected` change.

3. **Correction**:
```kotlin
@Stable
class SelectedAppointment(val appointment: Appointment) {
    var isSelected by mutableStateOf(false)
}

@Composable
fun AppointmentCard(selectedAppointment: SelectedAppointment) {
    val backgroundColor by animateColorAsState(if (selectedAppointment.isSelected) Color.LightGray else Color.White)

    Card(
        modifier = Modifier
            .fillMaxWidth()
            .height(IntrinsicSize.Min),
        backgroundColor = backgroundColor,
        shape = RoundedCornerShape(8.dp),
        elevation = 4.dp
    ) {
        Column(modifier = Modifier.padding(16.dp)) {
            Text(text = "Appointment Details")
            Text(text = "Date: ${selectedAppointment.appointment.date}")
            Text(text = "Time: ${selectedAppointment.appointment.time}")
        }
    }
}

@Composable
fun AppointmentCalendar() {
    val weekDays = listOf("Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat")
    var selectedAppointment = remember { SelectedAppointment(Appointment("", "")) }

    WeekView(weekDays)
    SelectButton(selectedAppointment)

    if (selectedAppointment.appointment.date.isNotEmpty()) {
        AppointmentCard(selectedAppointment)
    }
}
```

4. **Résumé**: Nous avons raffiné notre utilisation de `@Stable` pour minimiser les recompositions inutiles. Cela nous aide à garantir que notre application reste performante même en cas de changements d'état.

---

## Exercice 8: Expansion du Layouts intrinsectSize

1. **Notion à connaître**: Les layouts intrinsèques sont extrêmement utiles lorsque nous avons besoin de plus de contrôle sur la taille et la disposition de nos Composables.

2. **Besoin**: Mettez à jour le `WeekView` pour afficher non seulement le nom du jour, mais aussi un nombre variable de `AppointmentCard`. Le layout doit s'adapter pour accommoder le nombre de `AppointmentCard`.

3. **Correction**:
```kotlin
@Composable
fun DayColumn(day: String, appointments: List<Appointment>) {
    Column {
        Text(day, modifier = Modifier.padding(4.dp))
        appointments.forEach { 
            AppointmentCard(SelectedAppointment(it), isSelected = true)
        }
    }
}

@Composable
fun WeekView(weekDays: List<String>, weekAppointments: Map<String, List<Appointment>>) {
    Row(
        modifier = Modifier.fillMaxWidth(),
        horizontalArrangement = Arrangement.SpaceBetween
    ) {
        weekDays.forEach { day ->
            DayColumn(day, weekAppointments[day] ?: emptyList())
        }
    }
}
```

4. **Résumé**: Nous avons élargi notre utilisation des layouts intrinsèques pour adapter notre layout à un nombre variable de `AppointmentCard`. Cela démontre la flexibilité des layouts intrinsèques.

---

## Exercice 9: Complexification des Custom Animations

1. **Notion à connaître**: Jetpack Compose permet des

 animations plus complexes en utilisant `Transition`.

2. **Besoin**: Animer l'affichage des `AppointmentCard` de sorte qu'elles se déplacent de bas en haut lorsqu'elles sont ajoutées à l'écran.

3. **Correction**:
```kotlin
@Composable
fun AnimatedAppointmentCard(selectedAppointment: SelectedAppointment) {
    val transition = updateTransition(selectedAppointment.isSelected, label = "Appointment Card")
    val translateY by transition.animateDp(label = "Slide Animation") { isSelected ->
        if (isSelected) 0.dp else 200.dp
    }

    AppointmentCard(selectedAppointment).offset(y = translateY)
}

@Composable
fun DayColumn(day: String, appointments: List<Appointment>) {
    Column {
        Text(day, modifier = Modifier.padding(4.dp))
        appointments.forEach { 
            AnimatedAppointmentCard(SelectedAppointment(it), isSelected = true)
        }
    }
}
```

4. **Résumé**: Nous avons utilisé `Transition` pour créer une animation plus complexe pour nos `AppointmentCard`. Cela nous a permis de rendre l'application plus dynamique et interactive.

---

## Exercice 10: Recomposition Skippable et état partagé

1. **Notion à connaître**: L'utilisation correcte de `remember` et `mutableStateOf` est essentielle pour gérer l'état partagé de manière efficace et minimiser les recompositions.

2. **Besoin précis**: Ajoutez la possibilité de sélectionner un jour spécifique dans `WeekView`. Partagez cet état avec `SelectButton` pour mettre à jour l'état de sélection.

3. **Correction**:
```kotlin
@Composable
fun SelectButton(selectedDay: MutableState<String>) {
    Button(onClick = { selectedDay.value = "Mon" },
            colors = ButtonDefaults.buttonColors(backgroundColor = Color.Red)) {
        Text("Select Monday")
    }
}

@Composable
fun DayColumn(day: String, appointments: List<Appointment>, selectedDay: MutableState<String>) {
    val isSelected = remember { mutableStateOf(false) }
    isSelected.value = day == selectedDay.value

    val backgroundColor by animateColorAsState(if (isSelected.value) Color.LightGray else Color.White)

    Column(modifier = Modifier.background(color = backgroundColor)) {
        Text(day, modifier = Modifier.padding(4.dp))
        appointments.forEach { 
            AnimatedAppointmentCard(SelectedAppointment(it), isSelected = isSelected.value)
        }
    }
}

@Composable
fun AppointmentCalendar() {
    val weekDays = listOf("Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat")
    val weekAppointments = mutableMapOf<String, List<Appointment>>()
    weekDays.forEach { day ->
        weekAppointments[day] = listOf(Appointment("$day Appointment", "14:00"))
    }

    var selectedDay = remember { mutableStateOf("") }
    WeekView(weekDays, weekAppointments, selectedDay)
    SelectButton(selectedDay)
}
```

4. **Résumé**: Nous avons utilisé `remember` et `mutableStateOf` pour gérer l'état partagé entre différents composables, minimisant ainsi les recompositions inutiles. Nous avons également utilisé les animations pour indiquer visuellement le changement d'état.

---

## Exercice 11: Layouts intrinsectSize et Custom Animations combinés

1. **Notion à connaître**: Combinaison des layouts intrinsèques et des animations personnalisées. L'utilisation combinée de ces deux concepts peut aider à créer des interfaces utilisateur dynamiques et réactives.

2. **Besoin**: Modifiez le `WeekView` pour que la hauteur de chaque `DayColumn` soit animée pour correspondre au nombre d'`AppointmentCard` présentes pour chaque jour. Le `DayColumn` doit se développer et se rétracter de manière fluide lors de l'ajout ou de la suppression d'un rendez-vous.

3. **Correction**:
```kotlin
@Composable
fun DayColumn(day: String, appointments: List<Appointment>, selectedDay: MutableState<String>) {
    val isSelected = remember { mutableStateOf(false) }
    isSelected.value = day == selectedDay.value

    val backgroundColor by animateColorAsState(if (isSelected.value) Color.LightGray else Color.White)

    Column(
        modifier = Modifier
            .background(color = backgroundColor)
            .animateContentSize(animationSpec = tween(durationMillis = 500))
    ) {
        Text(day, modifier = Modifier.padding(4.dp))
        appointments.forEach { 
            AnimatedAppointmentCard(SelectedAppointment(it), isSelected = isSelected.value)
        }
    }
}
```

4. **Résumé**: Nous avons combiné les layouts intrinsèques et les animations personnalisées pour créer un `DayColumn` dynamique qui s'étend et se rétracte en fonction du nombre de `AppointmentCard`. Cela a permis d'améliorer l'expérience utilisateur en rendant l'interface plus interactive.

---

## Exercice 12: Custom Layouts avec Recomposition Stability et état partagé

1. **Notion à connaître**: Utiliser les custom layouts pour organiser l'état partagé de manière efficace et maintenir une recomposition stable est un défi intéressant. Cela nécessite une compréhension approfondie des deux concepts pour être réalisé correctement.

2. **Besoin**: Ajoutez un `MonthView` Composable qui affiche plusieurs `WeekView`. Utilisez un custom layout pour organiser ces `WeekView`. De plus, assurez-vous que la recomposition reste stable et que l'état partagé est géré efficacement.

3. **Correction**:
```kotlin
@Stable
class MonthState {
    var selectedDay by mutableStateOf("")
    var appointments by mutableStateOf(mutableMapOf<String, List<Appointment>>())
}

@Composable
fun MonthView(monthState: MonthState) {
    Column {
        monthState.appointments.keys.forEach { week ->
            WeekView(monthState.appointments[week] ?: emptyList(), monthState.selectedDay)
        }
    }
}

@Composable
fun AppointmentCalendar() {
    val monthState = remember { MonthState() }
    monthState.appointments = generateAppointmentsForMonth()

    MonthView(monthState)
    SelectButton(monthState.selectedDay)
}
```

4. **Résumé**: Nous avons ajouté un `MonthView` Composable qui affiche plusieurs `WeekView` en utilisant un custom layout. Nous avons également géré l'état partagé et la stabilité de la recomposition de manière efficace.

---

## Exercice 13: Complexification des Custom Layouts et des Custom Animations

1. **Notion à connaître**: L'ajout de complexité aux custom layouts et aux animations

 personnalisées peut aider à créer des interfaces utilisateur encore plus dynamiques et interactives.

2. **Besoin**: Modifiez le custom layout de `MonthView` pour afficher les `WeekView` dans une grille, avec des animations pour déplacer les `WeekView` lors de la sélection d'un jour spécifique.

3. **Correction**:
```kotlin
@OptIn(ExperimentalAnimationApi::class)
@Composable
fun MonthView(monthState: MonthState) {
    val transition = updateTransition(monthState.selectedDay, label = "Week Shift")
    val weekOffset by transition.animateInt(label = "Week Offset") { weekNumber ->
        when (weekNumber) {
            "1" -> 0
            "2" -> 1
            "3" -> 2
            "4" -> 3
            else -> 0
        }
    }

    Row(modifier = Modifier.fillMaxWidth()) {
        monthState.appointments.keys.forEach { week ->
            AnimatedVisibility(
                visible = monthState.selectedDay == week,
                enter = slideInHorizontally(initialOffsetX = { it * weekOffset }),
                exit = slideOutHorizontally(targetOffsetX = { -it * weekOffset })
            ) {
                WeekView(monthState.appointments[week] ?: emptyList(), monthState.selectedDay)
            }
        }
    }
}
```

4. **Résumé**: En ajoutant de la complexité à nos layouts et animations personnalisés, nous avons créé une interface utilisateur dynamique où les `WeekView` se déplacent en fonction du jour sélectionné. Cela a permis d'augmenter l'interactivité et l'intuitivité de notre application.

---

## Exercice 14: Intégration de toutes les notions avec gestion des erreurs

1. **Notion à connaître**: L'intégration des notions précédemment étudiées, avec l'ajout d'une gestion d'erreurs. Il est essentiel de pouvoir gérer les erreurs de manière élégante et informative pour les utilisateurs.

2. **Besoin**: Intégrez toutes les notions que nous avons étudiées jusqu'à présent et ajoutez une gestion d'erreurs. Si un jour sélectionné ne contient pas d'`AppointmentCard`, affichez un message d'erreur à l'utilisateur.

3. **Correction**:
```kotlin
@Composable
fun MonthView(monthState: MonthState) {
    val transition = updateTransition(monthState.selectedDay, label = "Week Shift")
    val weekOffset by transition.animateInt(label = "Week Offset") { weekNumber ->
        when (weekNumber) {
            "1" -> 0
            "2" -> 1
            "3" -> 2
            "4" -> 3
            else -> 0
        }
    }

    Row(modifier = Modifier.fillMaxWidth()) {
        monthState.appointments.keys.forEach { week ->
            val appointments = monthState.appointments[week] ?: emptyList()
            AnimatedVisibility(
                visible = monthState.selectedDay == week,
                enter = slideInHorizontally(initialOffsetX = { it * weekOffset }),
                exit = slideOutHorizontally(targetOffsetX = { -it * weekOffset })
            ) {
                if (appointments.isEmpty()) {
                    Text("No appointments available for $week")
                } else {
                    WeekView(appointments, monthState.selectedDay)
                }
            }
        }
    }
}
```

4. **Résumé**: En intégrant toutes les notions que nous avons étudiées jusqu'à présent et en ajoutant une gestion d'erreurs, nous avons créé une application robuste et résiliente. Cette approche assure que notre application est non seulement interactive et intuitive, mais également capable de fournir un feedback utile à l'utilisateur.

---

## Exercice 15: Intégration complète avec gestion d'état complexe et animations

1. **Notion à connaître**: La gestion d'état complexe et des animations dans un contexte d'application complète. Cette notion réunit tout ce que nous avons appris jusqu'à présent pour créer une application entièrement fonctionnelle.

2. **Besoin**: Créez un système de sélection de semaine dans lequel, lorsque l'utilisateur sélectionne une semaine, les autres semaines disparaissent et l'utilisateur ne voit que la semaine sélectionnée. De plus, chaque jour de la semaine doit être sélectionnable, et lorsqu'un jour est sélectionné, l'heure des rendez-vous de ce jour doit être modifiable. Toutes ces modifications doivent être animées.

3. **Correction**:
```kotlin
@Composable
fun WeekSelector(monthState: MonthState) {
    Row {
        monthState.appointments.keys.forEach { week ->
            Button(onClick = { monthState.selectedDay = week }) {
                Text(week)
            }
        }
    }
}

@Composable
fun AppointmentEditor(appointment: Appointment, onTimeChange: (String) -> Unit) {
    val time = remember { mutableStateOf(appointment.time) }
    TextField(value = time.value, onValueChange = { newValue ->
        time.value = newValue
        onTimeChange(newValue)
    })
}

@Composable
fun DayColumn(appointments: List<Appointment>, selectedDay: MutableState<String>, onTimeChange: (Appointment, String) -> Unit) {
    appointments.forEach { appointment ->
        if (selectedDay.value == appointment.day) {
            AppointmentEditor(appointment, onTimeChange = { newTime ->
                onTimeChange(appointment, newTime)
            })
        }
    }
}

@Composable
fun MonthView(monthState: MonthState, onTimeChange: (Appointment, String) -> Unit) {
    val transition = updateTransition(monthState.selectedDay, label = "Week Shift")
    val weekOffset by transition.animateInt(label = "Week Offset") { weekNumber ->
        when (weekNumber) {
            "1" -> 0
            "2" -> 1
            "3" -> 2
            "4" -> 3
            else -> 0
        }
    }

    Row(modifier = Modifier.fillMaxWidth()) {
        monthState.appointments.keys.forEach { week ->
            val appointments = monthState.appointments[week] ?: emptyList()
            AnimatedVisibility(
                visible = monthState.selectedDay == week,
                enter = slideInHorizontally(initialOffsetX = { it * weekOffset }),
                exit = slideOutHorizontally(targetOffsetX = { -it * weekOffset })
            ) {
                if (appointments.isEmpty()) {
                    Text("No appointments available for $week")
                } else {
                    WeekView(appointments, monthState.selectedDay, onTimeChange)
                }
            }
        }
    }
}

@Composable
fun AppointmentCalendar() {
    val monthState = remember { MonthState() }
    monthState.appointments = generateAppointmentsForMonth()

    WeekSelector(monthState)
    MonthView(monthState, onTimeChange = { appointment, newTime ->
        appointment.time = newTime
    })
}
```

4. **Résumé**: En combinant toutes les notions précédemment étudiées et en ajoutant des interactions et animations complexes, nous avons créé une application de calendrier entièrement interactive. Cette application permet de sélectionner des semaines, de sélectionner des jours spécifiques dans ces semaines, et d'éditer les heures des rendez-vous pour chaque jour. Tout cela est accompagné d'animations fluides et intuitives pour rendre l'expérience utilisateur aussi agréable que possible.
