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
