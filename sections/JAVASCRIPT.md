# Javascript

<p align="center">
  <img src="https://upload.wikimedia.org/wikipedia/commons/7/73/Javascript-736400_960_720.png" height="150">
  <br/>
</p>

## Indice

* [Variabili](#variabili)
  * [Tipi di variabili](#tipi-di-variabili)
  * [Nomi da assegnare alle variabili](#nomi-da-assegnare-alle-variabili)
* [Operazioni](#Operazioni)
* [Funzioni](#Funzioni)
* [Utilities](#Utilities)

<br>

## Variabili

<br>

### Tipi di variabili

In Javascript abbiamo diversi modi per salvare, scrivere o manipolare dei dati attraverso delle variabili. Prima di poter svolgere qualsiasi operazione con esse vanno dichiarate e per farlo abbiamo due tipi di variabili:

```javascript
const animale = "cane";
let anni = 30;
```

* **const** viene utilizzato per dichiare le cosiddette "costanti" ed il valore assegnato non verrà mai modificato in tutto il nostro applicativo;
* **let** viene utilizzato per dichiarare dei valori variabili, in modo da poter modificare il valore assegnato anche durante l'esecuzione dell'applicativo;
* esiste un terzo tipo di variabile, **var**, non più utilizzato ma che potremmo trovare in vecchi applicativi, sostituito dal moderno e più efficiente **let**.

```javascript
// Corretto
const nome = "Antonio";
let anni = 30;

anni = 31;
```
La variabile **anni** è dichiarata come **let**, quindi il suo valore è modificabile.

```javascript
// Sbagliato
const nome = "Antonio";
let anni = 30;

nome = "Luca";
```
La variabile **nome** è dichiarata come **const**, quindi il suo valore non è modificabile.


<br>

### Nomi da assegnare alle variabili

Abbiamo un modo universale per dare dei nomi alle nostre variabili, cercando di essere più descrittivi possibili e usando il **camelCase** (ovvero unire le parole senza spazi e usare la prima lettera maiuscola dalla seconda parola in poi). Inoltre i nomi devono iniziare solo con lettere, non possono contenere caratteri speciali e non devono utilizzare keyword di JavaScript.

```javascript
// Corretto
const mioPrimoLavoro = "Web Developer";
let giornoDelMese = 13;
const additionFunction = "Funzione per l'addizione";
let eta = 24;

// Sbagliato
const mioprimolavoro = "Web Developer";
const n = "Luca";
let e = 24;
const 1Lavoro = "Web Developer";
const #Lavoro = "Web Developer";
const function = "Funzione per l'addizione";
```

<br>






## Operazioni

## Funzioni

## Utilities