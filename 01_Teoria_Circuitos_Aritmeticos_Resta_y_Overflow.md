

🏠 [Inicio](README.md) › 🔌 [Unidad 1](Unidad1.md) › 📅 [Semana 4](Semana4.md) › 📖 Teoría

---

# 📖 Semana 4 — Teoría: Circuitos Aritméticos, Resta y Overflow

## ➕ 1. Suma binaria por columnas

### 1.1. Un bit: cuatro casos
Dos salidas por columna: **$S$** (*sum*, lo que escribo) y **$C$** (*carry*, lo que me llevo).

| Operación | Resultado | $S$ | $C$ |
| :-: | :-: | :-: | :-: |
| $0+0$ | 0 | 0 | 0 |
| $0+1$ | 1 | 1 | 0 |
| $1+0$ | 1 | 1 | 0 |
| $1+1$ | $10_2$ | 0 | 1 |

### 1.2. Ejemplos de 4 bits
**$0110_2(6)+0111_2(7)=1101_2(13)$**

| Columna | $A_i+B_i+C_{in}$ | $S_i$ | $C_{out}$ |
| :-: | :--- | :-: | :-: |
| 0 | $0+1=1$ | 1 | 0 |
| 1 | $1+1+0=2$ | 0 | 1 |
| 2 | $1+1+1=3$ | 1 | 1 |
| 3 | $0+0+1=1$ | 1 | 0 |

**Acarreos encadenados:** $0111+0001=1000$ — el acarreo de la columna 0 provoca otro en la 1, luego en la 2 y llega a la 3.

### 1.3. Dos tipos de columna
- **Columna 0:** suma 2 bits ⇒ **Semisumador** (*Half Adder*).
- **Columnas 1, 2, 3…:** suman 3 bits ($A_i,B_i,C_{in}$) ⇒ **Sumador completo** (*Full Adder*).

---

## 🛠️ 2. Diseño de los sumadores

### 2.1. Semisumador
| $A$ | $B$ | $C$ | $S$ |
| :-: | :-: | :-: | :-: |
| 0 | 0 | 0 | 0 |
| 0 | 1 | 0 | 1 |
| 1 | 0 | 0 | 1 |
| 1 | 1 | 1 | 0 |

$$S=A\oplus B\qquad C=A\cdot B$$
Se llama «semi» porque **no tiene entrada de acarreo previo**.

### 2.2. Sumador completo — «contar unos»

| $A$ | $B$ | $C_{in}$ | # de 1 | Binario | $C_{out}$ | $S$ |
| :-: | :-: | :-: | :-: | :-: | :-: | :-: |
| 0 | 0 | 0 | 0 | `00` | 0 | 0 |
| 0 | 0 | 1 | 1 | `01` | 0 | 1 |
| 0 | 1 | 0 | 1 | `01` | 0 | 1 |
| 0 | 1 | 1 | 2 | `10` | 1 | 0 |
| 1 | 0 | 0 | 1 | `01` | 0 | 1 |
| 1 | 0 | 1 | 2 | `10` | 1 | 0 |
| 1 | 1 | 0 | 2 | `10` | 1 | 0 |
| 1 | 1 | 1 | 3 | `11` | 1 | 1 |

- $S=1$ ⇔ **número impar** de unos.
- $C_{out}=1$ ⇔ **al menos dos** unos (mayoría).

**K-map de $S$** — patrón de «tablero de ajedrez» ⇒ función XOR:

| $A\backslash BC_{in}$ | `00` | `01` | `11` | `10` |
| :-: | :-: | :-: | :-: | :-: |
| **0** | 0 | **1** | 0 | **1** |
| **1** | **1** | 0 | **1** | 0 |

$$S=A\oplus B\oplus C_{in}$$

**K-map de $C_{out}$:**

| $A\backslash BC_{in}$ | `00` | `01` | `11` | `10` |
| :-: | :-: | :-: | :-: | :-: |
| **0** | 0 | 0 | **1** | 0 |
| **1** | 0 | **1** | **1** | **1** |

