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
