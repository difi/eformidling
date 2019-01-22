---
title: Integrasjonspunktet
description: Beskrivelse av integrasjonspunktets funksjon
summary: "Beskrivelse av integrasjonspunktets funksjon"
permalink: integrasjonspunktet.html
layout: page
sidebar: eformidling
folder: eformidling
---

## Integrasjonspunktet

Integrasjonspunktet er per definisjon grenselinjen mellom det interne og eksterne miljøet. Intensjonen er at denne grenselinjen skal være så enkel som mulig. Erfaring viser at det kan være aktuelt å eksponere enkelte tjenester gjennom integrasjonspunktet. Dette vil være tjenester som i dag medfører kompleksitet for hver enkel leverandør, men der vi alle kan ha stor nytte av at det er en felles løsning. Eksempel på dette kan være tjenester for å bygge en sikker melding (lage ASIC-konteiner) og lokal eksponering av nasjonale tjenester relatert til adressering. En mulig tanke er at man legger alle tjenester som krever virksomhetssertifikat inn i integrasjonspunktet. Hvilke applikasjonstjenester trengs for avsender, og hvor bør de realiseres?
Ved mottak av en melding, er det behov for en tilsvarende fordeling av ansvar. 

* Hvem validerer at meldingen er korrekt?
* Hvordan skal denne gjøres tilgjengelig for de løsninger som skal håndtere meldingen lokalt?
* Skal vi legge opp til en push eller pull arkitektur, eller begge deler?
* Skal vi ha forskjellige strategier for å håndtere meldinger basert på størrelse? (strømmende vs pull baserte grensesnitt). 
