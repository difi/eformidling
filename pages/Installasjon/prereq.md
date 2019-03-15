---
title: Forutsetninger for installasjon
description: Forutsetninger for installasjon
summary: "Forutsetninger for installasjon"
permalink: forutsetninger.html
sidebar: eformidling
folder: Installasjon
---

Einnsyn er en prosess der statelige virksomheter gjør metadata for korrespondanse tilgjengelig for offentligheten. Integrasjonspunktet fungerer som et bindeledd mellom eInnsynsklienten brukt av arkivarene og det sentrale eInnsynsystemet.

For å sette opp integrasjonspunktet til å støtte eInnsyn, må du gjøre følgende: 


## Dette gjør du før installasjon av Integrasjonspunktet

+ Tilgjengelig minne må være minimum 1GB for eInnsyn og 2GB for eFormidling.
+ Nødvendige brannmuråpninger
+ Java 8 med JCE installert (JDK)
+ Virksomhetssertifikat utstedt av Buypass eller Commfides. [Les mer](http://difi.github.io/eformidling/virksomhetssertifikat.html)
+ Tips: Installer integrasjonspunktet og eInnsynsklient på samme server.

### Tidssynkronisering
Bekreftelse på at en bruker er autentisert sendes fra OIDC til innholdsleverandør sitt integrasjonspunkt i form av en Json Web Token (JWT). En JWT inneholder tidsstempel som angir hvor lenge den er gyldig. Det er derfor viktig at alle servere som kommuniserer via integrasjonspunktet har synkroniserte klokker. OIDC bruker NTP (”network time protocol”) for synkronisering, tidskilden er GPS-basert. Det er videre viktig at alle servere i CoT er justert korrekt for tidssone og sommertid. (CET / CEST i Norge). Du kan sjekke din klokke her: https://time.is/ 

[For mer informasjon om Network Time Protocol.](https://no.wikipedia.org/wiki/Network_Time_Protocol)
Tjenesteleverandør velger selv tidskilde, denne bør være lokalisert internt i datasenteret.


### Brannmuråpninger i testmiljø

Når du installerer den typen eFormidling du skal ta i bruk så må du åpne opp noen brannmuråpninger. I akkordion lenger nede så må du åpne både generelle og spesifikke for den tjenesten du skal installere.

En kan også åpne DNS mot domenet ```lb.difi.no``` som dekker alle 3 lastbalansererene. DNS for logging er ```stream.difi.no```.


|    Beskrivelse    | IPv4-adresse | IPv6-adresse | Tjeneste |
| ------------- |:-------------:| :-----:| :------:|
| beta-meldingsutveksling.difi.no lb1 | 79.170.81.231:443	 | 2001:67c:2d68:d1f1::1b:1 | Alle |
| beta-meldingsutveksling.difi.no lb2 | 79.170.81.232:443	 | 2001:67c:2d68:d1f1::1b:2 | Alle |
| beta-meldingsutveksling.difi.no lb3 | 79.170.81.233:443	 | 2001:67c:2d68:d1f1::1b:3 | Alle |
| Logging - TCP utgående | 	79.170.81.228:8300  | 2001:67c:2d68:d1f1::56:1 | Alle |
| oidc-ver1.difi.no | 146.192.252.121:443		 | - | Alle |
| oidc-ver2.difi.no | 146.192.252.121:443	 | - | Alle |
| eInnsyn - move-dpe.servicebus.windows.net	 | 13.69.253.135:443 | - | DPE | 
| www.altinn.no | 89.250.123.0:443 | - | DPO |


### Brannmuråpninger i produksjon

|    Beskrivelse    | IPv4-adresse | IPv6-adresse | Tjeneste |
| ------------- |:-------------:| :-----:| :------:|
| meldingsutveksling.difi.no lb1 | 79.170.81.231:443	 | 2001:67c:2d68:d1f1::1b:1 | Alle |
| meldingsutveksling.difi.no lb2 | 79.170.81.232:443	 | 2001:67c:2d68:d1f1::1b:2 | Alle |
| meldingsutveksling.difi.no lb3 | 79.170.81.233:443	 | 2001:67c:2d68:d1f1::1b:3 | Alle |
| Logging - TCP utgående | 	79.170.81.228:8400  | 2001:67c:2d68:d1f1::56:1 | Alle |
| oidc.difi.no | 146.192.252.54:443	 | - | Alle |
| eInnsyn - move-dpe.servicebus.windows.net	 | 13.69.253.135:443 | - | DPE | 
| www.altinn.no | 89.250.123.0:443 | - | DPO |
| meldingsformidler.digipost.no | 146.192.168.126:443 | -  | DPI |
| meldingsformidler.digipost.no | 146.192.168.80:443 | -  | DPI |
| svarut.ks.no | 193.161.171.187:443 | - | DPF |


### Installere Java runtime environment (JDK)

> Fra 01.01.19 vil Oracle Java være lisensbasert. Denne kan brukes, eller en kan bruke gratisalternativ som OpenJDK. [Les mer](https://www.oracle.com/corporate/pressrelease/java-se-subscription-offering-062118.html)

Integrasjonspunktet er en Java applikasjon og krever derfor at man har Java(JDK) kjøremiljø installert på serveren den skal kjøre.
For å verifisere om java er installert og hvilken versjon kan du i et kommandolinjevindu bruke kommandoen

```
java -version
```

Integrasjonspunktet krever minimum versjon 1.8.0

### OpenJDK Java 8 (gratis)
For å laste ned en gratisversjon av Java finnes det mange ulike tilbydere av OpenJDK. Her kan en velge den tilbyderen en selv ønsker, men versjon må være Java 8. Vi har valgt å bruke JDK 8 fra [https://adoptopenjdk.net/](https://adoptopenjdk.net/) med HotSpot som JVM. Denne vil integrasjonspunktet støtte. 

Installasjonsveiledning for OpenJDK finner du her [https://adoptopenjdk.net/installation.html#x64_win-jdk](https://adoptopenjdk.net/installation.html#x64_win-jdk) . Om du bruker et annet OS enn Windows x64 bit kan du velge din platform inne på lenken.

### Oracle Java 8 (lisensbasert)
Dersom Java ikke er installert eller versjonen er for gammel, kan ny versjon lastes ned [her](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) og installeres. Denne vil integrasjonspunktet støtte.

> **NB!** Husk å installere 64-bit Java om du har 64-bit operativsystem! 

### Installere Java Cryptography Extension (JCE)

Bruker du ny versjon av Java, må ny JCE installeres. Last ned JCE fra [Oracles sider](http://www.oracle.com/technetwork/java/javase/downloads/jce8-download-2133166.html)

Det er ikke noen enkel måte å sjekke om Java Cryptography Extension er installert. Ofte kan det enkleste være å bare laste ned og installere JCE, men om du ønsker å sjekke, kan du gå til mappen ```$JAVA_HOME/jre/lib/security``` og sjekke om filene ```US_export_policy.jar``` og ```local_policy.jar``` har nyere dato enn øvrige filer. Hvis datoen er lik, må du installere JCE.
Dersom JCE mangler vil integrasjonspunket stoppe under oppstart og skrive logmelding om manglende JCE. På nyere Java versjoner må en legge JCE-filene inn i både ```$JAVA_HOME/jre/lib/security/unlimited ``` og ```$JAVA_HOME/jre/lib/security/limited```.

### Virksomhetssertifikat

> * NB! Testmiljø krever **test virksomhetssertifikat**. Produksjonsertifikat vil ikke virke i test
> * NB2! Produksjonsmiljøet krever virksomhetssertifikat for produksjon. 
> * NB3! Bruk sertifikatet merkt som **Autentiseringssertifikatet**

**Å gjøre:**
1. Anskaffe virksomhetssertifikat
2. Legge sertifikat i Java Key Store.
3. Sende sertifikat til Difi.

**Hvordan?**

[VEILEDNING: Les alt om håndtering av virksomhetssertifikat her](http://difi.github.io/eformidling/virksomhetssertifikat.html)

Integrasjonspunktet bruker virksomhetssertifikat til kryptering og signering av meldinger som går mellom integrasjonpunkter.
Virksomhetssertifikat som kan benyttes leveres av [Commfides](https://www.commfides.com/e-ID/Bestill-Commfides-Virksomhetssertifikat.html) og [Buypass](http://www.buypass.no/bedrift/produkter-og-tjenester/buypass-virksomhetssertifikat)

***
