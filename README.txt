Sursa principala de inspiratie pentru realizarea temei a fost laboratorul 11, dar si documentatia Microsoft si mai multe tutoriale online(chiar daca pe alte tipuri de servicii, am reusit sa filtrez informatia necesara).

Cod sursa: index.php


COGNITIVE SERVICE
Am creat un Cognitive Service pe Azure, de tip Computer Vision.

Initial am trimis cereri spre acest serviciu din Postman.
Cereri de tip POST, la URL-ul: https://brandstema3.cognitiveservices.azure.com/vision/v3.2/analyze?visualFeatures=Brands ==>std-tema03/get_brand
Corpul cererii este reprezentat de URL-ul imaginii pe care dorim sa o scanam.
Adaugand parametrul visualFeatures=Brands, setam tipul de analiza al imaginii.
Un output primit in urma cererii este vizibil in response.txt
Cererea Postman am transformat-o in cod PHP cu ajutorul functiilor CURL.


MYSQL
Am creat un SQL Server si o SQL Database pe Azure, cu optiunea de logare prin username si parola.
Am creat tabela brand_detection, in care, pe langa ID, stocam: brand-ul detectat, valorea de certitudine a scanarii, URL-ul imaginii si momentul in care s-a realizat scanarea.
Pentru conectarea si relationarea cu baza de date, am folosit extensia PDO a PHP.



WEB APP
Pentru gazduirea aplicatiei am creat un App Service, la care m-am conectat prin intermediul URL-ului: twma3app.azurewebsites.net.
Acesta are si rolul de a interconecta mai multe servicii(Data BAse, Cognitive Service, Blob Storage Account).


BLOB STORAGE
Am creat un Storage Account pe Azure, de tip Blob Storage.
Rolul acestuia este stocarea și gestionarea eficientă a datelor nestructurate, cum ar fi fișierele media in acest caz, accesul la date facandu-se usor, prin intermediul unui URL.
Am creat un container, in care vor fi stocate imaginile, in prealabil scanarii.
Conectarea la Blob Storage am realizat-o prin intermediul unui token SAS, generat cu drepturi de READ, WRITE si CREAT.
Am apelat la aceasta varianta de a accesa mediul de stocare, deoarece voiam sa realizez tot o cerere HTTP, de aceasta data de tip PUT ==>std-tema03/put_blob
Astfel, prin adaugarea SAS token-ului la finalul URL-ului aferent noilor date, daca exista permisiunile necesare, se obtine acces la mediu.
Ca si in cazul lucrului cu Cognitive Service, initial am trimit o cerere din Postman, pentru a verifica daca este corecta, abia apoi am transformat-o in cod PHP, tot cu ajutorul CURL.


HTML-PHP-CSS
Site-ul contine un formular, cu un camp de "Upload file" si un button de submit; o sectiune pentru rezultatul scanarii actuale; cat si un tabel pentru istoricul scanarilor.


MOD DE FUNCTIONARE
Accesam aplicatia prin intermediul: twma3app.azurewebsites.net
Incarcam un fisier, de preferam cu extensia PNG.
Fisierul este preluat si stocat in containerul brands, din Blob Storage, fiindu-i generat un URL.
Acest URL este trimit ca parametru lui Computer Vision, care analizeaza posibilitatea existentei unui brand in imagine.
Daca sunt detectate unul sau mai multe branduri in imagine, acestea sunt stocate in baza de date tema3db, a serverul tema3, dar si afisate in site-ul web. Daca nu, este intors un mesaj "No Brand Detected." in interfata. 
La final este interogata baza de date pentru afisarea tuturor scanarilor valide realizate, pentru a afisa in site un tabel cu acestea.


TESTARE SI REZULTATE
FUNCTIONEAZA FOARTE BINE
1. https://tema3storage.blob.core.windows.net/brands/shell_blur.png -blur
2. https://tema3storage.blob.core.windows.net/brands/amazon_semi_blur.webp -zgomot
3. https://tema3storage.blob.core.windows.net/brands/nike_bblur.jpg -blur
4. https://tema3storage.blob.core.windows.net/brands/download.jpg -parti din sigla
5. https://tema3storage.blob.core.windows.net/brands/74173410xlarge.jpg -galben pe tricou alb
6. https://tema3storage.blob.core.windows.net/brands/balmain.webp -nu se vede tot textul

NU FUNCTIONEAZA
1. https://tema3storage.blob.core.windows.net/brands/mastercard.webp
-in imagine: MasterCard
-IA: Pepsi

2. https://tema3storage.blob.core.windows.net/brands/tacobell.png
-in imagine: TacoBell
-IA: ASICS

3. https://tema3storage.blob.core.windows.net/brands/jackass_blur.png
-in imagine: Jackass
-IA: Kangaroo ->a facut analogia cu filmul

4. https://tema3storage.blob.core.windows.net/brands/GitHub-Logo.png
-in imagine: GitHub
-IA: Fendi

5. https://tema3storage.blob.core.windows.net/brands/picsart-icon-logo.png
-in imagine: PicArt
-IA: Target Corporation

6. https://tema3storage.blob.core.windows.net/brands/lipsa_lays.webp
-in imagine: Lays
-IA: Apple

7. https://tema3storage.blob.core.windows.net/brands/nivea.png
-in imagin: Nivea
-IA: Tata Motors