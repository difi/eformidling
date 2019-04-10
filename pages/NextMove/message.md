---
title: Konvolutt og melding
description: Beskrivelse av konvolutt og melding
permalink: message.html
layout: page
sidebar: eformidling
foler: NextMove
last_updated:  day.month.year
---


En inndelt i tre logiske deler: Adressering, forretningsmelding og dokumentpakke - som er selve meldingen man ønsker sende.
Adresseringen og forretningsmeldingen er realisert ved hjelp av Standard Business Document.


Standard Business Document er en GS1 standard utviklet for å forenkle utveksling av dokumenter i en B2B kontekst. Standardkonvolutten inneholder informasjon for identifisering, adressering og routing av forretningsmeldingen. SBD er obligatorisk i neste versjon av PEPPOL-infrastrukturen for fakturaformidling.

<div class="mermaid">
graph LR
subgraph Melding
  subgraph Konvolutt 
    el1[<b>Standard Business Document Header</b><br/> brukt til ruting av meldingen frem til mottaker]  
    el2[<b>Forretningsmelding</b><br/>brukt til effektiv håndtering av mottak]
  end
  subgraph Innhold
    el3[<b>ASIC-E med innhold</b><br/>En eller flere filer med strukturert informasjon som skal frem til mottaker]
  end
end
</div>


## Adressering

Adresseinformasjon legges i Standard Business Document Header. 

```json
{% include /custom/nextmove/sbd.json %}
```

| Felt | Beskrivelse | Validering | 
|-------|--------|---------|
| HeaderVersion |  |  | 
| Sender |  |  | 
|  |  |  | 
|  |  |  | 
|  |  |  | 


## Forretningsmelding

Forretningsmeldingene er spesifikke i forhold til meldingstypene som sendes. 

## Dokumentpakke
Payloaden består av en til flere filer man ønsker sende. Dette kan være både strukturert og ustrukturert informasjon. 

Dokumentpakken realiseres ved hjelp av Associated Signature Containers.

Associated Signature Containers er et pakkeformat som er designet for å ivareta integriteten til innholdet over lang tid. Kort fortalt definerer standarden hvordan man skal sette sammen en zip-fil med en filstruktur der man lager en digital signatur for hver enkelt fil med en kombinasjon av et digitalt fingeravtrykk av filen og et PKI-sertifikat eid av en virksomhet. Dette medfører at man kan verifisere at filene kommer fra rett virksomhet, og om de har blitt endret etter signering.


## Meldingstypene

| Type | Prosess | Dokumenttype | 
|------|---------|--------------|
|  |  | <b>Arkivmelding</b> |  
|  |urn:no:difi:profile:arkivmelding:ver2.0 |  | 
|  |urn:no:difi.arkivmelding:xsd:planByggOgGeodata::arkivmelding | |
|  |urn:no:difi.arkivmelding:xsd:helseSosialOgOmsorg::arkivmelding | |
|  |urn:no:difi.arkivmelding:xsd:oppvekstOgUtdanning::arkivmelding | |
|  |urn:no:difi.arkivmelding:xsd:kulturIdrettOgFritid::arkivmelding | |
|  |urn:no:difi.arkivmelding:xsd:trafikkReiserOgSamferdsel::arkivmelding | |
|  |urn:no:difi.arkivmelding:xsd:naturOgMiljø::arkivmelding | |
|  |urn:no:difi.arkivmelding:xsd:næringsutvikling::arkivmelding | |
|  |urn:no:difi.arkivmelding:xsd:skatterOgAvgifter::arkivmelding | |
|  |urn:no:difi.arkivmelding:xsd:tekniskeTjenester::arkivmelding | |
|  |urn:no:difi.arkivmelding:xsd:administrasjon::arkivmelding | |
|  |urn:no:difi.arkivmelding:xsd:kvittering::arkivmelding_kvittering | |
|  |urn:no:difi.arkivmelding:xsd:kvittering::status\* | |
|  |urn:no:difi.arkivmelding:xsd:kvittering::feil\* | |
|  |  | |
| <b>Digital post til innbygger</b> | | |
|  |urn:no:difi:profile:DigitalpostInfo:ver1.0 ||
|  |  |urn:no:difi.digitalpost:xsd:digital::digital |
|  |  |urn:no:difi.digitalpost:xsd:fysisk::print |
|  |urn:no:difi:profile:DigitalpostMedVarslingsplikt:ver1.0 ||
|  |  |urn:no:difi.digitalpost:xsd:digital::digital |
|  |  |urn:no:difi.digitalpost:xsd:fysisk::print |
|  |  |  |
| <b>eInnsyn</b> | | |
|  |urn:no:difi:profile:einnsyn-journalpost:ver1.0 | |
|  |  |urn:no:difi.einnsyn:xsd:journal::einnsynmelding |
|  |  |urn:no:difi.einnsyn:xsd:journal::kvittering |
|  |urn:no:difi:profile:einnsyn-innsysnkrav:ver1.0 | |
|  |  |urn:no:difi.einnsyn:xsd:innsyn::innsynskrav |
|  |  |urn:no:difi.einnsyn:xsd:innsyn::kvittering |
|  |urn:no:difi:meeting:ver1.0 |  |
|  |  |urn:no:difi.einnsyn:xsd:meeting::einnsynmelding |
|  |  |urn:no:difi.einnsyn:xsd:meeting::kvittering |

\* dokumenttypen er forbeholdt kontrollmeldinger i infrastrukturen og skal ikke brukes av integrasjoner

## Arkivmelding

Arkivmeldinger er meldinger som sendes mellom sak-/arkivisystemer bassert på NOARK5 metadata. 
Dersom mottaker ikke har integrasjonspunkt vil avsenders integrasjonpunkt mappe meldingen til mottakers foretrukne mottaksplattform. I første omgang vil dette i hovedsak dreie seg SvarInn og SvarInn2 etterhvert som denne tas i bruk. Dersom mottaker ikke er knyttet til en annen platform, vil meldingen sendes til Digital postkasse for virksomheter (DPV). 
En kan som mottaker med integrasjonspunkt velge at en ikke ønsker motta alle meldingstyper i sitt integrasjonspunkt. Meldingene man ikke ønsker motta vil da routes til virksomhetens postboks i AltInn via DPV.

```json
{% include /custom/nextmove/forettningsmeldingDpo.json %}
```


## Digital post til innbygger

Ved sending av digital post til innbygger må man ta stilling til om meldingen har varslingsplikt eller ikke. Utvidede regler rundt dette finnes her (link).
Begge prosessene støtter både digtalpost og fysisk post.


**Digital post**
```json
{% include /custom/nextmove/forettningsmeldingDpiDigital.json %}
```

**Fysisk post**

```json
{% include /custom/nextmove/forettningsmeldingDpiFysisk.json %}
```


## eInnsyn

**Journal**

```json
{% include /custom/nextmove/forettningsmeldingDpeJournal.json %}
```

**Innsynsbegjæring**

```json
{% include /custom/nextmove/forttningsmeldingDpeInnsyn.json %}
```

**Møte**
```json
{% include /custom/nextmove/forettningsmeldingDpeJournal.json %}
```





