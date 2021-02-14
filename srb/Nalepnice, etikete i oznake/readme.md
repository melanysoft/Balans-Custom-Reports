# Sadržaj #

Obrasci ove grupe služe za štampu nalepnica, etiketa ili drugih oznaka iz Balansa.
Osnov za štampu predstavljaju tabelu **TmpKorEleCho** koja se prethodno popunjava **odabranim artiklima za štampu**.

**Uslov** za upotrebu obrazaca, ukoliko to nije drugačije navedeno, jeste da je aktivan modul ***Naprednih korisničkih sesija***

# Obrasci štampe iz Menadžera Kataloga #

***Menadžer Kataloga*** jedna je od formi koja omogućava pristup štampi nalepnica, etikata i oznaka.
Obrasci koji se mogu štampati registruju se kroz pogled **fn_reports_elemselection**, a od forme će im biti prosleđeni parametri:

Parametar | Vrednost
-------- | ---------
intmpkid | Id korisničke sesija koja inicira štampu
InJobID | Id posla (grupa sa spiskom artikala) za štampu
