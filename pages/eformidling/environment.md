---
title: Miljøoversikt
description: 
permalink: environment.html
layout: page
sidebar: eformidling
foler: eformidling
---

Integrasjonspunktet kan kjøres i forskjellige profiler/modus både for Produksjon, Staging (pre-produduksjonstesting) samt utvikling. Under følger en oversikt over hvikle av de benyttede tjenestene man når i modus for produksjon og staging.


## Produksjon

Default profil for integrasjonspunktet er produksjon. I denne profilen vil alle integrasjonspunktet 

<div class="mermaid">
graph LR 
    subgraph Virksomhet
        IP[<b>Integrasjonspunkt</b><br>]
    end
    subgraph Difi
        SR[<b>ServiceRegistry</b><br>meldingsutveksling.difi.no/serviceregistry]
        OIDC[<b>OIDC</b><br>oidc.difi.no]
        ELMA[<b>ELMA</b><br>test-smp.difi.no] 
        Virksert[<b>Virksert</b><br>meldingsutveksling.difi.no/virksomhetssertifikat] 
        KRR[<b>KRR</b><br>oidc.difi.no/kontaktinfo-oauth2-server]                 
        Konfig[<b>Konfigurasjonstjeneste</b><br>]
        Log[<b>Logtjeneste</b>]
    end
    subgraph Meldingsformidler
        subgraph DPO
        mfDpo[<b>AltInn</b><br>www.altinn.no/ServiceEngineExternal/BrokerServiceExternalBasic<br>www.altinn.no/ServiceEngineExternal/BrokerServiceExternalStreamed]
        end   
        subgraph DPV
        MfDpv[<b>AltInn</b><br>www.altinn.no/ServiceEngineExternal/CorrespondenceAgencyExternal]
        end
        subgraph DPF
        mfSvarUt[<b>KS - SvarUt</b><br>svarut.ks.no/tjenester/forsendelseservice]
        mfSvarInn[<b>KS - SvarInn</b><br>svarut.ks.no/tjenester/svarinn]
        end
        subgraph DPI
        MfDpi[<b>DPI Meldingformidler</b> <br>qaoffentlig.meldingsformidler.digipost.no]
        end
        subgraph DPE
        MfDpe[<b>Azure</b><br>http://move-dpe-prod.servicebus.windows.net/]
        end
        Generisk 
    end
    IP -- Hent egenskap for mottaker--- SR
    SR---ELMA
    SR---KRR
    SR---Virksert
    SR---OIDC
    SR---Brreg[<b>Brreg</b><br>data.brreg.no]
    SR---FiksAdresse[<b>FIKS Adressetjeneste</b><br>svarut.ks.no/tjenester/forsendelseservice]
    IP -- Send melding --- Generisk 
    IP -- HentKvittering --- Generisk 
    IP -- GetTolken --- OIDC
    IP --- Konfig
    IP --- Log
</div>

## Staging


<div class="mermaid">
graph LR 
    subgraph Virksomhet
        IP[<b>Integrasjonspunkt</b><br>spring.profiles.active=staging]
    end
    subgraph Difi
        SR[<b>ServiceRegistry</b><br>beta-meldingsutveksling.difi.no]
        OIDC[<b>OIDC</b><br>oidc-ver1.difi.no]
        ELMA[<b>ELMA</b><br>test-smp.difi.no] 
        Virksert[<b>Virksert</b><br>beta-meldingsutveksling.difi.no/virksomhetssertifikat] 
        KRR[<b>KRR</b><br>oidc-ver1.difi.no/kontaktinfo-oauth2-server] 
        FiksMock[<b>FIKS Adressetjeneste</b>Adressse mock]
        BrregFacade[BrregFacade]
        Konfig[<b>Konfigurasjonstjeneste</b><br>]
        Log[<b>Logtjeneste</b>]
    end
    subgraph Meldingsformidler
        subgraph DPO
        mfDpo[<b>AltInn</b><br>www.altinn.no/ServiceEngineExternal/BrokerServiceExternalBasic<br>www.altinn.no/ServiceEngineExternal/BrokerServiceExternalStreamed]
        end   
        subgraph DPV
        MfDpv[<b>AltInn</b><br>tt02.altinn.no/ServiceEngineExternal/CorrespondenceAgencyExternal]
        end
        subgraph DPF
        mfSvarUt[<b>KS - SvarUt</b><br>test.svarut.ks.no/tjenester/forsendelseservice]
        mfSvarInn[<b>KS - SvarInn</b><br>test.svarut.ks.no/tjenester/svarinn]
        end
        subgraph DPI
        MfDpi[<b>DPI Meldingformidler</b> <br>qaoffentlig.meldingsformidler.digipost.no]
        end
        subgraph DPE
        MfDpe[<b>Azure</b><br>http://move-dpe.servicebus.windows.net/]
        end
        Generisk 
    end
    IP -- Hent egenskap for mottaker--- SR
    SR---ELMA
    SR---KRR
    SR---Virksert
    SR---OIDC
    SR---BrregFacade
    SR---FiksMock
    BrregFacade ---Brreg[<b>Brreg</b><br>data.brreg.no]
    IP -- Send melding --- Generisk 
    IP -- HentKvittering --- Generisk 
    IP -- GetTolken --- OIDC
    IP --- Konfig
    IP --- Log

</div>

