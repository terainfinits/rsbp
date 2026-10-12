Nama : Willy Dava Nugraha \
NRP  : 5025241090 \
Kelas: Rekayasa Sistem Berbasis Pengetahuan (A)

Link untuk query: https://query.wikidata.org/

1. Deceased royal members
This finds members of the British royal family who have a death date:
```
SELECT ?person ?personLabel ?deathDate WHERE {
  ?person wdt:P463 wd:Q645968 ;   # member of British royal family
          wdt:P570 ?deathDate .   # date of death
  SERVICE wikibase:label { bd:serviceParam wikibase:language "en". }
}
ORDER BY ?deathDate
```

2. Prince George’s grandfather
Prince George’s father is Prince William; William’s father is King Charles III.
```
SELECT ?grandfather ?grandfatherLabel WHERE {
  wd:Q13590412 wdt:P22 ?father .        # George -> father
  ?father wdt:P22 ?grandfather .        # father -> grandfather

  SERVICE wikibase:label { bd:serviceParam wikibase:language "en". }
}
```

3. Princess Beatrice’s uncle
Beatrice’s father is Prince Andrew. Her uncle is her father’s male sibling—King Charles III.
```
SELECT DISTINCT ?uncle ?uncleLabel WHERE {
  wd:Q165657 wdt:P22 ?father .              # Beatrice -> father
  ?father wdt:P3373 ?uncle .                # father -> sibling
  ?uncle wdt:P21 wd:Q6581097 .              # sibling is male

  FILTER(?uncle != ?father)

  SERVICE wikibase:label { bd:serviceParam wikibase:language "en". }
}
```

4. Kate Middleton’s in-laws
This returns the parents and siblings of Kate’s spouse, Prince William:
```
SELECT DISTINCT ?inLaw ?inLawLabel ?relationship WHERE {
  wd:Q10479 wdt:P26 ?husband .              # Kate -> spouse

  {
    ?inLaw wdt:P22 ?husband .
    BIND("Father-in-law" AS ?relationship)
  }
  UNION
  {
    ?inLaw wdt:P25 ?husband .
    BIND("Mother-in-law" AS ?relationship)
  }
  UNION
  {
    ?inLaw wdt:P3373 ?husband .
    BIND("Sibling-in-law" AS ?relationship)
  }

  FILTER(?inLaw != wd:Q10479)

  SERVICE wikibase:label { bd:serviceParam wikibase:language "en". }
}
```
