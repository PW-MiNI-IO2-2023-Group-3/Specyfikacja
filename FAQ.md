# FAQ

Dokument zawiera odpowiedzi na pytania dotyczące niezrozumianych szczegółów dokumentacji.

### 1. Jak działa serializacja obrazu i przekazywanie go w formie stringa (POST /events)
Obraz powienien stanowić zserializowany plik PNG. (Base64)

### 2. Jak działa blob na zdjęcia:
Blob to storage plików niezależny od BD. Ma przechowywac zdjecia zwiazane z eventem (wiele zdj do jednego wydarzenia). Obniża to zużycie transferu z backendem.

Web i Mobile mają bezpośredni dostep do bloba. Pliki w blobie identyfikowane są przez path. W backendzie powstały 3 nowe endpointy GET, PUT i DELETE events/photos. Backend przechowuje tylko SCIEZKI do zdjęc

##### Web dodaje zdjęcie:
Web najpierw dodaj zdjecie do bloba (blob.put(content)) a nastepnie wysyła informacje o dodanym zdjeciu do PUT events/photos.


##### Web usuwa zdjęcie:
Web najpierw usuwa spis o zdjeciu z BD (DELETE) a nastepnie usuwa zdj z bloba.


##### Mobile przeglada zdjecia:
Mobile moze pobrac liste sciezek do zdj z danego eventu (GET), a nastepnie pobrac zdjecie z bloba.

W naszym przypadku rozwiazanie skrojone jest na Azure Blob Storage, jednak w roli bloba może być wszystko (FTP, S3 itp.), oczywiscie przy załozeniu, ze ten feature nie bedzie kompatybilny miedzy systemami. Podobnie my do bloba wysyłamy zdjecia w znanej juz postaci hasha Base64. Jesli was system umożliwia przesyałnei plikow jako plikow (np przez WebDAV), droga wolna!
