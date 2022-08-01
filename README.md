## Задание 1
___

### 1 этап
При выполенении программы jvm начинает анализировать файл. 
В файле находятся три класс которые нужно загрузить:
- JvmComprehension
- Object
- Integer

для этого java машина вызывает ClassLoader. 

### 2 этап
После того как все классы подгрузятся, начнется процесс связывания. На этом этапе происходит подготовка классов к выполнению.
>- проверка, валидации кода.
>- подготовка приитивов в статических полях.
>- связывание ссылок на другие классы.
>

### 3 этап
После __анализа__ и __связывания__, начинается процесс инициализации ***статических*** методов и полей.

Все три этапа принадлежат к процессу загрузки классов.
Вся мета-информация о классах загружается в область памяти, которая называется ***Metaspace***.
Там будет находится информация о классе(имя, методы, тип, поля и т.д.), константы.
По дефолту объем памяти не ограничен.

Далее для хранения данных во время процесса программы буду выделены еще две области памяти
- Stack Memory (стек) - храняться примитивы и ссылки на объекты.
- Heap (куча) - храняться сами объекты.

В стеке, под каждый метод, будет создаваться фрейм для переменных, принадлежащих к этому методу.
Таким образом первый фрейм буде для метода main().

 ```
 public static void main(String[] args) { 
    int i = 1;                      // 1
    Object o = new Object();        // 2
    Integer ii = 2;                 // 3
    printAll(o, i, ii);             // 4
    System.out.println("finished"); // 7
    }                               // 8

private static void printAll(Object o, int i, Integer ii) {
        Integer uselessVar = 700;                   // 5
        System.out.println(o.toString() + i + ii);  // 6
    }
```
Далее, 1 строка, примитив, следовательно помещается в стек.
2 строка читается с права на лево, по этому сперва происходит создание объекта который под который в куче
выделяется память. А после опретора присваивания, в стек ляжет ссылка 'o'.
Т.к. Integer является объектом, соответственно в строке 3 в кучу кладется объект Integer со значением 2,
а в стек помещается ссылка ii.

В строке 4 вызывается метод printAll и для него в стеке сохдаеися новый фрейм.
В методо printAll, предается ссылка на Object, примитив типа int и ссылка на Integer.
Object уже есть в куче, остается в стек добавить на него ссылку, так же в стек помещается переменная i = 2 и ссылка 
на объект Integer. 
При вызове printAll будет создана еще один объект Integer (5 строка) и ссылка на него добавиться в стек.

При выполнении 6 строки в стеке будет создан новый фрейм, с тремя ссылками __о i ii__.
Поcле отработки 6 строки, удалиться последний фрейм и прежде чем перейти к выпполнению
7 строки, стек так же освободится от фрейма метода printAll.

В 7 строке будет создан фрейм, для ссылки на объект String, который будет помещен в кучу.
Это произойдет по скольку в строке 7 должна будет создаться строка для вывода на консоль.
И после отработки 7 строки стек освободится от только создавшегося фрейма
При переходе на строку 8 стек полностью очиститься от фреймов.





