---
title: Meldingstyper
description: Oversikt over meldingstyper/mottaks plattformer supportert av integrasjonspunktet
summary: "Informasjon om hvordan man kommer igang med eformidling"
permalink: messagetypes.html
layout: page
sidebar: eformidling
folder: integrasjonspunktet
---

I flytene nedenfor er det for tydeliggjøre de sentrale kompoentene i flyten fjernet autentisering/autorisering mot SR samt oppdatering av statusdatabasen.  
I flytene under vises også flytene synkront, og ikke asynkront med kø som er defauten i integrasjonspunktet. Dette også for å fokusere på det sentrale i flyten.
En generell flyt der asynkronitet, autentisering/autorisering og statusdatabase er inkludert kan sees i bunn av denne siden

Oppslaget for adrressering er også generalisert ved komponenten ServiceRegistry (SR). I virkligheten er skjer det i bakant av denne oppslag i en rekke register for å avgjøre hvordan meldingen skal routes. For nærmere beskrivelse se [ServiceRegistry](). 



## Digital post offentlige virksomheter (DPO)

DPO meldinger er meldinger der både avsender og mottaker har integrasjonspunkt.
Sak-/Arkivsystemet starter prosessen med å sjekke om mottaker kan motta melding med tjenesten GetCanReceive. Integrasjonspunktet returnerer true dersom den SR returnerer DPO egenskapen. Sak-/Arkivsystemet vil deretter kalle tjenesten PutMessage med meldingen som ønskes sendt. Den mottatte meldingen valideres og pakkes i en ASiC kontainer, som krypteres og legges til SBD meldingen. SBDH fylles ut med adresseringsinformasjon, og hele meldingen signeres. Meldingen lastes deretter opp på AltInns formidlingstjeneste.
Mottakende integrasjonspunkt puller sin "meldingsboks" på AltInns formidlingstjeneste. Dersom den finner ny melding lastes denne ned, pakkes ut, signaturer valideres og payload dekrypteres. Deretter hentes BestEdu meldingen ut fra ASiC kontaineres. Mottagers integrasjonspunkt kaller mottakers Sak-/Arkivsystems PutMessage med BestEdu medlingen. Mottakers Sak-/Arkivsystem sender en AppReceipt ved hjelp av ny PutMessage som kvittering på mottak av meldingen. Denne formidles tilbake til avsender som andre meldinger mellom integrasjonspunktet.


<div class="mermaid">
sequenceDiagram
    participant saa as SakArkiv avsender
    participant ipa as Integrasjonspunkt 
    participant sr as ServiceRegistry
    participant mf  as AltInn formidlingstjeneste
    participant ipm as Integrasjonspunkt mottaker
    participant sam as SakArkiv mottaker

    saa->>ipa: GetCanReceive
    activate ipa
    ipa->>sr: GetReceiver
    sr-->>ipa: Receiver(DPO)    
    ipa-->>saa:response
    deactivate ipa    
    saa->>ipa: PutMessage
    activate ipa
    ipa-->>saa: 
    deactivate ipa
    ipa->>mf: Upload
    loop time
        opt NewMessageAvailable 
            ipm->>mf: DownloadMessage
            activate ipm
            mf-->>ipm: Message
            ipm->>sam: PutMessage
            sam->>ipm: PutMessage (AppRecipt)
            ipm-->>mf: UploadMessage
            deactivate ipm
        end
    end
    loop timeunit
        opt NewMessageAvailable 
            ipa->>mf: DownloadMessage
            activate ipa
            ipa->>saa: PutMessage(AppRecipt)
            deactivate ipa
        end
    end
</div>

## DPO med MSH

Dersom man tar ibruk integrasjonspunktet og allerede har MSH med mottakere en kommuneiserer med over BestEdu i dag, vil integrasjonspunktet virke som en proxy mot eksisterende MSH.
SR vil da returnere egenskapen DPV. Det gjøres da et ektra GetCanReceive mot msh, med påfølgende PutMessage dersom denne returnerer true. Mottakers Sak-/Arkivsystem svarer med AppReceipt over MSH infratstruktur, som tidligere.
Dette gjør at man kan fortsette å sende til mottakere man har sendt meldinger med på MSH infrastruktur. Samtidig vil man etterhvert som disse tar ibruk integrasjonspunktet sømmløst begynne å sende meldinger til de på infrastrukturen beskrevet over. For beskrivelse av oppsett for dette se installsjonsveiledningen


