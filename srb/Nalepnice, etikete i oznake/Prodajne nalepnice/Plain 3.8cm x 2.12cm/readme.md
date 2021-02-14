# Nalepnice 3,8cm x 2,12cm #

Oznaka: **"ulaznalep"**  
Uređaj: Običan štampač  
Medijum: Papir sa samolepećim etiketama  

Nalepnice su napravljene na zahtev Ciciban/Planika grupe, i omogućavaju štampu prodajnih deklaracija na posebno pripremljen A4 papir.
Štampaju se odabrani artikli iz neke forme koja ih prethodno smešta u **TmpKorEleCho** tabelu (npr. ***Menadžer Kataloga***).

# Upit #

Obrazac se oslanja na upit:

## rptEL_CeneByUlID_mag ##

Upit ne ulazi u standardni Balans paket, potrebno ga je dodati ručno:

```sql
PARAMETERS InTmpkID Long, InJobID Long;
SELECT C.CntFld, E.ElID, E.Artikal, E.Tip, E.Boja, E.Model, E.Naziv, E.Jed, S.Naziv AS SkladNaz, S.Adr AS SkladAdr, SK.MCena
FROM (((TmpKorEleCho AS T 
        INNER JOIN EL_ElementiEx AS E ON T.ElID=E.ElID) 
        INNER JOIN TmpCount AS C ON C.CntFld<=T.BKol) 
        INNER JOIN SkladKol AS SK ON (T.SklID=SK.SklID) AND (T.ElID=SK.ElID)) 
        INNER JOIN Skladista AS S ON T.SklID=S.SklID
WHERE T.TmpkID=InTmpkID AND T.JobID=InJobID
ORDER BY E.ElID;
```
