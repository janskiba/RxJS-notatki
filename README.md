# RxJS - kurs

## 1. Podstawy i definicje

### Observable

Observable to strumień, który emituje wartości (pojedyncze lub wiele) dowolnego typu do którego możesz się podłączyć oraz od niego odłączyć. Można na niej używać operatorów. Używa się na nim subscribe() żeby observer mógł nasłuchiwać wyemitowanych wartości

### Observer

Observer to obiekt który obserwuje i odpowiada na powiadomienia dostarczone przez Observable.

Inna definicja to że observer to odbiorca wartości dostarczanych przez Observable.

Robi to takimi metodami jak:

- next() - obsługuje następne wyemitowane wydarzenie
- error() - obsługuje błędy
- complete() - powiadomienie o skompletowaniu observable

### Subscriber

Każdy Observer jest konwertowany na Subscriber czyli Observer który może od-subskrybować od Observable.

### Subscribe

Observer nie otrzyma danych albo observable nie zacznie przesyłać danych dopóki nie zasubskrybuje do Observable. Na tej podstawie rozróżnia się

- hot observable- czyli taka która nie czeka na subscribe i może emitować wartości mimo że nikt do niej nie zasubskrybował a każdy subscriber dzieli strumień danych
- cold observable - czyli taka która nie emituje wartości dopóki się do niej nie zasubskrybuje a każdy subscriber ma swój strumie danych

### Unsubscribe

Żeby przestać słuchać wartości emitowanych przez observable wywołujemy unsubscribe na subskrypcji, co zapobiega wyciekowi danych w aplikacji. Można to zrobić w następujący sposób:

- wywołując metodę `complete()`
- używając operatorów które kończą się automatycznie
- poprzesz wyrzucenie błędu
- wywołując `unsubscribe()`; należy przypisać do zmiennej observable na której wywołujemy subscribe, która zwróci typ subscribe i użyć na tej zmiennej metody unsubscribe

### Shedulers

Sposób na kontrolowanie strategii czasowej używanej do wykonywania zadań w RxJS. Używamy jej kiedy chcemy wywołać synchroniczną Observable asynchronicznie. np prost of czy from.

Należy do nic np scheduler _delay_

```tsx
import { of, asyncScheduler, asapScheduler } from 'rxjs';
import { observeOn } from 'rxjs/operators';
const observableOne = of('observableOne');
const observableTwo = of('observableTwo');

console.log(1);
observableTwo.pipe(observeOn(asyncScheduler)).subscribe(console.log); // asapScheduler performs macro task
observableOne.pipe(observeOn(asapScheduler)).subscribe(console.log); // asapScheduler performs micro task -
// runs first, before macro tasks such as setTimeout()
console.log(2);

/*
  console output:
    1
    2
    observableOne
    observableTwo
*/
```

### Tworzenie Observable - of() i from() creation functions

W rxjs można stworzyć observable za pomocą:

- konstruktora

```jsx
const observable = new Observable((subscriber) => {
  subscriber.next(1);
  subscriber.next(2);
  subscriber.next(3);
});
```

- funkcja of(), która tworzy observable i emituje elementy przekazane jako parametry a później się kończy - nie ma potrzeby od subskrybować

```jsx
of(1, 2, 3);
```

- funkcja from(), która tworzy observable z tablicy

```jsx
from([1, 2, 3]);
```

## 2. RxJS operators
