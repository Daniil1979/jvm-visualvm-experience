Задача - Исследование JVM через VisualVM

- Программа JvmExperience запускается выводит сообщение Please open 'ru.netology.JvmExperience' in VisualVm и ждёт 30 секунд.
- Последовательно загружает классы из трёх пакетов: io.vertx, io.netty, org.springframework.
- После этого создаёт большое количество объектов SimpleObject в куче.

Анализ по временным меткам из консоли (см.также Heap_VisualVM.jpg и Metaspase_VisualVM.jpg):

1) Запуск программы
   10:21:59: Запуск ':ru.netology.JvmExperience.main()'...
- Heap: небольшой линейный рост
- Metaspace: незначительный рост
- Classes: рост общего число загруженных классов

> Task :compileJava
> Task :processResources NO-SOURCE
> Task :classes

> Task :ru.netology.JvmExperience.main()
Please open 'ru.netology.JvmExperience' in VisualVm

2) Загрузка io.vertx
   10:22:34.127406400: loading io.vertx
   10:22:34.397880300: loaded 529 classes
- Heap: минимальный рост
- Metaspace: скачок использования памяти
- Classes: скачок числа загруженных классов (529)

3) Загрузка io.netty
   10:22:37.406054500: loading io.netty
   10:22:37.896733900: loaded 2117 classes
- Heap: незначительный рост
- Metaspace: более выраженный скачок
- Classes: резкий скачок числа загруженных классов (2117)

4) Загрузка org.springframework
   10:22:40.907830400: loading org.springframework
   10:22:41.050469900: loaded 869 classes
- Heap: незначительный рост
- Metaspace: средний рост
- Classes: скачок числа загруженных классов (869)

5) Начало работы с кучей, первая порция объектов
   10:22:44.066001600: now see heap
   10:22:44.067026700: creating 5000000 objects
   10:22:44.222378300: created
- Heap: резкий и значительный рост — 5 млн объектов занимают много памяти
- Metaspace: стабилен, новые классы не загружаются
- Classes: стабилен

6) Вторая порция объектов
   10:22:47.232015400: creating 5000000 objects
   10:22:47.421949100: created
- Heap: ещё один рост
- Metaspace: стабилен
- Classes: стабилен

7) Третья порция объектов
   10:22:50.475839600: creating 5000000 objects
   10:22:50.642226600: created
- Heap: пик использования памяти
- Metaspace: стабилен
- Classes: стабилен

BUILD SUCCESSFUL in 54s
2 actionable tasks: 2 executed

8) Завершение программы
   10:22:54: Выполнение завершено ':ru.netology.JvmExperience.main()'.

Выводы: 
- Heap растёт при создании объектов
- Metaspace растёт при загрузке классов
- Classes (счётчик загруженных классов) растёт поэтапно

VisualVM демонстрирует разделение памяти в JVM и позволяет изучить поведение Metaspace и Heap в реальном времени.