Tres grupos de 2: columna `11` → $BC_{in}$; fila 1, columnas `11-10` → $AB$; fila 1, columnas `01-11` → $AC_{in}$.
$$C_{out}=AB+AC_{in}+BC_{in}$$

---

## ➕ 3. Sumador de 4 bits en cascada (*ripple-carry*, CI 7483)

Regla única: **$C_{out}$ de cada etapa → $C_{in}$ de la siguiente.**
$$C_0=0\to\text{Sum.0}(S_0,C_1)\to\text{Sum.1}(S_1,C_2)\to\text{Sum.2}(S_2,C_3)\to\text{Sum.3}(S_3,C_4)$$

### 3.1. Efecto *ripple* y retardo
Cada sumador espera el acarreo del anterior: el resultado se estabiliza de derecha a izquierda como una ola ($t_1<t_2<t_3<t_4$).
- Cada compuerta: $t_p\approx10\text{ ns}$.
- El camino crítico del acarreo cruza **2 compuertas** por etapa (nivel AND + nivel OR).

$$\text{Retardo}\approx 2\cdot n\cdot10\text{ ns}$$

| Bits $n$ | Compuertas ($2n$) | Retardo |
| :-: | :-: | :-: |
| 4 | 8 | ≈ 80 ns |
| 16 | 32 | ≈ 320 ns |
| 32 | 64 | ≈ 640 ns |

---

## ⚠️ 4. Sumador/restador unificado y overflow

### 4.1. Restar = sumar el negativo
$A-B=A+(-B)$, con $-B$ = invertir $B$ y sumar 1.
**$6-3$:** $B=0011\to1100\to1101\,(-3)$; $0110+1101=\mathbf1\,0011$ → se descarta el `1` ⇒ $0011=3$.

### 4.2. XOR como «inversor controlado»
Una XOR por cada $B_i$ ($B_i\oplus M$) y $M$ conectada también al $C_{in}$ de la etapa 0:

| $M$ | Efecto sobre $B_i$ | Resultado |
| :-: | :--- | :--- |
| **0** (SUMA) | $B\oplus0=B$; $C_{in,0}=0$ | $A+B$ |
| **1** (RESTA) | $B\oplus1=B'$; $C_{in,0}=1$ aporta el «+1» | $A+B'+1=A-B$ |

### 4.3. Detección de overflow
Rango con signo en 4 bits: $-8$ a $+7$.
$$0101\,(+5)+0100\,(+4)=1001\,(-7)\ \times$$

> ⚠️ $C_{out}=1$ **no** implica error en números con signo: en $6-3=0110+1101=\mathbf1\,0011$ sale $C_{out}=1$ y el resultado $+3$ es correcto.

Hay overflow ⇔ el acarreo que **entra** a la última etapa es distinto del que **sale**:
$$\boxed{V=C_3\oplus C_4}$$

### 4.4. Demostración: $0101_2+0011_2$
| Etapa | $A_i$ | $B_i$ | $C_{in}$ | # de 1 | $S_i$ | $C_{out}$ |
| :-: | :-: | :-: | :-: | :-: | :-: | :-: |
| 0 | 1 | 1 | 0 | 2 | 0 | 1 ($C_1$) |
| 1 | 0 | 1 | 1 | 2 | 0 | 1 ($C_2$) |
| 2 | 1 | 0 | 1 | 2 | 0 | 1 ($C_3$) |
| 3 | 0 | 0 | 1 | 1 | 1 | 0 ($C_4$) |

$S_3S_2S_1S_0=1000_2$:
- **Sin signo:** $5+3=8$ ✔ correcto.
- **Con signo:** $+5+(+3)=-8$ ✘ **overflow**, pues $V=C_3\oplus C_4=1\oplus0=1$.

---

<div align="center">

⬅️ [📅 Semana 4](Semana4.md) &nbsp;|&nbsp; 🏠 [Índice](README.md)

</div>