<div class="mermaid">
sequenceDiagram
    participant saa as SakArkiv avsender
    participant ipa as Integrasjonspunkt 
    participant sr as ServiceRegistry
    participant msh as MSH
    participant mshm as MSH
    participant ipm as Integrasjonspunkt mottaker
    participant sam as SakArkiv mottaker

    saa->>ipa: GetCanReceive
    ipa->>sr: GetReceiver
    sr-->>ipa: Receiver(DPV)
    ipa->>msh: GetCanReceive    
    msh-->>ipa: Response
    ipa-->>saa: Response    
    saa->>ipa: PutMessage
    ipa->>msh: PutMessage 
    msh->>mshm: Send
    msh-->>ipa: 
    ipa-->>saa: 
    mshm->>sam: PutMessage
    sam->>mshm: PutMessage(AppReceipt)
    mshm->>msh: Send(AppReceipt)
    msh->>saa: PutMessage(AppReceipt)
</div>



## Digital post private virksomheter (DPV)

DPV meldinger er meldinger sendt fra integrasjonspunktet til en virksomhets meldingsboks hos AltInn. Flyten initieres som ved DPO melding, men SR svarer med DPV egenskap. Meldingen mappes til en DPV melding og lastes opp til mottakes meldingsboks ved hjelp av AltInns webservice for dette. 
Etter meldingen er levert sjekkes status på meldingen med en bachjobb. 

<div class="mermaid">
sequenceDiagram
    participant fs as SakArkiv
    participant ipa as Integrasjonspunkt 
    participant sr as ServiceRegistry
    participant mf as AltInn meldingsformidler

    fs->>ipa: GetCanReceive
    ipa->>sr: GetReceiver
    sr-->>ipa: Receiver
    ipa-->>fs : Response
    fs ->>ipa: PutMessage
    ipa->>mf: Send
    mf-->>ipa: 
    ipa->>fs: PutMessage(AppReceipt[OK/ERROR])
    loop time
        ipa->>mf: GetStatus
    end
</div>



## Digital post til mottaker på FIKS (DPF)

DPF meldinger er meldinger som sendes til en mottaker på FIKS platformen. Flyten initieres som ved DPO melding, men SR svarer med DPF egenskap. Meldingen mappes til en SvarUt melding og lastes deretter opp ved hjelp av SvarUt grensesnittet. 
Etter meldingen er levert sjekkes status på meldingen med en bachjobb. 

Bruk av SvarUt og SvarInn forutsetter egen avtale med KS om dette. 

### Sende melding - SvarUt

<div class="mermaid">
sequenceDiagram
    participant fs as SakArkiv
    participant ip as Integrasjonspunkt 
    participant sr as ServiceRegistry
    participant mf  as SvarUt

    fs->>ip: UploadMessage
    ip->>sr: GetReceiver
    sr-->>ip: Receiver
    ip->>ip: CreateMessage
    ip->>mf: SendForsendelse    
    mf-->>ip: 
    ip->>fs: PutMessage(AppReceipt[OK/ERROR])
    loop time
        ip->>mf: RetrieveForsendelseStatus
    end
</div>

Mottak av meldinger fra FIKS platformen skjer ved at en bachjobb sjekker etter nye meldinger ved hjelp av SvarInn tjenesten. Dersom det finnes nye meldinger lastes disse ned og leveres mottakers Sak-/Arkivsystem via BestEdu importtjeneste . Meldingens status uppdateres dersom til lest med SvarInn. 
Dersom meldingen feiler ved levering via BestEdu kanalen sendes den mottakers postmottak med mail vi intern epost server. 

### Motta melding - SvarInn

<div class="mermaid">
sequenceDiagram
    participant fs as SakArkiv
    participant ip as Integrasjonspunkt 
    participant mf  as SvarInn
    participant ms as lokal epostserver

    loop time
        ip->>mf: HentNyeForsendelser
        mf-->>ip: Forsendelse
        ip->>mf: SettForsendelseMottatt 
        mf-->>ip: 
        loop Nye forsendelser
            ip->>fs: PutMessage(Forsendelse)
            fs-->>ip: Response
            opt Response=Error
                ip->>ms: Send(Forsendelse)
            end
        end
    end
</div>



## Digital post til innbygger (DPI)

DPI meldinger er meldinger sendt fra integrasjonspunktet til en privat innbyggers digitale postkasse. 
Dette er foreløpig på pilotstadie gjennom pilot med Forsvaret

