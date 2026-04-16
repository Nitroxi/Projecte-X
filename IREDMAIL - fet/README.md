# Guia de la pràctica: servidor de correu amb iRedMail

## Objectiu
En aquesta pràctica he muntat un servidor de correu amb **Ubuntu Server 24.04** i **iRedMail 1.8.0**. La idea és deixar-lo funcionant, crear usuaris, provar el correu intern, fer una prova amb Gmail, afegir un segon domini, eliminar un usuari i revisar els logs.

Això no està escrit com un blog, sinó com una **guia pas a pas** de tot el procés.

---

## Pas 1. Configurar la màquina virtual
El primer que he fet ha sigut preparar la xarxa de la màquina virtual.

- **Adaptador 1:** NAT
- **Adaptador 2:** només amfitrió

Això és important perquè amb NAT tenim eixida a Internet i amb només amfitrió podem entrar al servidor des del host.

![img](img/img01.png)

Després he comprovat el nom del servidor. Ací és important que el hostname estiga ben posat perquè després el correu depén d'això.

![img](img/img02.png)

---

## Pas 2. Començar la instal·lació d'iRedMail
Una vegada ja tenia el sistema preparat, he executat l'instal·lador d'iRedMail.

La primera pantalla demana la ruta on es guardaran les bústies. **Jo ací ho he deixat per defecte**, que és `/var/vmail`.

![img](img/img03.png)

Després l'instal·lador demana el servidor web. En aquest cas he triat **Nginx**.

![img](img/img04.png)

Després toca triar on es guardaran els comptes de correu. **Jo he posat MariaDB**, perquè és la configuració que he acabat utilitzant en aquesta pràctica.

![img](img/img05.png)

A continuació m'ha demanat la contrasenya de l'administrador de la base de dades.

![img](img/img06.png)

Després he escrit el domini principal del servidor, que és `serveis20.test`.

![img](img/img07.png)

Després he configurat el compte administrador del domini. En aquest cas és `postmaster@serveis20.test`.

![img](img/img08.png)

Quan he executat l'script, m'ha detectat un fitxer de configuració. He continuat amb l'assistent per acabar de preparar el servidor.

![img](img/img09.png)

En la selecció de components addicionals he deixat els més útils per a la pràctica. Ací es veu la selecció.

![img](img/img10.png)

Finalment, abans d'instal·lar del tot, l'assistent mostra un resum amb tota la configuració. Ací convé revisar-ho bé abans de seguir.

![img](img/img11.png)

---

## Pas 3. Entrar a iRedAdmin i Roundcube
Quan la instal·lació ha acabat, el següent pas és entrar al panell d'administració web.

Primer he entrat a **iRedAdmin** per comprovar que el servidor ja estava operatiu.

![img](img/img12.png)

Ací es veu el tauler principal una vegada iniciada la sessió.

![img](img/img13.png)

Després també he entrat a **Roundcube**, que és el webmail que utilitzaran els usuaris.

![img](img/img14.png)

Ací ja es veu la bústia del compte `postmaster@serveis20.test` funcionant.

![img](img/img15.png)

---

## Pas 4. Crear usuaris del domini principal
El següent pas ha sigut crear els usuaris per poder fer les proves.

Primer he entrat al formulari de creació d'usuaris.

![img](img/img16.png)

Després he creat un altre usuari més.

![img](img/img17.png)

En aquesta captura ja es veu el llistat d'usuaris del domini principal. Això és important perquè així es pot demostrar que els comptes existeixen de veritat.

![img](img/img18.png)

---

## Pas 5. Fer proves de correu intern
Una vegada creats els usuaris, he passat a les proves internes.

Primer he entrat amb un dels comptes per comprovar que la bústia funcionava bé.

![img](img/img19.png)

### Prova 1: de postmaster a user21
He redactat un correu des de `postmaster@serveis20.test` cap a `user21@serveis20.test`.

![img](img/img20.png)

Ací es veu que el correu ha arribat correctament.

![img](img/img21.png)

### Prova 2: de user21 a user22
Després he enviat un correu des de `user21@serveis20.test` cap a `user22@serveis20.test`.

![img](img/img22.png)

I ací es veu el correu rebut a la bústia de `user22@serveis20.test`.

![img](img/img23.png)

### Prova 3: resposta de user22 a user21
Per acabar de demostrar que tot funcionava, he fet una resposta des de `user22@serveis20.test` cap a `user21@serveis20.test`.

![img](img/img24.png)

En aquesta última captura es veu la resposta rebuda correctament.

![img](img/img25.png)

Amb aquestes tres proves queda demostrat que el correu intern funciona.

---

## Pas 6. Prova d'enviament extern amb Gmail
Després he intentat enviar un correu cap a Gmail.

Ací es veu el missatge preparat abans d'enviar-lo.

![img](img/img26.png)

El resultat ha sigut un missatge de retorn del sistema. Gmail ha rebutjat el correu amb error **550 5.7.1**.

![img](img/img27.png)

Això vol dir que la IP del servidor no estava autoritzada per enviar correu directament als servidors de Gmail.

---

## Pas 7. Crear un segon domini
Una altra part de la pràctica era configurar un segon domini.

Ací es veu el formulari per donar d'alta el domini `alumne20.test`.

![img](img/img30.png)

Quan ja tenia el domini creat, he afegit un usuari dins d'eixe domini.

![img](img/img28.png)

Després he comprovat el llistat d'usuaris del segon domini.

![img](img/img29.png)

---

## Pas 8. Provar correu entre dominis diferents
Quan ja tenia el segon domini preparat, he fet la prova d'enviament entre dominis.

En aquest cas he enviat un correu des de `prova@alumne20.test` cap a `user21@serveis20.test`.

![img](img/img31.png)

Ací es veu que el correu ha arribat correctament.

![img](img/img32.png)

També es veu el compte del segon domini ja creat i actiu.

![img](img/img33.png)

Això demostra que el servidor suporta multidomini.

---

## Pas 9. Eliminar un usuari
Una vegada acabades les proves, també he fet la part d'administració bàsica eliminant un usuari.

Primer he seleccionat el compte que volia eliminar.

![img](img/img34.png)

I després es veu el missatge de confirmació indicant que s'ha eliminat correctament.

![img](img/img35.png)

---

## Pas 10. Revisar els logs
Per acabar la pràctica, he revisat els logs del servidor. Aquesta part va bé perquè així es pot veure si les autenticacions i les entregues de correu s'han fet correctament.

En aquesta captura es veu una consulta al fitxer `/var/log/mail.log`.

![img](img/img36.png)

Ací ja es veuen línies del log del servidor.

![img](img/img37.png)

I en aquesta última captura es continuen veient registres de serveis com `postfix`, `dovecot` i `roundcube`.

![img](img/img38.png)

---

## Conclusió
Amb aquesta pràctica he pogut muntar un servidor de correu funcional amb iRedMail, crear usuaris, provar el correu intern, afegir un segon domini, enviar correu entre dominis, eliminar un usuari i revisar els logs.

La part del correu intern i del multidomini ha funcionat bé. La part de Gmail no ha funcionat per la política de rebuig de la IP, però això també forma part de la pràctica i queda explicat amb la captura del correu retornat.

