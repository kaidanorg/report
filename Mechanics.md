# Values
## Player
The player starts with a HP value of 50, when going against an enemy inside the room if his value is lesser or equal to the player's value, his value will increase by half of the value of the enemies value, if the value of the enemy is higher, then the player will lose.

The player won't be able to enter a room, if his value is lower than the room's value.
When the player goes into a room with a **xm** (eg. x2) modifier, his value will get multiplied by **n**. Likewise, if the player enters a room with a +n, his value will increase accordingly by n.

## Common Enemies
### Enemy HP (E)

α: Proporción del PVL usada para generar el enemigo (ej: 0.05 – 0.2).
K: Constante positiva para estabilizar el logaritmo (evita log(0)).

PVL: valor actual del jugador.

NUM: número de cuartos en pantalla.

MAX: valor máximo de **n** en una sala en pantalla

COE: coeficiente de control para ajustar dificultad. (COE = 0.6)

E: probabilidad final que queremos calcular.

E = (PVL × α) × log(PVL + K) × NUM^{1/3} × COE

## Bosses

### Boss HP (B)
The boss HP will be the double of the highest room value

## Rooms
### Room's value (n)
Valores de los cuartos (n) -> rand number in ( V inicial de personaje <=x>= V inicial de personaje * 10)
### Multiplier's value (m)
Si un cuarto tiene multiplicador, la probabilidad que sea un x2 es del 75% y que sea x3 un 25%.
### Rate's spawn of modifier (P)
NUM: número de cuartos en pantalla.

PVL: valor actual del jugador.

MAX: valor máximo de **n** en una sala en pantalla

FBM: frecuencia base de aparición de multiplicadores. (FBM = 0.10)

COE: coeficiente de control para ajustar dificultad. (COE = 0.6)

P: probabilidad final que queremos calcular.

P = FBM × (1 - PVL / MAX) × NUM^{1/3} × COE
