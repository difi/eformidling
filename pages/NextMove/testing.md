---
title: Utvikling og testing
description: Hvordan drive utvikling og testing mot integrasjonspunktet
permalink: testing.html
layout: page
sidebar: eformidling
foler: NextMove
---

## Utvikling

For raskere komme igang med utvikling av integrasjoner og testing av disse tilbyr vi et mock miljø, MoveMocks. 
MoveMocks er en samling komponenter som til sammen gjør det mulig å teset integrajonen mot NextMove og dens bruk av meldingsformidlere fult ut

For å teste meldingsformidlere som ender i  en postkasse er det api og gui på slik at man kan se at en sendt melding ender i forventet medldingsformidler ut fra adressering
 
 {% include /custom/nextmove/test_utvikling.jpg %}

 For testing ende til ende mellom to integrasjonspunkt er det utviklet et mock sak-/arkivsystem (SaMock) hvor man får listet ut motatte meldinger samt har mulighet til å sende meldinger tilbake til sitt eget fagsystem via samme infrastruktur 
 
 {% include /custom/nextmove/test_endeTilEnde.jpg %}



## Staging

Etter intern testing vil en kunne teste dette i staging ved å gjøre oppsett for staging. I staging vil det være en SaMock tilgjengelig som en når ved å adressere til en predeffinert mottaker. En vil på denne måte kunne se at en får sendt melding til annet integrsjonspunkt på en reell infrasturktur. En vil via SaMock i stagin også kunne sende meldinger tilbake til seg selv


 {% include /custom/nextmove/test_staging.jpg %}



 Dokumentasjon for oppsett og kjøring av MoveMocks og SaMocks finnes (her)[https://github.com/difi/move-mocks].



