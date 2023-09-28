# Exercice 3

## Objectif

Créer cette interface.

Les ressources sont disponibles dans le dossier /ressources

![Exemple d'interface](img/fight.png)

code exemple :

```kotlin

@Composable
fun FightPhoto() {
    Box() {
        // Dessinez l'écran ici
}


@Preview(device = PIXEL_3_XL, showSystemUi = true)
@Composable
fun FightPhotoPreview() {
    Surface {
        FightPhoto()
    }

}
```
