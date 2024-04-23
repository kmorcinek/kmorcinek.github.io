---
layout: post
title: Zacznij od implementacji kontraktu i fake'owych danych
---

## Opis problemu

W aplikacjach webowych mamy BE i FE. Zazwyczaj najpierw powstaje BE zeby później na zwracanych danych mógł operować FE. Przez to zdaża się że w sprincie FE jest o kilka dni do tyłu za BE.

Prowadzi to czasem do tego że zadania są za duże na jeden sprint i przechodzą do następnego albo że story w sprincie nie jest E2E całości funkcjonalności tylko jest podziała na BE i FE.

## Rozwiązanie (czyli co możemy zrobić z perspektywy backendowca)

Z perspektywy BE zaczniemy tak żeby szybko dostarczyć fake'owe dane w ustalonym formacie żeby FE mógł już działać na danych i podpinać swój kod pod rzeczywiste endpointy.

Dzięki temu odblokujemy pracę frontendowców już pierwszego dnia sprintu. I zaraz po tym piszemy już właściwą implementację.

## Przykład

(przykład oczywiście strywializowany, właściwości produktu jest więcej)

Tak wyglądają ustalone payloady:

Create
POST `api/products/`

```json
{
  "name": "iPhone 12"
}
```

GET `api/products/`

```json
{
  "products": [
    {
      "id": "1edecf37-8994-43d4-9766-13a3f14c2939",
      "name": "iPhone 12"
    }
  ]
}
```

analogicznie kolejne metody PUT, DELETE oraz GET na pojedynczy produkt. Czyli będziemy mieć pełnego CRUDa.

## Kolejność

1. Endpoint do zwrócenia listy wszystkich (GET dla listy). Dzięki temu FE będzie mógł wyświetlić elementy na liście.

GET `api/products/` zwróci

```json
{
  "products": [
    {
      "id": "1edecf37-8994-43d4-9766-13a3f14c2939",
      "name": "iPhone 12"
    },
    {
      "id": "8ef0cf41-9c6f-4805-87d8-d270148dadb4",
      "name": "Samsung Galaxy S21"
    }
  ]
}
```

na sztywno zawsze, jest to zahardkowowane na warstwie API (boundary), nie przechodzi z domeny. Tak jest prościej. Proste dokumenty w boundary nie mają żadnej logiki więc możemy je od razu prosto utworzyć.

2. Możemy umożliwić stworzenie nowego productu (POST) ale tylko wystawić kontrakt. Czyli endpoint który wymaga okreśnolego payloadu i zwraca poprawny kod HTTP (201 - CREATED) ale w środku jeszcze nic nie robi.

3. Możemy wybierać co dalej robić. FE jest już odblokowany więc może już czas implentować POST, GET listy.

4. Kolejne endpointy może już można robić od razu wraz z implementacją.
