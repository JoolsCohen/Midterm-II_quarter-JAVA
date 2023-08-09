# Задание 2. Магазин игрушек (Java)
## Информация о проекте
Необходимо написать проект, для розыгрыша в магазине игрушек. Функционал
должен содержать добавление новых игрушек и задания веса для выпадения
игрушек.
## Как сдавать проект
Для сдачи проекта необходимо создать отдельный общедоступный репозиторий(Github, gitlub, или Bitbucket). Разработку вести в этом репозитории, использовать пул реквесты на изменения. Программа должна запускаться и работать, ошибок при выполнении программы быть не должно. Программа, может использоваться в различных системах, поэтому необходимо разработать класс в виде конструктора
## Задание
1) Напишите класс, конструктор у которого принимает минимум 3 строки,
   содержащие три поля id игрушки, текстовое название и частоту выпадения
   игрушки
2) Из принятой строки id и частоты выпадения(веса) заполнить минимум три
   массива.
3) Используя API коллекцию: java.util.PriorityQueue добавить элементы в
   коллекцию
4) Организовать общую очередь 
5) Вызвать Get 10 раз и записать результат в
   файл
## Пример работы
    В метод put передаете последовательно несколько строк
    1 2 конструктор;
    2 2 робот;
    3 6 кукла.
    Метод Get должен случайно вернуть id в соответствии с весом.
    В 20% случаях выходит единица
    В 20% двойка
    И в 60% тройка.
## Критерии оценки
Приложение должно запускаться, записывать значения в файл.

# *Реализация*
1. Будем считывать список игрушек из файла, на одну строчку по одной игрушке: 
   ```
      <id товара> <вес вероятности выпадения> <количество> <наименование> 
   ``` 
   Помимо считывания из файла можно добавлять игрушки к розыгрышу вручную через метод put(Collection<toy> toys)
   Основные классы **Toy** и **ToyList**
2. Классы **Participant** и **ParticipantQueue** отвечают за контроль очереди вытягивания призов, соответствующий порядку добавления участников.
   (FIFO). Не предусматривает повторного использования без нового наполнения(участники играют один раз)
3. Основная механика розыгрыша в классе **Raffle**. 
   1. При создании розыгрыша в конструктор подается очередь участников и список игрушек.
   2. рассчитывается относительная вероятность выпадения каждой игрушки(***категория редкости***) в соответствии с суммой весов всех текущих игрушек. 
   3. В ходе розыгрыша каждый участник получает случайное число от 0 до максимальной ***редкости***(чем ниже, тем более редкие игрушки попадутся), 
   и это число сравнивается с ближайшей вероятностью по очереди розыгрыша игрушек (приоритет порядка=редкость).  
   Ответственный класс **ChanceCalc**
   4. Предусмотрена возможность ограничения выпадения по количеству через поле "количество" у каждой игрушки.
   Если количество указано отрицательным(-1 ), то оно считается неограниченным.
   Когда количество выигранных игрушек сравняется с этим полем, игрушка убирается из розыгрыша(из текущей очереди и общего списка игрушек).  
   Ответственный подкласс **QuantityCalc** 
   5. Предусмотрена возможность задания шанса проигрыша через механику веса вероятности игрушек. При изменении веса проигрыша(метод **Raffle.setLossWeight()**),
   в розыгрыш попадает **Toy**(lossWeight, "ничего") с бесконечным количеством, 
   происходит перерасчет ***категорий редкости*** и очереди розыгрыша.
4. За доступ к файлам отвечает класс **FileIO**, через него инициализируется начальный список игрушек из файла, и идет запись хода
розыгрыша через открытый BufferedWriter. Также возможна запись состояния списка игрушек в файл для просмотра остатков.