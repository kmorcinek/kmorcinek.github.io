---
layout: post
title: Rzucaj błędami i rób to dobrze
---

A trochę mniej clickbajtowo ;) to by było:

> Podejście do zarządzania rzucania wyjątkami i tłumaczenia ich na odpowiedzi HTTP

Napisane we współpracy z [Artur Wincenciak](https://teo-vincent.blog/)

## Opis problemu

Mamy aplikację webową. Przychodzą requesty, zwracamy response. Czasem zdarzają się sytuacje gdzie request nie będzie poprawnie obsłużony i zwraci `4xx` lub `500`.
Cała nasza logika nie może być w kontrolerze. Dodatkowo korzystamy z architektury hexagonalnej (Ports & Adapters). Więc mamy warstwy Domain, Application, a także różne warstwy Infrastruktury, które nie mogą nic wiedzieć o warstwie HTTP i zwracanych kodach błędów. Rzucane wyjątki też nie mogą nic wiedzieć o HTTP.

Potrzebujemy możliwości przerwania takiego procesowania w możliwie prosty sposób.

## Przykład implementacji

Kod będzie w TypeScript, bo w takim obecnie projekcie się udzielam.

```typescript
export class DomainError extends Error {
  constructor(message: string) {
    super(message);
  }
}

export class InfrastructureError extends Error {
  constructor(message: string, cause: unknown) {
    super(message, { cause: cause });
  }
}
```

```typescript
export function errorHandlingMiddleware(error, reply) {
  if (error instanceof DomainError) {
    return reply.code(400).send(error.message);
  }

  if (error instanceof InfrastructureError) {
    return reply.code(500).send(error.message);
  }

  // ...
}
```

(żeby nie komplikować to pominąłem errory `ValidationError` i `NotFoundError`)

`cause` wygląda tu na pomijany i na teraz to jest ok, bo nie chcemy go zwracać do klienta, ale będziemy go zapisywać do logów lub np Sentry.

## Rzucanie Exception/Error z warstwy Infrastructure

Lepszą nazwą niż "Infrastructure" będzie w tym wypadku "port wyjściowy (outbound)".

Słów Exception i Error będę używał zamiennie, myślę o Exception ale akurat w TypeScript nazywa się to Error.

Integrujemy się z zewnetrznym serwisem "supabase". Będzie on w osobnym pakiecie/folderze ukryty za interfejsem.

Poniżej przykład wywołania metody z biblioteki.

```typescript
const { error: error } = await supabase.generateOtp({
  // params
});

if (error) {
  throw new InfrastructureError("Cannot generate OTP", error);
}
```

Może z czasem będziemy chcieli rozbudować niektóre wyjątki (natomiast nie jest to clue dzisiejszego posta):

W przykładzie poniżej zdefiniowaliśmy nowy wyjątek, który posiada możliwość przekazania Erroru z SDK/biblioteki do Supabase z której korzystamy.

`SupebaseSomethingError` z kodu poniżej to jest przykład Erroru zdefiniowanego w SDK.

```typescript
export class SupabaseError extends InfrastructureError {
  constructor(message: string, error: SupebaseSomethingError) {
    super(message, error);
  }
}

// ...

if (error) {
  throw new SupabaseError("Cannot generate OTP", error);
}
```

## Rzucanie Exception z warstwy Domain/Application

```typescript
// I want to remove comment on a blog
// and the domain invariant is that I should be the owner if it
if (!isItMyPost) {
  throw new CannotRemoveNotMyCommentException();
}
```

Dzięki takim rozróżnieniom mamy odpowiednią separację i nie zaśmiecamy wszystkich warst informacjami że gdzieś tam jest HTTP. Nie wycieka nam to.
