# Nalepnice 4,6cm x 2,5cm #

Oznaka: **"uvozdeklar"**  
Uređaj: Običan štampač  
Medijum: Papir sa samolepećim etiketama  

Nalepnice su napravljene na zahtev Ciciban/Planika grupe, i omogućavaju štampu uvoznih deklaracija na posebno pripremljen A4 papir.

# Upit #

Obrazac se oslanja na dva upita:

## rptEL_DeklarByUlID_mag ##

Upit ne ulazi u standardni Balans paket, potrebno ga je dodati ručno:

```sql
PARAMETERS InTmpkID Long, InJobID Long;
SELECT C.CntFld, E.ElID, E.Artikal, E.Tip, E.Boja, E.Model, E.Naziv, E.Jed
FROM (TmpKorEleCho AS T INNER JOIN EL_ElementiEx AS E ON T.ElID=E.ElID) INNER JOIN TmpCount AS C ON C.CntFld<=T.BKol
WHERE T.TmpkID=InTmpkID AND T.JobID=InJobID
ORDER BY E.ElID;
```

## EL_DeklarByEdgElID ##

Upit je deo standardnog Balans paketa.
