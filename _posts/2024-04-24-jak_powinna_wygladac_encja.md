---
layout: post
title: Jak powinna wyglądać encja
---

Nie jest idealna bo to by zależało od kontekstu, ale omówię kilka elementów i opiszę dlaczego "właśnie tak".

To jest obiektowy język Java + annotacje pozwalające korzystać z ORM.

```java
@Entity(name = "traders")
public class Trader {

    @Getter
    @EmbeddedId
    TraderId id;

    @Getter
    @NotNull
    String fullName;

    // default constructor for Hibernate
    protected Trader() {
    }

    private Trader(TraderId id, String newFullName) {
        // validate not null, non blank

        this.id = id;
        this.fullName = newFullName;
    }

    public static Trader create(String fullName) {
        TraderId traderId = TraderId.createNew();
        return new Trader(traderId, fullName);
    }

    void updateFullName(String newFullName) {
        this.fullName = newFullName;
    }

}
```

## Annotacje ORMa w kodzie encji

Niektórzy uważają że tak nie jest "czysto", ja nie uważam tego za problem, przyspiesza kodowanie, nie sprawia problemów.

## Używanie getterów explicite

Można by ustawić `@Getter` na całej klasie, ale uważam to za złe podejście. To nie jest torba na propertiesy tylko świadomie udostępniamy rzeczy.

## Explicite statyczne metody tworzące obiekt

(jak to ^ lepiej nazwać?)

```java
public static Trader create(String fullName)
```

pozwala nam nie myśleć o konstruktorze i ukryć część detali związanych z budowaniem obiektu.

Takich metod może być kilka w zależności od potrzeb.

## Explicite konstructor z walidacjami

Gdzieś trzeba to wszystko zwalidować. Najlepiej w jednym wspólnym miejscu.

<!-- Walidacje mogłyby też być we wspólnej statycznej metodzie tworzącej obiekt - mała dywagacja -->

## Metody zmieniające stan

`updateFullName()`

Nie używamy setterów z lombocka. Będą sytuację, kiedy trzeba zrobić zmiany w kilku zależnych polach, np

```java
void updateRange(Date from, Date to) {
    // check if (from < to) and throw exception if not
    // ...
}
```

(Zauważyłeś, że to może być Value Object "DateRange" - to dobrze)

Albo zmienić tylko jedno pole, ale najpierw trzeba zwalidować stan obiektu.
