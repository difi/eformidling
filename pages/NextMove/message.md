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


## Foretningsmelding

Fortetningsmeldingen inneholder meldingsformidlingsspesifikk informasjon. Dette er informasjon som ikke krypters og dermed kan brukes til f.eks. routing av meldingen, samt som beslutningsgrunnlag ved mottak av meldingen. 

## Dokumentpakke

Payloaden består av en til flere filer man ønsker å sende. Dette kan være både strukturert og ustrukturert informasjon. 

Dokumentpakken realiseres ved hjelp av Associated Signature Containers.

Associated Signature Containers er et pakkeformat som er designet for å ivareta integriteten til innholdet over lang tid. Kort fortalt definerer standarden hvordan man skal sette sammen en zip-fil med en filstruktur der man lager en digital signatur for hver enkelt fil med en kombinasjon av et digitalt fingeravtrykk av filen og et PKI-sertifikat eid av en virksomhet. Dette medfører at man kan verifisere at filene kommer fra rett virksomhet, og om de har blitt endret etter signering.


## Meldingstypene


### Arkivmelding

Arkivmeldinger er meldinger som sendes mellom sak-/arkivsystemer basert på NOARK5 metadata. 
Dersom mottaker ikke har integrasjonspunkt vil avsenders integrasjonspunkt mappe meldingen til mottakers foretrukne mottaksplattform. I første omgang vil dette i hovedsak dreie seg SvarInn og SvarInn2 etterhvert som denne tas i bruk. Dersom mottaker ikke er knyttet til en annen plattform, vil meldingen sendes til Digital postkasse for virksomheter (DPV). 
En kan som mottaker med integrasjonspunkt velge at en ikke ønsker motta alle meldingstyper i sitt integrasjonspunkt. Meldingene man ikke ønsker å motta vil da routes til virksomhetens postboks i AltInn via DPV.


| Prosess | Dokumenttype | 
|---------|--------------|
|urn:no:difi:profile:arkivmelding:planByggOgGeodata:ver1.0 | |
|  |urn:no:difi.arkivmelding:xsd::arkivmelding |
|urn:no:difi:profile:arkivmelding:helseSosialOgOmsorg:ver1.0 | |
|  |urn:no:difi.arkivmelding:xsd::arkivmelding |
|urn:no:difi:profile:arkivmelding:oppvekstOgUtdanning:ver1.0 | |
|  |urn:no:difi.arkivmelding:xsd::arkivmelding |
|urn:no:difi:profile:arkivmelding:kulturIdrettOgFritid:ver1.0 | |
|  |urn:no:difi.arkivmelding:xsd::arkivmelding |
|urn:no:difi:profile:arkivmelding:trafikkReiserOgSamferdsel:ver1.0 | |
|  |urn:no:difi.arkivmelding:xsd::arkivmelding |
|urn:no:difi:profile:arkivmelding:naturOgMiljø:ver1.0 | |
|  |urn:no:difi.arkivmelding:xsd::arkivmelding |
|urn:no:difi:profile:arkivmelding:næringsutvikling:ver1.0 | |
|  |urn:no:difi.arkivmelding:xsd::arkivmelding |
|urn:no:difi:profile:arkivmelding:skatterOgAvgifter:ver1.0 | |
|  |urn:no:difi.arkivmelding:xsd::arkivmelding |
|urn:no:difi:profile:arkivmelding:tekniskeTjenester:ver1.0 | |
|  |urn:no:difi.arkivmelding:xsd::arkivmelding |
|urn:no:difi:profile:arkivmelding:administrasjon:ver1.0 | |
|  |urn:no:difi.arkivmelding:xsd::arkivmelding |
|urn:no:difi:profile:arkivmelding:response:ver1.0 | | 
|  |urn:no:difi.arkivmelding:xsd::arkivmelding_kvittering |
|  |urn:no:difi.eformidling:xsd::status\* |
|  |urn:no:difi.eformidling:xsd::feil\* |

\* dokumenttypen er forbeholdt kontrollmeldinger i infrastrukturen og skal ikke brukes av integrasjoner


```json
{% include /custom/nextmove/forettningsmeldingDpo.json %}
```

- betyding av felter 
- mapping mot dpv
- mapping til ks


### Digital post til innbygger

Ved sending av digital post til innbygger må man ta stilling til om meldingen har varslingsplikt eller ikke. Utvidede regler rundt dette finnes her (link).
Begge prosessene støtter både digitalpost og fysisk post.

Info = informasjonsmelinger uten varlingsplikt
vedtak = meldinger som medfører varslingsplikt

| Prosess | Dokumenttype | 
|---------|--------------|
|urn:no:difi:profile:digitalpost:info:ver1.0 | |
|  |urn:no:difi.digitalpost:xsd:digital::digital|
|  |urn:no:difi.digitalpost:xsd:fysisk::print |
|urn:no:difi:profile:digitalpost:vedtak:ver1.0 | |
|  |urn:no:difi.digitalpost:xsd:digital::digital|
|  |urn:no:difi.digitalpost:xsd:fysisk::print|


**Digital post**
```json
{% include /custom/nextmove/forettningsmeldingDpiDigital.json %}
```

**Fysisk post**

```json
{% include /custom/nextmove/forettningsmeldingDpiFysisk.json %}
```


### eInnsyn

| Prosess | Dokumenttype | 
|---------|--------------|
|urn:no:difi:profile:einnsyn:journalpost:ver1.0 | |
|  |urn:no:difi.einnsyn:xsd::publisering |
|urn:no:difi:profile:einnsyn:innsynskrav:ver1.0 | |
|  |urn:no:difi.einnsyn:xsd::innsynskrav |
|urn:no:difi:profile:einnsyn:meeting:ver1.0 |  |
|  |urn:no:difi.einnsyn:xsd::publisering |
|urn:no:difi:profile:einnsyn:response:ver1.0 | |
|  |urn:no:difi.einnsyn:xsd::einnsyn_kvittering |
|  |urn:no:difi.eformidling:xsd::status\* |
|  |urn:no:difi.eformidling:xsd::feil\* |

\* dokumenttypen er forbeholdt kontrollmeldinger i infrastrukturen og skal ikke brukes av integrasjoner

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