<div class="mermaid">
sequenceDiagram
    participant fs as SakArkiv
    participant ip as Integrasjonspunkt 
    participant sr as ServiceRegistry
    participant mf  as Meldingsformidler
    participant pk as Postkasse 

    fs->>ip: PutMessage
    ip->>oidc: GetToken
    oidc-->>ip: Token
    ip->>sr: GetReceiver
    sr->>oidc: ValidateTolken
    oidc-->>sr: 
    sr->>krr: GetAddress
    krr->>oidc: ValidateTolken
    oidc-->>krr: 
    krr-->>sr:Address
    sr-->>ip: Receiver
    ip->>ip: CreateMessage
    ip->>mf: Send
    mf->>pk:Send
    pk-->>mf: 
    mf-->>ip: 
    ip->>fs: PutMessage(AppReceipt[OK/ERROR]) 
    loop timeunit 
        ip->>mf: GetRecieipt
        ip->>ip: UpdateReceiptsDB
    end
</div>

## eInnsynsmeldinger

<div class="mermaid">
sequenceDiagram
    participant saa as SakArkiv avsender
    participant ei as eInnsynsklient
    participant ipa as Integrasjonspunkt 
    participant sr as ServiceRegistry
    participant mf  as Azure SericeBus
    participant ipm as Integrasjonspunkt mottaker
    participant eim as eInnsyn

    alt 0
        saa->>ei: upload 
        ei->>ipa: POST /out/messages
    else 1
        saa->>ipa: POST /out/messages
    end
    ipa->>sr: GetReceiver
    sr-->>ipa: Receiver
    ipa->>mf: Upload
    loop time
        opt NewMessageAvailable 
            ipm->>mf: DownloadMessage
            mf-->>ipm: Message
        end
    end
    eim ->> ipm: GET /in/messages/peek
    ipm-->> eim: MessageMetaData
    eim->> ipm: GET /in/messages/pop
    ipm-->> eim: Message
</div>

## Innsynsbegjæring

<div class="mermaid">
sequenceDiagram
    participant eik as eInnsynsløsning    
    participant ei as eInnsynklient    
    participant ipa as Integrasjonspunkt 
    participant sr as ServiceRegistry
    participant mf  as Azure SericeBus    
    participant ipm as Integrasjonspunkt mottaker  
    participant eim as eInnsynsklient mottaker  
    participant epm as Lokal SMTP server
    participant pmm as Postmottak
    participant sam as SakArkiv mottaker
    

    eik->>ei: Send
    ei->>ipa: POST /out/messages
    ipa->>sr: GetReceiver
    sr-->>ipa: Receiver
    ipa->>mf: Upload
    loop time
        opt NewMessageAvailable 
            ipm->>mf: DownloadMessage
            mf-->>ipm: Message
        end
    end
    alt 0
        eim->>ipm: GET /in/messages/peek
        ipm-->>eim: MessageMetaData
        eim->>ipm: GET /in/messages/pop
        ipm-->>eim: Message
        eim->>epm: Send
        epm->>pmm: Send
        pmm->>sam: Importer
    else 1
        sam->>ipm: GET /in/messages/peek
        ipm-->>sam: MessageMetaData
        sam->>ipm: GET /in/messages/pop
        ipm-->>sam: Message
    end
</div>



## Utvidet meldingsflyt

<div class="mermaid">
sequenceDiagram
    participant fs as Fagsystem
    participant ip as Integrasjonspunkt 
    participant db as StatusDB
    participant oidc as ID-portenOAuthServer
    participant sr as ServiceRegistry
    participant mf  as Meldingsformidler

    fs->>ip: GetCanReceive
    ip->>oidc: GetToken
    oidc-->>ip: Token
    ip->>sr: GetReceiver
    sr->>oidc: ValidateTolken
    oidc-->>sr: 
    sr-->>ip: Receiver
    ip-->>fs: 
    fs->>ip: PutMessage
    ip->>ip: Enqueue
    ip-->>fs: 
    ip-Xdb: SetCreated
    ip->>oidc: GetToken
    oidc-->>ip: Token
    ip->>sr: GetReceiver
    sr->>oidc: ValidateTolken
    oidc-->>sr: 
    sr-->>ip: Receiver
    loop untilSendt
    ip->>ip: CreateMessage
    ip->>mf: Send    
    mf-->>ip: 
    ip-Xdb: SetSendt
    end
    opt DPV/DPF/DPI
        ip->>fs:AppReceipt [OK/ERROR]
    end
    ip->>ip: Dequeue 
    loop time    
        ip->>mf: GetStatus
        mf-->>ip: 
        opt DPV/DPI/DPO
            ip-Xdb: SetDelivered
        end
        opt DPF
            ip-Xdb: SetReadyForReceiver
        end
        opt DPV/DPI/DPF/DPO
            ip-Xdb: SetOpened/SetRead
        end
        opt DPI
            ip-Xdb: SetReadyForPrint
            ip-Xdb: SetNotificationFailed
            ip-Xdb: SetMesssageReturned
            ip-Xdb: SetFailed
        end
    end
</div>

