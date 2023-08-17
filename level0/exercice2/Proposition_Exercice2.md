# Proposition pour l'exercice 2

```kotlin
@Composable
fun CardPhoto() {
    Card(
        modifier = Modifier.shadow(
            elevation = 4.dp,
            shape = RoundedCornerShape(10.dp)
        )
    ) {

        Column(
            modifier = Modifier
                .padding(16.dp)
                .fillMaxWidth(),
            verticalArrangement = Arrangement.spacedBy(16.dp)
        ) {
            Row(
                horizontalArrangement = Arrangement.spacedBy(16.dp)
            ) {
                Image(
                    painter = painterResource(id = R.drawable.avatar),
                    modifier = Modifier
                        .size(48.dp)
                        .clip(CircleShape),
                    contentScale = ContentScale.Crop,
                    contentDescription = "avatar"
                )

                Column {
                    Text(
                        "name",
                        style = MaterialTheme.typography.body1.copy(fontWeight = FontWeight.Medium)
                    )
                    Text("12/03/2023", style = MaterialTheme.typography.caption)
                }
            }
            Image(
                painter = painterResource(id = R.drawable.pop_corn),
                contentDescription = "main image",
                modifier = Modifier
                    .fillMaxWidth()
                    .height(250.dp),
                contentScale = ContentScale.Crop
            )
        }
    }
}


@Preview
@Composable
fun CardPhotoPreview() {
    Column(modifier = Modifier.padding(20.dp)) {
        CardPhoto()
    }

}
```
