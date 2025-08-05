@Composable
fun MainScreen(viewModel: VitalsViewModel) {
    val vitalsList by viewModel.vitalsList.collectAsState()
    var showDialog by remember { mutableStateOf(false) }

    Scaffold(
        floatingActionButton = {
            FloatingActionButton(onClick = { showDialog = true }) {
                Icon(Icons.Default.Add, contentDescription = "Add Vitals")
            }
        }
    ) {
        LazyColumn {
            items(vitalsList) { vitals ->
                Text("BP: ${vitals.systolic}/${vitals.diastolic} | HR: ${vitals.heartRate} bpm | Weight: ${vitals.weight} kg | Kicks: ${vitals.babyKicks}")
            }
        }

        if (showDialog) {
            AddVitalsDialog(
                onDismiss = { showDialog = false },
                onSubmit = { vitals ->
                    viewModel.addVitals(vitals)
                    showDialog = false
                }
            )
        }
    }
}
