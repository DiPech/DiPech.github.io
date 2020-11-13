## 1. Describe main interfaces and classes.

`List` – Упорядоченный список, доступ элементов осуществляется по индексу.

Основные реализации:
- **ArrayList** - Реализация на основе массива.
- **LinkedList** - Реализация на основе связного списка.
- **Vector** - Deprecated. 
- **Stack** - Работает по LIFO, потокобезопасен.

`Set` – Неупорядоченный набор уникальных объектов. Внутри set может быть только один элемент со значением null.

Основные реализации:
- **HashSet** - Реализация на основе хеш-таблицы. Быстрый - *O(1)*, но порядок элементов не определен.
- **LinkedHashSet** - Как HashSet, только гарантируется порядок элементов. В среднем производительность между HashSet и TreeSet.
- **TreeSet** - Реализация на основе бинарного дерева. Медленнее - *O(logN)*, но гарантирует порядок элементов.

Queue и Deque – Коллекция, предназначенная для хранения элементов в порядке, нужном для обработки.

**Queue** - это однонаправленная очередь, а **Deque** - двунаправленная очередь.
- **FIFO:** ```Queue<Integer> queue = new LinkedList<>();```
- **LIFO:** ```Queue<Integer> queue = Collections.asLifoQueue(new ArrayDeque<>());```

`Map` – Сопоставляет один набор объектов другому набору объектов.

Ключи уникальные, значение - не уникальные.

Основные реализации:
- **HashMap** - Реализация на основе хэш-таблицы. Быстро - O(logN), порядок не определен.
- **LinkedHashMap** - Как HashMap, но гарантирует порядок элементов. Производительность между HashMap и TreeMap.
- **TreeMap** - Реализация на основе красно-чёрного дерева (время добавления/поиска/удаления элемента — O(logN)), порядок определен.
- **Hashtable** - Deprecated.

![](/resources/data/blog/java-collections-questions/collection-classes.png)

--------------------

## 2. `ArrayList` vs `LinkedList`.

ArrayList – Построен на базе **массива**.
Элементы располагаются **подряд**, у каждого элемента есть **индекс**.
Автоматически расширяется когда возникает необходимость, а также можно самому выделить место под элементы, если заранее известно сколько нужно при помощи метода **ensureCapacity()**.
Если фактически заполнено меньше ячеек массива чем реально выделено, то можно оптимизировать хранилище по памяти при помощи метода **trimToSize()**.

LinkedList – Построен на базе **списка**.
Каждый элемент списка имеет **ссылку** на **предыдущий** и **следующий** элементы списка.
**Весит** больше, т.к. на каждый элемент хранится ссылка.

Разница во временной сложности алгоритмов добавления, обновления, удаления элементов данных коллекций, а также в итоговом размере хранения, **подробности во вложении**.

ArrayList:
- Если нужен произвольный доступ к элементам.
- Если не нужно вставлять элементы в середину списка, а только в конец.
- Когда важно экономить память (т.к. LinkedList ещё хранит на каждый элемент по две ссылки - на предыдущий и следующий элементы).
- Иногда может сыграть на руку оптимизация процессора (если массив целиком влезет в кэш процессора).

LinkedList:
- Если нужно часто добавлять элементы в коллекцию, причём не обязательно в начало или в конец.
- Если нужно часто удалять элементы из коллекции, причём заранее имеется ссылка на элемент коллекции.

Итого:
- Нужны быстрые **get(index)** / **set(index, val)** - выбрать нужно **ArrayList**.
- Нужны быстрые **add(index, val)** / **remove(index)** в середину коллекции - выбирать нужно **LinkedList**.

--------------------

## 3. `HashMap` vs `HashSet`.

**HashMap** имеет внутри себя **хэш-таблицу** - массив определённого размера, каждый элемент которого содержит **LinkedList** в которые располагаются значения **Map** после вычисления **HashCode** на основании ключа **Map** (смотри картинку во вложении).

Если для очередного ключа хэш-функция вернула индекс, по которому уже лежит объект, то новый объект добавится в конец списка (или заменит один из объектов в LinkedList, см. примечание в конце).

**!!!Важно, чтобы выполнялся контракт Equals и HashCode!!!** (смотри вложения)

Данные в **HashMap** хранятся без сортировки.

Если наш Map заполняется больше определенного коэффициента **loadFactor**, то происходит **перебалансировка Map**: увеличивается размер хэш-таблицы, объекты перераспределяются по ячейкам, коллизий становится меньше. При этом размер таблицы считается как **2^N**, т.е. увеличивается в 2 раза. Изначальное значение *N=4*, т.е. изначальный размер таблицы - 16 ячеек.

**hashCode** - используется хэш-функцией для поиска индекса в хэш-таблице, а **equals** - для сравнения объектов в *LinkedList*, лежащем по найденному индексу в хэш-таблице. Если *equals* вернуло *true* - *объект заменяется новым, иначе - объект добавляется в конец списка.

**HashSet** устроен аналогичным образом, только в качестве **ключа** используется само **значение**.

<details>
  <summary>HashMap internal structure image</summary>
  
  ![](/resources/data/blog/java-collections-questions/hashmap.png)
  
</details>

<details>
  <summary>HashSet internal structure image</summary>
  
  ![](/resources/data/blog/java-collections-questions/hashset.png)
  
</details>

--------------------

## 4. `List` vs `Set` vs `Map`.

- **List** - это упорядоченная коллекция данных, в которой каждый элемент имеет индекс, начинающийся с нуля. Может содержать одинаковые объекты.
- **Set** - это неупорядоченная коллекция данных, которая содержит уникальный набор данных. Не может содержать одинаковые объекты.
- **Map** - это неупорядоченная коллекция данных, которая содержит набор данных, за каждым закреплен уникальный ключ.

--------------------

## 5. `HashMap` vs `TreeMap`.

**Differences:**
- **HashMap** работает быстрее **TreeMap**.
- **TreeMap** построен на основе красно-черного дерева. Время добавления/поиска/удаления элемента в среднем *O(logN)*.
- **HashMap** построен на основе хэш-таблицы. Время доступа к отдельному элементу в среднем *O(1)*.

**Recommendations:**
- Если нужна упорядоченность, или нужно работать с вещественными числами – TreeMap. 
- Для всех остальных случаев – HashMap.

> Естественно, надо индивидуально рассматривать каждый сценарий отдельно, выявлять тонкости и нюансы, какие операции будут использоваться чаще и в каких сценариях.

--------------------

## 6. How to remove an element from a collection?

- Через **Collection.remove**, но нельзя это делать в цикле, т.к. иначе получим исключение **ConcurrentModificationException** (попытка модифицировать коллекцию, пока с ней происходит какая-то работа, например, чтение).
- Через итератор **Collection.iterator**, метод **Iterator.remove()**, можно использовать в процессе итерации.

--------------------

## 7. How to remove duplicates from a collection?

Use `Set`, don't use cycles (this is a common mistake):

```java
List<Integer> valuesWithDuplicates = List.of(1, 2, 2, 3, 3, 3);
Set<Integer> uniqueValues = new HashSet(valuesWithDuplicates);
```