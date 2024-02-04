# How To Arduino

How To Arduino er en lille guide til hvordan man kan skrive kode I C++ med hjælp fra Arduino.h biblioteket

## Basic info

Arduino anvender C++ som sit programmeringssprog, hvilket giver en bred palet af muligheder for at skrive effektiv kode. Dette betyder dog også, at visse koncepter kan være udfordrende for nybegyndere at forstå. Denne guide er designet til at hjælpe med at lette den indledende læringsproces og gøre det lettere at forstå de grundlæggende og mere avancerede aspekter af Arduino-programmering.

---
Her er en lista af alt guiden har at hjælpe med

Alle sektioner har en eller flere eksempler under introduktionen

[Datatyper: Tal](#tal)
- [Datatyper: unsigned](#kun-heltal)
- [Datatyper: Konstanter](#konstanter)

[Funktioner](#funktioner)

[Loops: General](#loops)
- [Loops: While](#while-loops)
- [Loops: For](#for-loops)

---

## Tal
C++ har en masse data typer for forskællige tal
```cpp
// Her er en liste af alle datatyper fra mindst til størst
bool // bool kan enten være true eller false
char // char kan enten være en charakter eller et tal og er 1 byte
int // int er et heltal på 4 bytes
float // float er kommatal på 4 bytes
double // double er kommatal på 8 bytes
long // long er en 8 byte int så den kan også kun være heltal

// Alle datatyper består af [Type] [Navn]
```

En float værdi er speciel fordi den meget gerne skal endes med 'f' hvis der er brugt et komma tal. Fx ```float tal = 3.2f;```

Her er et par eksempler

```cpp
bool isArduino = true; // bool kan enten være true eller false
char lightStrength = 100; // char kan have tal fra -128 til 127
char lightColor = 'R'; // Char kan også være en enkelt character
int maxTimeOut = 1700; // int er et heltal
int powerOffTime = -3600; // int kan være positiv og negativ
float temperature = 20.5f; // float er et kommatal med enkelt præcision
double voltage = 5.732f; // double er et kommatal med dobbelt præcision
long sensorValue = 4294967295; // long er en 8 byte heltalsværdi
```

### Kun heltal

Tal kan også modificeres en smule ved hjælp af unsigned-modifieren. Dette udvider rækkevidden af positive tal, men kan også medføre uventede problemer, såsom "wrap around".

"Wrap around" opstår, når et tal går under eller over dets maksimale værdi og starter på den modsatte side af talområdet.

```cpp
unsigned char t = 0;
t - 1; // Nu er t = 255
t + 1; // Nu er t = 0 igen
```

Her er et eksempel på det og alle de datatyper der kan bruge unsigned

```cpp
unsigned char tal = 255; // Nu er en char fra 0 til 255
unsigned int tal = 721; // Nu er int fra 0 til over 4.200.000.000
```

Her kommer der også en lille liste af det typer arduino har lavet og hvad de i virkeligheden er

```cpp
byte = unsigned char
word = unsigned int
```

### Konstanter

En konstant er en modifier, der er ekstremt kraftfuld, da den gør en variabel "read-only", hvilket betyder, at dens værdi ikke kan ændres efter tildeling.

Vi kan erklære konstanter ved hjælp af const-nøgleordet, efterfulgt af datatypen og variabelnavnet.

```cpp
const char moveLeftKey = 'A';
const int jumpHeight = 15;
const float moveSpeed = 13.34f;
```

Når vi definerer en konstant, skal vi også give den en værdi med det samme. Dette skyldes, at en konstant ikke kan ændres senere i koden. Dette sikrer, at værdien forbliver konstant og forhindrer utilsigtede ændringer i koden.


## Funktioner

Funktioner er kraftfulde værktøjer, der tillader dig at organisere din kode, gøre den mere læselig og genanvendelig. Selvom Arduino IDE traditionelt opretter to funktioner for dig, `setup()` og `loop()`, kan du oprette dine egne funktioner til at udføre specifikke opgaver i dit program.

En funktion følger en bestemt syntaks, der inkluderer typen af data, funktionens navn og eventuelle parametre, den modtager. Et eksempel kan ses neden under.

```cpp
void functionName(parameter1, parameter2, ...) {
  // Kode til udførelse af opgave
}
```

Typen void indikerer, at funktionen ikke returnerer nogen værdi. Du kan også bruge parenteserne til at definere parametre, som er data, som funktionen kan bruge i sin udførelse.

Her er et eksempel på en funktion, der initialiserer Arduino's serielkommunikation og udskriver en besked til konsollen:

```cpp
void PrintConsole() {
  Serial.begin(9600); // Initialiserer seriel kommunikation ved 9600 bps
  Serial.println("Hello, World!"); // Udskriver "Hello, World!" til seriel monitor
}

void setup() {
  PrintConsole(); // Kalder PrintConsole funktionen
}
```

I dette eksempel er `PrintConsole()` funktionen oprettet for at initialisere serielkommunikationen og udskrive en besked til konsollen. I `setup()` funktionen kaldes `PrintConsole()` funktionen for at udføre disse opgaver ved opstart af Arduino.

Funktioner kan også tage parametre, som er data, de kan bruge i deres udførelse.

```cpp
// pin og duration = det der er indsat hvor vi kalder funktionen
void BlinkLed(int pin, int duration) { 
  pinMode(pin, OUTPUT); // Indstiller pin som output
  digitalWrite(pin, HIGH); // Tænder LED
  delay(duration); // Venter i den angivne periode
  digitalWrite(pin, LOW); // Slukker LED
}

void setup() {
  int ledPin = 13;

  // Kalder BlinkLed-funktionen med pin-nummeret og varigheden af blinket
  BlinkLed(ledPin, 1000); 
}
```

Og til sidst så kan en funktion også give noget tilbage. Her er et eksempel hvor vi sender summen af to tal tilbage

```cpp
int Add(int a, int b) {
  int result = a + b;
  return result; // return sender den variabel tilbage til den der kaldte funktionen
}

void setup() {
  int sum = Add(5, 3); // Kalder Add-funktionen med to tal og gemmer resultatet i variablen sum
  Serial.begin(9600);
  Serial.println(sum); // Udskriver resultatet (8) til serielmonitor
}
```

Her returnerer Add()-funktionen summen af to tal (a og b). I setup()-funktionen kaldes Add() med to tal (5 og 3), og resultatet gemmes i variablen sum, som derefter udskrives til serielmonitor.

## Loops

Loops er en grundlæggende kontrolstruktur i programmering, der tillader gentagelse af kodeblokke. Selvom der er flere typer loops, som for eksempel for-, while- og do-while-loops, tjener de alle det samme formål: at udføre en bestemt kode gentagne gange, indtil en bestemt betingelse er opfyldt eller en tæller når en vis værdi. På denne måde er loops afgørende for at automatisere gentagne opgaver og reducere gentagelse af kode

### While loops

Et while-loop er en af de mest fundamentale former for loops. Det er ikke kun let at læse, men det kan også være meget nyttigt. Konceptet med et while-loop er, at det skal forstås som "kør dette, så længe [betingelse] er opfyldt".

Her er et eksempel af et while-loop der vil køre for evigt

```cpp
while(true){

}
```

Dette while-loop kører uendeligt, fordi det skal tolkes som "så længe sandt er sandt, gentag koden". "Sandt" kan erstattes med en betingelse, der passer bedre til formålet.

Her er et eksempel på et loop, der sætter 10 Arduino-pins til output:

```cpp
void setup(){
  const unsigned char pinOffset = 1;
  unsigned char pins = 0;

  while(pins < 10){
    pinMode(pins + pinOffset, OUTPUT);
    pins += 1;
  }
}
```

Dette while-loop udfører pinMode-funktionen for at indstille hver af de 10 Arduino-pins som output. pins variablen bruges som en tæller, og pinOffset variablen bruges til at justere pinnumrene. Loopen gentages, indtil pins når værdien 10.

### For loops

For-loops er en anden type loop, der findes i C++. Der er to typer af for-loops: den traditionelle for-loop og for each-loop, som findes i sprog som C#. Lad os først se på den traditionelle for-loop.

Den traditionelle for-loop består af tre dele: initialisering af index, en betingelse og en opdatering af index i hvert gennemløb.

Her er hvordan en traditionel for-loop ser ud

```cpp
// Her initialiseres indexvariablen i til 0
// i < 10 er betingelsen for at fortsætte loopet
// ++i eller i += 1 er opdateringen af indexvariablen for hvert gennemløb
for(int i = 0; i < 10; ++i){

}
```

I dette eksempel initialiseres i til 0, og så længe i er mindre end 10, vil loopet køre. Efter hvert gennemløb af loopet bliver i øget med 1.

For each-loopet er en lidt mere kompliceret type af vores for-loop. Det kræver, at vi har noget at loop over, såsom et array eller en samling af elementer.

Når man skal læse et for each-loop, skal det læses som "for hvert element i array".

Her er et eksempel på hvordan det kan se ud 

```cpp
for(int i : array){

}
```

I dette eksempel gennemløbes hvert element i array, og værdien af hvert element tildeles variablen i. Bemærk, at typen af i skal passe til typen af elementerne i array. For eksempel, hvis array indeholder heltal, skal i også være af typen heltal.

Her er et eksempel, hvor vi finder det største tal i et array ved hjælp af et for each-loop

```cpp
const float array[7] = { 12.0f, 8.4f, 9.121f, 0.01f, -76.77f, 12.0001f, 3.3333f };
float largest = array[0]; // Vi initialiserer 'largest' med det første element i arrayet

for(float i : array){
  if(i > largest){
    largest = i; // Hvis det nuværende element er større end 'largest', opdateres 'largest'
  }
}
```

I dette eksempel initialiseres largest med det første element i array. Derefter gennemløbes hvert element i array ved hjælp af et for each-loop. For hvert element sammenlignes værdien med largest, og hvis det nuværende element er større end largest, opdateres largest med værdien af det nuværende element. Når loopet er færdigt, vil largest indeholde det største tal i array.