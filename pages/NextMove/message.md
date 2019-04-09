---
title: Konvolutt og melding
description: Beskrivelse av konvolutt og melding
permalink: message.html
layout: page
sidebar: eformidling
foler: NextMove
last_updated:  day.month.year
---


En inndelt i 3 logiske deler - Adressering, forettningsmelding og en dokumentpakke, som er selve meldingen man ønsker sende.
Adrsseringen og forettningsmeldingen er realisert vet hjelp av Standard Busniness Document.


Standard Business Document er en GS1 standard utviklet for å forenkle utveksling av dokumenter i en B2B kontekst. Standardkonvolutten inneholder informasjon for identifisering, adressering og ruting av forretningsmeldingen. SBD er obligatorisk i neste versjon av PEPPOL infrastrukturen for fakturaformidling.

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

Adresseinformasjon gjøres i Standard Business Dockument Header 

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


## Forettningsmelding

Forettningsmeldingen er spesifikke i forhold til meldingstypene som sendes. 

## Dokumentpakke
Payloaden består av en til flere filer man øsker sende. Dette kan være både strukurert og ustrukurert informasjon. 

Dokumentpakken realiseres ved hjelp av Assosiated Signatur Container

Associated Signature Container er et pakkeformat som er designet for å ivareta integritet til innholdet over lang tid. Kort fortalt så definerer standarden hvordan man skal sette sammen en zip fil med en filstruktur der man lager en digital signatur hver enkel fil med en kombinasjon av et digitalt fingeravtrykk av filen og et PKI sertifikat eid av en virksomheten. Dette medfører at man kan verifisere at filene kommer fra rett virksomhet, og om filene har blitt endret.


## Meldingstypene

| Type | Prosess | Dokumenttype | Beskrivelse
|-------|--------|---------|-----|
| <b>Arkivmelding</b> |  urn:no:difi:profile:eFormidling:ver2.0 |  | |
|  |  |  |
|  |  |  |
|  |  |  |
|  |  |  |
| <b>Digital post til innbygger</b> | urn:no:difi:profile:Digitalpost:ver1.0 |  |
|  |  |  |
|  |  |  |
|  |  |  |
|  |  |  |
|  |  |  |
| <b>Digital post til virksomhet</b> |  |  |
|  |  |  |
|  |  |  |
| <b>eInnsyn</b> | urn:no:difi:profile:einnsyn-journalpost:ver1.0 |  |
|  | urn:no:difi:profile:einnsyn-innsysnkrav:ver1.0 |  |
|  | urn:no:difi:meeting:ver1.0 |  |
|  |  |  |
|  |  |  |
|  |  |  |
|  |  |  |


## Arkivmelding

Arkivmelding er samlebegrepet 

```json
{% include /custom/nextmove/forettningsmeldingDpo.json %}
```


## Digital post til private virksomheter

```json
{% include /custom/nextmove/forettningsmeldingDpv.json %}
```


## Digital post til innbygger

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





