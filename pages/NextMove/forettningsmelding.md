---
title: Forettningsmelding
description: Beskrivelse av forettningsmeldinger
permalink: forettningsmelding.html
layout: page
sidebar: eformidling
foler: NextMove
---

## DPI

```json
{
    "serviceIdentifier": "DPI_CITIZEN_REQUEST",
    "conversationId": "808f5003-55cd-48f3-9cc3-2fbba283bc70",
    "sender" : {
        "senderId": "123456789",
        "senderName": "AvsenderNavn"
    },
    "receiver" : {
        "receiverId": "12345678901",
        "receiverName": "Hentes fra capabilitylookup",
        "avsenderidentifikator": "",
        "faktureaReferanse": ""
    },   
    "securityLevel": "3",
    
    "mandetoryNotefication": "false",
    "spraak": "NO", //ISO_639-1
    "fysiskpostinfo" : {
        "utskriftsfarge" : "SORT_HVIT",
        "posttype": "B",
        "retur":{
            "mottaker":{
                "navn": "Hentes fra capabilitylookup",
                "adresselinje1": "Hentes fra capabilitylookup",
                "adresselinje2": "Hentes fra capabilitylookup",
                "adresselinje3": "Hentes fra capabilitylookup",
                "postnummer": "Hentes fra capabilitylookup",
                "poststed": "Hentes fra capabilitylookup",
                "adresselinje4": "Hentes fra capabilitylookup",
                "landkode": "Hentes fra capabilitylookup",
                "land": "Hentes fra capabilitylookup"

            },
            "postHaandtering": "DIREKTE_RETUR"
        } 
    },
    "digitalPostInfo" : {
        "virkningsdato": "",
        "virkningstidspunkt": "",
        "aapningskvittering": "false",
        "ikkeSensitivTittel": "",
        "varsler":{
            "epostVarsel": {
                "tekst": "Varseltekst"
            },
            "smsVarsel": {
                "tekst": "Varseltekst"
            }
        }            
    },
    "files" : [{
            "hoveddokument": "",
            "mimetype":"",
            "filnavn":"",
            "tittel":""
        }
    ] 
}
```

