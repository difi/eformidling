---
title: ServiceRegistry
description: Informasjon om ServiceRegistry
permalink: about.html

layout: page
sidebar: eformidling
---

# Virksomhet

```json
{
    "infoRecord": {
        "identifier": "123456789",
        "organizationName": "En offenlig organisasjon",
        "entityType": {
            "name": "ORGL",
            "acronym": null
        },
        "postadresse": {
            "adresse": "Postboks 8115 Dep",
            "postnummer": "0032",
            "poststed": "OSLO",
            "land": "Norge"
        }
    },
    "serviceRecords": [
        {
            "serviceIdentifier": "DPO",
            "organisationNumber": "123456789",
            "pemCertificate": "-----BEGIN CERTIFICATE-----PublicKey-----END CERTIFICATE-----",
            "endPointURL": "https://www.altinn.no",
            "serviceCode": "4192",
            "serviceEditionCode": "170101"
        },
        {
            "serviceIdentifier": "DPE_INNSYN",
            "organisationNumber": "123456789",
            "pemCertificate": "-----BEGIN CERTIFICATE-----PublicKey-----END CERTIFICATE-----"
            "endPointURL": "",
        },
        {
            "serviceIdentifier": "DPE_DATA",
            "organisationNumber": "123456789",
            "pemCertificate": "-----BEGIN CERTIFICATE-----PublicKey-----END CERTIFICATE-----"
            "endPointURL": "",
        },
        {
            "serviceIdentifier": "DPV",
            "organisationNumber": "919859962",
            "endPointURL": "https://www.altinn.no/ServiceEngineExternal/CorrespondenceAgencyExternal.svc"
        }        
    ]
}
```

#Innbygger

```json
{
    "infoRecord": {
        "identifier": "06068700602",
        "organizationName": null,
        "entityType": {
            "name": "citizen",
            "acronym": null
        },
        "postadresse": null
    },
    "serviceRecords": [
        {
            "serviceIdentifier": "DPI",
            "organisationNumber": "06068700602",
            "pemCertificate": "-----BEGIN CERTIFICATE-----PublicKey-----END CERTIFICATE-----",
            "endPointURL": "https://qaoffentlig.meldingsformidler.digipost.no/api/ebms",
            "serviceCode": null,
            "serviceEditionCode": null,
            "orgnrPostkasse": "984661185",
            "postkasseAdresse": "test.testesen#8HC7",
            "epostAdresse": "06068700602-test@minid.norge.no",
            "mobilnummer": "+4799999999",
            "fysiskPost": false,
            "kanVarsles": true,
            "postAddress": {
                "name": "MARGIT FOS MOE",
                "street": "ETTERSTAD",
                "postalCode": "0603",
                "postalArea": "OSLO",
                "country": "Norway",
                "norge": false
            },
            "returnAddress": {
                "name": "MARGIT FOS MOE",
                "street": "ETTERSTAD",
                "postalCode": "0603",
                "postalArea": "OSLO",
                "country": "Norway",
                "norge": false
            }
        }
    ]
}
```
