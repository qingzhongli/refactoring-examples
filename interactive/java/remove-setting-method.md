remove-setting-method:java

###

1. Значение поля должно меняться только в конструкторе. Если конструктор не содержит параметра для установки значения, нужно его добавить.

2. Найдите все вызовы сеттера.<ul><li>Если вызов сеттера стоит сразу после вызова конструктора текущего класса, переместите его аргумент в вызов конструктора и удалите сеттер.</li><li>Вызовы сеттера в конструкторе замените на прямой доступ к полю.</li></ul>

3. Удалите сеттер.



###

```
class Account {
  // ...
  private String id;
  
  public Account(String id) {
    setId(id);
  }
  public void setId(String id) {
    this.id = id;
  }
}
```

###

```
class Account {
  // ...
  private String id;
  
  public Account(String id) {
    initializeId(id);
  }
  protected void initializeId(String id) {
    this.id = "ID" + id;
  }
}

class InterestAccount extends Account {
  private double interestRate;
  public InterestAccount(String id, double interestRate) {
    initializeId(id);
    this.interestRate = interestRate;
  }
}
```

###

Set step 1

# Рассмотрим <b>удаление сеттера</b> на таком простом примере — у нас есть класс банковского счета. В нем есть поле идентификатора, который должен создаваться только один раз и больше не меняться.

Select name of "setId"

# Тем не менее, сейчас в классе есть сеттер. Вот его мы и попытаемся убрать.

Select body of "setId"

# Самым простым решением было бы интегрировать код сеттера в конструктор.

Select body of "public Account"

Print:
```
    this.id = id;
```

Select whole "setId"

Remove selected

Select name of "Account"

# По сути, для такого простого случая, мы уже все сделали. Но бывают и другие, более сложные случаи. 

Select whole "public Account"

Print instant:
```
  public Account(String id) {
    setId(id);
  }
  public void setId(String id) {
    this.id = "ID" + id;
  }

```

Select body of "setId"

#< Например, если сеттер выполняет какие-то вычисления над аргументом:

#< Если изменение простое, как в этом случае, его тоже можно вынести в конструктор.

#< Однако, если изменение сложное, состоит из вызовов нескольких методов, лучше создать новый метод для инициализации значения.

Select visibility of "setId"

Wait 500ms

Print "private"

Wait 500ms

Select "setId"

Wait 500ms

Print "initializeId"

Set step 2

Go to the end of file

# Отлично, давайте рассмотрим ещё один случай.

Print instant:
```


class InterestAccount extends Account {
  private double interestRate;
  public InterestAccount(String id, double interestRate) {
    setId(id);
    this.interestRate = interestRate;
  }
}
```

Select name of "InterestAccount"

# Ещё один неприятный случай возникает, когда есть подклассы, инициализирующие закрытые переменные родительского класса.

Select "setId(id)"

# В этом случае, вместо вызова сеттера стоит вызывать родительский конструктор.

Print "super(id)"

Select "super(id)"

# В случаях, когда это невозможно, остаётся вызвать нужный метод инициализации, хотя сперва следует позаботиться о том, чтобы он был доступен для подклассов.

Select visibility of "initializeId"

Wait 500ms

Print "protected"

Wait 500ms

Select "super(id)"

Wait 500ms

Print "initializeId(id)"

#C Запускаем финальную компиляцию.

#S Отлично, все работает!

Set final step

#Q На этом рефакторинг можно считать оконченным. В завершение, можете посмотреть разницу между старым и новым кодом.