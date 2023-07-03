# Exercice 3

## Objectif

créer cette interface
code exemple :

```kotlin

@Composable
fun FightPhoto() {
    Box(
        modifier = Modifier
            .fillMaxSize()
            .paint(
                painterResource(id = R.drawable.ic_wintery_sunburst),
                contentScale = ContentScale.FillBounds
            )

    ) {
        Column(
            modifier = Modifier
                .fillMaxWidth()
                .align(Alignment.TopCenter)
        ) {

            Row(
                horizontalArrangement = Arrangement.spacedBy(40.dp),
                modifier = Modifier
                    .fillMaxWidth()
                    .padding(start = 20.dp, end = 20.dp, top = 50.dp)
            ) {

                Column(
                    Modifier
                        .fillMaxWidth()
                        .weight(1F)
                ) {
                    LinearProgressIndicator(
                        progress = 0.1f,
                        color = Color.Red,
                        modifier = Modifier
                            .fillMaxWidth()
                            .height(30.dp)

                    )
                    Text(
                        text = "Ios",
                        color = Color.Gray,
                        style = MaterialTheme.typography.body1.copy(fontWeight = FontWeight.Bold)
                    )
                }
                Column(
                    Modifier
                        .fillMaxWidth()
                        .weight(1F)
                ) {
                    LinearProgressIndicator(
                        progress = 0.8f,
                        color = Color.Green,
                        modifier = Modifier
                            .fillMaxWidth()
                            .height(30.dp)

                    )
                    Text(
                        text = "Android",
                        color = Color.Gray,
                        style = MaterialTheme.typography.body1.copy(fontWeight = FontWeight.Bold)
                    )
                }

            }
            Text(
                textAlign = TextAlign.Center,
                text = "FIGHT!",
                modifier = Modifier
                    .fillMaxWidth()
                    .padding(top = 30.dp),
                color = Color.Red,
                style = MaterialTheme.typography.h1.copy(fontWeight = FontWeight.Bold)
            )
        }

        Image(
            painter = painterResource(id = R.drawable.ic_swift),
            contentDescription = "",
            modifier = Modifier
                .size(200.dp)
                .align(Alignment.CenterStart),

            )

        Image(
            painter = painterResource(id = R.drawable.ic_android),
            contentDescription = "",
            modifier = Modifier
                .size(200.dp)
                .align(Alignment.BottomEnd),
        )


    }
}


@Preview(device = PIXEL_3_XL, showSystemUi = true)
@Composable
fun FightPhotoPreview() {
    Surface {
        FightPhoto()
    }

}
```
