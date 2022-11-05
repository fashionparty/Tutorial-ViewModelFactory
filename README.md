## ViewModelFactory

Tworząc instację VM przy pomocy `ViewModelsProvider` nie mamy możliwości przekazania żadnych argumentów do konstruktora:


*MainViewModel.kt*
```
class MainViewModel(argument: String): ViewModel()
```


*MainActivity.kt*
```
val vm = ViewModelProvider(this)[MainViewModel::class.java]
```


Należy w takim wypadku użyć klasy dziedziczącej po `ViewModelProvider.Factory` i jej przekazać argument:


*MainViewModelFactory.kt*
```
@Suppress("UNCHECKED_CAST")
class MainViewModelFactory(private var argument: String): ViewModelProvider.Factory {

    override fun <T : ViewModel> create(modelClass: Class<T>): T {
        return if(modelClass.isAssignableFrom(MainViewModel::class.java)) {
            MainViewModel(argument) as T
        } else throw IllegalArgumentException("ViewModel not found.")
    }
}
```


*MainActivity.kt*
```
val vmf = MainViewModelFactory(argument = "Mój argument")
val vm = ViewModelProvider(this, vmf)[MainViewModel::class.java]
```
        


