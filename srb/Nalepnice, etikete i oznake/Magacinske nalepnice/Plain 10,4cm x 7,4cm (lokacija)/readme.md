# Nalepnice 10,4cm x 7,4cm #

Oznaka: **"loknalep"**  
Uređaj: Običan štampač  
Medijum: Papir sa samolepećim etiketama  

Nalepnice su napravljene na zahtev Kleemann d.o.o., i omogućavaju štampu lokacijske adrese na posebno pripremljen A4 papir.  
Štampaju se odabrani artikli iz neke forme koja ih prethodno smešta u **TmpKorEleCho** tabelu (npr. ***Menadžer Kataloga***).

**Kartica sadrži**

1. Čitljivu oznaku lokacije u skladištu (polica) - polje ***elementi.skladr***
2. Barkod oznake lokacije za skeniranje pomoću magacinskih uređaja

# Upit #

Obrazac se oslanja na upit 

## usr_kleem_locatlabels ##

Upit nije deo standardnog Balans paketa pa ga je potrebno dodati u bazu:

```sql
CREATE OR REPLACE FUNCTION bcus.usr_kleem_locatlabels(
	intmpkid integer,
	injobid integer,
	infilter character varying)
    RETURNS TABLE(tmpkecid integer, tmpkid integer, jobid integer, sklid integer, sklnaziv character varying, bkol double precision, fcena double precision, popust double precision, mcena double precision, doctip integer, kolsmer double precision, pozicija integer, napomid integer, dtstamp double precision, elid integer, eltip integer, elpodtip integer, barkod character varying, artikal character varying, tip character varying, boja character varying, model character varying, naziv character varying, jed character varying, pcena double precision, tcena double precision, valid integer, tarbr integer, cm3 double precision, skladr character varying, tehdoc character varying, jed1 character varying, jk1 double precision, jed2 character varying, jk2 double precision, descript character varying, garrok integer, g double precision, gbrut double precision, kolpak double precision, jedpak character varying, duzina double precision, sirina double precision, visina double precision, kolpal double precision, jeddsv character varying, jedtez character varying, jedzap character varying, ccode character varying, cname character varying, katnaz character varying, podkatnaz character varying, eng_naziv character varying, gr_naziv character varying, ru_naziv character varying) 
    LANGUAGE 'sql'
    COST 100
    STABLE PARALLEL UNSAFE
    ROWS 1000

AS $BODY$

SELECT  T.TmpkecID, T.TmpkID, T.JobID, T.SklID, S.Naziv AS SklNaziv, T.BKol, T.FCena, T.Popust, T.MCena, T.DocTip, T.KolSmer, T.Pozicija, T.NapomID, T.DTStamp, 
        E.ElID, E.ElTip, E.ElPodTip, B.ElCode AS BarKod, E.Artikal, E.Tip, E.Boja, E.Model, E.Naziv, E.Jed, E.PCena, E.TCena, E.ValID, E.TarBr, E.Cm3, 
        SK.SklAdr, E.TehDoc, E.Jed1, E.JK1, E.Jed2, E.JK2, E.Descript, E.GarRok, E.g, E.gBrut, E.KolPak, E.JedPak, E.Duzina, E.Sirina, E.Visina, E.KolPal, 
        E.JedDSV, E.JedTez, E.JedZap, E.CCode, E.CName, E.KatNaz, E.PodKatNaz, L.ENG_Naziv, L.GR_Naziv, L.RU_Naziv 

FROM ((((((TmpKorEleCho AS T 
        INNER JOIN EL_ElementiEx AS E ON T.ElID=E.ElID) 
        INNER JOIN TmpCount AS C ON C.CntFld<=T.BKol) 
        INNER JOIN (SELECT  InTmpkID AS TmpId, SerNum AS SklAdr 
                    FROM TmpSerNumPrn 
                    WHERE PrnJob=-93771 AND SerNum=@ InFilter) AS SK ON SK.TmpId=T.TmpkID) 
        LEFT JOIN ElemCodeAgr AS B ON E.ElID=B.ElID) 
        LEFT JOIN Valuta AS V ON E.ValID=V.ValID) 
        LEFT JOIN Skladista AS S ON T.SklID=S.SklID) 
        LEFT JOIN UniLangElID AS L ON E.ElID=L.UlngaID 
        
WHERE T.TmpkID=InTmpkID AND T.JobID=InJobID ORDER BY SK.SklAdr NULLS FIRST;

$BODY$;
```
