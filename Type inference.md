Проверка типов основана на статической проверке типов силами статического анализатора и проверкой типов в рантайме, пример это тайпкастинг.
Использование типов обязательно, указание их не является обязательным, поскольку есть механизм Type inference. 

Type inference или выведение типов. Статический анализатор выводит типы для полей, методов, локальных переменных и для дженерик объектов. Если анализатор по тем или иным причинам не смог вывести тип объекта, то выводится и используется dynamic.
Коллекции являются дженерик объектами, соответственно, если явно не указать тип этой коллекции при объявлении переменной, то типы выводятся согласно всем элементам этой коллекции.

```dart
Map<String, dynamic> myMap = {'ten': 10, 'one': '1'}

var mayMap2 = {'ten': 10, 'one': '1'}; // Тип данной мапы будет <String, Object>
```
> В структуре Map типы выводятся в зависимости от типов ключа и значения. В первом примере мы явно указали типы ключ/значение. Во втором же тип приводится к ближайшему верхнему типу объектов значений и это Object. Так же работает и с кастомными классами.

- Поля и методы класса при переопределении, имеют тип как у своего родительского класса.
- Статические поля класса выводят тип из своего инициализатора.
- Если не указан тип для поля, то он выводится из своего значения по умолчанию.


---  
### Материалы:
- https://www.tutorialandexample.com/dart-type-inference
- https://github.com/dart-lang/language/blob/main/resources/type-system/inference.md
---
Теги: #ready 
