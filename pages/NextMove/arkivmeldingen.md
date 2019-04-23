---
title: Arkivmeldingen
permalink: arkivmeldingen.html
layout: page
sidebar: eformidling
foler: NextMove
---

BEST/EDUs meldingsformat var tett knyttet til NOARK4. For meldinger som sendes mellom Sak-/arkivsystemer med NextMove er det valgt et meldingsformat som ligger tettere opp mot NOARK5 basisregistrering. 

I et møte mellom Difi og Acos ble det diskutert at ACOS ønsker å realisere en meldingsbasert integrasjon. I den sammenheng ønsker kunden å standardisere meldinger slik at de kan få til en asynkron arkivering fra fagsystem til arkivkjerne av ferdige elementer. Løsningen er altså tenkt å fungere i scenariet:

Fagsystem arkiverer ferdige elementer og bryr seg deretter ikke mer om de arkiverte data. Et grunnleggende prinsipp er at alle systemId er GUID – dette vil også komme i neste versjon av Noark. På denne måten kan det eksterne systemet opprette og vedlikeholde mapper/registreringer uten å kjenne til den interne ID til objektene (arkivsakid, journalpostid).

Dette samsvarte godt med behovet Difi hadde i forbindelse med eFormidling. Det ble derfor avtalt at Difi skulle se videre på standardisering av denne meldingen opp mot Arkivverket. 

For mer informasjon om arkivmelding:
- [arkivmelding.xsd]({{ site.url }}/resources/arkivmelding/arkivmelding.xsd) er selve definisjonen. 
- [metadatakatalog.xsd]({{ site.url }}/resources/arkivmelding/metadatakatalog.xsd) definerer metadataelementer og denne er hentet uendret fra Noark5-standarden.

Arkivmelding.xsd tar utgangspunkt i objektene mappe og registrering i Noark5, samt spesialiseringer av disse, men vi har valgt å gruppere inn noen elementer i mappe og registrering, istedenfor slik de er i Noark5 – der de er plassert utenfor – dette gjelder spesielt klassifikasjon samt referanse til eksisterende mapper i strukturen.


En arkivmelding består av:
- arkivmelding.xml
- Filer som er referert fra arkivmelding.xml 


Eksempel
```xml
{% include /custom/nextmove/arkivmelding.xml %}
```
