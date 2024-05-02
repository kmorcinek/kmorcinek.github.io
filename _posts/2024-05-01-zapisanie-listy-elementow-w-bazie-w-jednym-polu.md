---
layout: post
title: Zapisanie listy elementów w bazie w jednym polu
---

Proste podejście gdzie listę czegokolwiek zapiszemy w jednej kolumnie w bazie danych zamiast tworzyc nowe elementy (tabele) w bazie i w kodzie.

## Tak wygląda użycie

```java
public class Client {

    // ...

    @Convert(converter = StringListConverter.class)
    private List<String> emails;
}
```

korzystamy z Java, Hibernate
`AttributeConverter` to jest to co zrobi robotę.

```java
public class StringListConverter implements AttributeConverter<List<String>, String> {
    private final String DELIMITER = ";";

    @Override
    public String convertToDatabaseColumn(List<String> attribute) {
        return join(DELIMITER, attribute);
    }

    @Override
    public List<String> convertToEntityAttribute(String dbData) {
        return isBlank(dbData) ? emptyList() : List.of(dbData.split(DELIMITER));
    }
}
```

### SQL który stworzy prostą wersję

```sql
CREATE TABLE clients (
    id UUID PRIMARY KEY,
    -- other columns

    emails VARCHAR
);
```

Te rzeczy w liście nie są osobnymi Aggregatami więc nie trzeba ich trzymać w osobnej tabeli i linkować. Wystarczy płaska lista w jednym polu (jednej kolumnie).

### SQL który stworzyłby osobną tabelkę

```sql
CREATE TABLE clients (
    id UUID PRIMARY KEY
    -- other columns
);

CREATE TABLE emails (
    -- id UUID PRIMARY KEY
    client_id UUID REFERENCES clients(id),
    email VARCHAR,
);
```

W tabeli `emails` można zamodelować bez tego klucza `id`, ale ciągle jest to nowa IMHO zbędna tabela.

Analogicznie zapisalibyśmy więcej mapowań w kodzie applikacji, ale to już można sobie wyobrazić i pominę.

## Działa też dobrze dla własnych ValueObjectów

Użycie dla listy nie tylko stringów

```java
public record Email(String emailAddress) {
}

public class Client {
    // ...

    @Convert(converter = EmailListConverter.class)
    private List<Email> emails;
}

public class EmailListConverter implements AttributeConverter<List<Email>, String> {
    private final String DELIMITER = ";";

    @Override
    public String convertToDatabaseColumn(List<Email> attribute) {
        return attribute.stream()
                .map(Email::emailAddress)
                .collect(joining(DELIMITER));
    }

    @Override
    public List<Email> convertToEntityAttribute(String dbData) {
        return isBlank(dbData) ? emptyList() :
                stream(dbData.split(DELIMITER))
                        .map(Email::new)
                        .toList();
    }
}
```
