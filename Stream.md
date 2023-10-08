
```dart
import 'dart:async';
import 'dart:convert';

  
const duration = Duration(milliseconds: 500);
final computation = (int computationCount) => computationCount;

void main(List<String> arguments) async {

final myStream = Stream<int>.periodic(duration, computation);

/// Возвращает только первые 5 событий стрима после того как на него подписались.

final onlyFirst5Events = myStream.take(5);

/// Подписывается на стрим и получает первое событие.

/// Отсюда, если стрим не broadcast, то получить снова first или last нельзя!

final myStream1 = Stream<int>.periodic(duration, computation);

final firstEvent = await myStream1.first;

print(firstEvent); // 0

/// Подписывается на стрим и получает последнее событие.

/// Поскольку стрим [myStream] не имеет окончания, то что бы получить последнее

/// значение нужно взять первые 5 его значений.

final myStream2 = Stream<int>.periodic(duration, computation).take(5);

final lastEvent = await myStream2.last;

print(lastEvent); // 5


/// Подписывается на стрим и получает только одно событие.

/// Если событий больше одного, то будет выброшено исключение.

final myStream3 = Stream<int>.periodic(duration, computation).take(1);

final single = await myStream3.single;

print(single); // 0 
  

/// Подписывается на стрим и происходит проверка значений стрима по функции test.

/// Возвращает true если любое из значений стрима будет true в функции test.

/// Иначе возвращает false по окончанию стрима.

final myStream4 = Stream<int>.periodic(duration, computation);

final test = await myStream4.any((element) => element == 4);

print(test);
  

/// Превращает стрим в broadcast стрим, не изменяя исходный.

final myStream5 = Stream<int>.periodic(duration, computation);

myStream5.asBroadcastStream()

..listen(print)

..listen(print);

myStream5.listen(print); // Bad state: Stream has already been listened to.  

/// Создает Future<bool> который отдает результат true в случае наличия данного объекта в стриме.

/// Либо false в случае отсутствия и стрим закончился.

final myStream6 = Stream<int>.periodic(duration, computation);

final result = await myStream6.contains(10);

print(result);

/// Удаляет из текущего стрима все повторяющиеся значения. Возвращает новый стрим.

/// Функцию сравнения можно переопределить.

final myStream7 = Stream<int>.periodic(duration, computation);

final distinctStream = myStream7.distinct();


/// Подписывается на стрим и по окончанию стрима возвращает значение

/// заданное в параметре futureValue метода drain.

final myStream8 = Stream<int>.periodic(duration, computation).take(5);

final result = await myStream8.drain(100);

print(result);

  
/// Позволяет получить значение по порядковому индексу в стриме.

/// Если указанный индекс не существует, то будет выброшено исключение.

/// Аналогично работе с коллекцией.

final myStream9 = Stream<int>.periodic(duration, computation).take(3);

final result = await myStream9.elementAt(4);

print(result);

  
/// Позволяет обработать стрим где то за его пределами при помощи объекта

/// StreamConsumer.

final myStream10 = Stream<int>.periodic(duration, computation).take(5);

final result = await myStream10.pipe(MyStreamConsumer());

print(result);

  
/// Позволяет трансформировать/преобразовать исходный стрим в новый.

/// Для этого создается объект StreamTransformer и переопределяется метод

/// bind, который можно представить в виде асинхронного генератора возвращающего

/// новый стрим.

final myStream11 = Stream<int>.periodic(duration, computation).take(5);

final result = myStream11.transform(MyStreamTransformer());

result.forEach(print);

  

/// Расширяет стрим превращая одно значение ивента в iterable и эмитит их вместе.

final myStream12 = Stream<int>.periodic(duration, computation).take(5);
final newStream = myStream12.expand((element) => [element, element * 2]);

newStream.forEach(print);
}

  

class MyStreamConsumer<String> implements StreamConsumer<int> {

@override
Future addStream(Stream<int> stream) {
return stream.forEach(print);
}

  

@override
Future close() {
return Future.value('closed');

}
}

  

class MyStreamTransformer extends StreamTransformerBase<int, String> {

@override
Stream<String> bind(Stream<int> stream) async* {
await for (var value in stream) {
yield 'Current value is $value';
}
}
}

```


---  
### Материалы:
- https://pub.dev/packages/stream_transform
---
Теги: #ready 
