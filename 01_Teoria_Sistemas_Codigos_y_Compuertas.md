<p align="center"><img src="../../assets/logo_unl.png" alt="Universidad Nacional de Loja" width="280"></p>

🏠 [Inicio](../../README.md) › 🔌 [Unidad 1](../Unidad1.md) › 📅 [Semana 1](Semana1.md) › 📖 Teoría

---

# 📖 Semana 1 — Teoría: Sistemas Numéricos, Códigos y Compuertas

## ⚡ 1. Representación binaria y niveles de voltaje

### 1.1. ¿Por qué solo 0 y 1?
- **Estado `0`** (interruptor abierto): no pasa corriente, voltaje bajo.
- **Estado `1`** (interruptor cerrado): pasa corriente, voltaje alto.
- Un **transistor** es un interruptor controlado por voltaje. Con solo dos estados es fácil distinguirlos aunque la señal llegue con ruido.

### 1.2. Un «1» es un rango, no un voltaje exacto (familia 74LS)

| Nivel / Zona | Salida de compuerta A (promete) | Entrada de compuerta B (exige) | Observación |
| :--- | :--- | :--- | :--- |
| **Alto (`1`)** | $\ge 2{,}7\text{ V}$ hasta $5\text{ V}$ | $\ge 2{,}0\text{ V}$ hasta $5\text{ V}$ | Rango válido para un `1` |
| **Transición / indefinida** | $0{,}5$–$2{,}7\text{ V}$ (solo de paso) | $0{,}8$–$2{,}0\text{ V}$ | El chip no garantiza nada; típico de una **entrada sin conectar** |
| **Bajo (`0`)** | $0\text{ V}$ hasta $\le 0{,}5\text{ V}$ | $0\text{ V}$ hasta $\le 0{,}8\text{ V}$ | Rango válido para un `0` |

### 1.3. Margen de ruido
- $\text{Margen alto} = 2{,}7 - 2{,}0 = 0{,}7\text{ V}$ → un `1` puede perder hasta $0{,}7\text{ V}$ y se lee como `1`.
- $\text{Margen bajo} = 0{,}8 - 0{,}5 = 0{,}3\text{ V}$ → un `0` puede subir hasta $0{,}3\text{ V}$ y se lee como `0`.
- **Falla típica:** sale un `0` de $0{,}5\text{ V}$ + $0{,}4\text{ V}$ de ruido inducido por un motor $\rightarrow$ llegan $0{,}9\text{ V}$ $\rightarrow$ **zona indefinida** $\rightarrow$ error.

---

## 🔢 2. Sistemas numéricos y conversiones

### 2.1. Sistema posicional
Cada posición vale una potencia de la base. Ejemplo base 10: $555 = 5\cdot100 + 5\cdot10 + 5\cdot1$.
*Contraejemplo:* en números romanos ($\text{XV}$) la $\text{V}$ vale 5 sin importar su posición; por eso no es posicional y sumar es incómodo.

### 2.2. Binario (base 2)
- **Bit** = dígito binario. **Byte** = 8 bits.
- Pesos de 8 bits: $128 \mid 64 \mid 32 \mid 16 \mid 8 \mid 4 \mid 2 \mid 1$

| Peso | 8 | 4 | 2 | 1 |
| :--- | :-: | :-: | :-: | :-: |
| Dígito | 1 | 0 | 1 | 1 |

$$1011_2 = 8+0+2+1 = 11_{10}$$

### 2.3. Decimal → binario (dividir entre 2, leer restos de abajo hacia arriba)

| División | Cociente | Resto |
| :--- | :-: | :-: |
| $25\div2$ | 12 | **1** |
| $12\div2$ | 6 | **0** |
| $6\div2$ | 3 | **0** |
| $3\div2$ | 1 | **1** |
| $1\div2$ | 0 | **1** |

$$25_{10} = 11001_2 \quad (16+8+1=25\ \checkmark)$$

### 2.4. Hexadecimal y octal como abreviaturas del binario
> Si faltan bits al agrupar, se completa con ceros a la izquierda.

| Decimal | 10 | 11 | 12 | 13 | 14 | 15 |
| :-: | :-: | :-: | :-: | :-: | :-: | :-: |
| Hex | A | B | C | D | E | F |

- **Hexadecimal (base 16):** grupos de **4 bits** desde la derecha.
- **Octal (base 8):** grupos de **3 bits** desde la derecha.

**Ejemplo $157_{10} = 10011101_2$:**
- Hex: `1001`=9, `1101`=D → $\text{9D}_{16}$.
- Octal: `010` `011` `101` = 2, 3, 5 → $235_8$.

---

## ➕ 3. Aritmética binaria y números con signo

### 3.1. Suma binaria
$0+0=0$ · $0+1=1$ · $1+1=10_2$ (escribo 0, llevo 1) · $1+1+1=11_2$ (escribo 1, llevo 1)

```text
  Acarreo:   1 1 1
             1 0 1 1   (11)
           + 0 1 1 0   ( 6)
           ---------
           1 0 0 0 1   (17)
```

### 3.2. Negativos: complemento a dos
Analogía del cuentakilómetros: retroceder desde `000` da `999` ($-1$). En 4 bits, retroceder desde `0000` da `1111`, que **acordamos** como $-1$.

| Positivo | Dec | Negativo | Dec |
| :-: | :-: | :-: | :-: |
| `0000` | 0 | `1000` | −8 |
| `0001` | +1 | `1001` | −7 |
| `0010` | +2 | `1010` | −6 |
| `0011` | +3 | `1011` | −5 |
| `0100` | +4 | `1100` | −4 |
| `0101` | +5 | `1101` | −3 |
| `0110` | +6 | `1110` | −2 |
| `0111` | +7 | `1111` | −1 |

- Bit más significativo `0` → positivo; `1` → negativo.
- **Rangos:** 4 bits: $-8$ a $+7$ · 8 bits: $-128$ a $+127$.

### 3.3. Obtener el negativo: invertir y sumar 1
**$-3$ en 4 bits:** $+3=0011 \rightarrow$ invertir `1100` $\rightarrow$ $+1$ = `1101`.

**Resta como suma ($5-3=5+(-3)$):** $0101+1101=\mathbf{1}0010$ → se descarta el `1` sobrante → $0010=2$.

> **Ventaja:** no se necesita un restador; basta **el mismo sumador + inversores**.

### 3.4. Desbordamiento (overflow)
$$0111\,(+7)+0001\,(+1)=1000\,(-8)\ \times$$
Ejemplo en Java: `int x = 2147483647; x = x + 1;` → `x` vale `-2147483648`.
**Regla:** dos positivos que suman un negativo (o dos negativos que suman un positivo) indican overflow. *(Detección por hardware: Semana 4.)*

---

## 🔣 4. Códigos binarios

### 4.1. BCD (Binary-Coded Decimal)
Cada dígito decimal se codifica con **4 bits**. $157 \rightarrow$ `0001 0101 0111` (≠ binario natural `10011101`).
- **Uso:** displays de 7 segmentos, relojes, calculadoras, contadores.
- **Restricción:** `1010` (10) a `1111` (15) **no se usan**.

### 4.2. Código Gray
Entre valores vecinos **cambia un solo bit**.
- *Problema del binario natural:* de $3\rightarrow4$ (`011`→`100`) cambian 3 bits a la vez; un sensor puede leer valores falsos durante la transición.
- **Método del espejo:** copiar la lista al revés; anteponer `0` a la mitad superior y `1` a la inferior.

| Dec | 1 bit | 2 bits | 3 bits |
| :-: | :-: | :-: | :-: |
| 0 | `0` | `00` | `000` |
| 1 | `1` | `01` | `001` |
| 2 | — | `11` | `011` |
| 3 | — | `10` | `010` |
| 4 | — | — | `110` |
| 5 | — | — | `111` |
| 6 | — | — | `101` |
| 7 | — | — | `100` |

**Usos:** encoders/perillas rotativas y el orden `00, 01, 11, 10` del **Mapa de Karnaugh** (Semana 3).

### 4.3. ASCII
- 7 bits → 128 caracteres (letras, dígitos, puntuación, control).
- No incluye `ñ` ni tildes → para eso existe **UTF-8**.

| Carácter | H | O | L | A |
| :--- | :-: | :-: | :-: | :-: |
| Decimal | 72 | 79 | 76 | 65 |
| Hex | 0x48 | 0x4F | 0x4C | 0x41 |

La «A» se guarda como $65=01000001_2$.

---

## 🚪 5. Compuertas lógicas

### 5.1. Básicas
| Compuerta | Operación | Expresión |
| :--- | :--- | :---: |
| **NOT** | Invierte la entrada | $Y=A'$ |
| **AND** | `1` solo si **todas** las entradas son `1` | $Y=A\cdot B$ |
| **OR** | `1` si **al menos una** entrada es `1` | $Y=A+B$ |

### 5.2. Negadas (la burbuja = «negado»)
| Compuerta | Operación | Expresión |
| :--- | :--- | :---: |
| **NAND** | `0` solo si todas son `1` | $Y=(AB)'$ |
| **NOR** | `1` solo si ninguna es `1` | $Y=(A+B)'$ |

> **Universalidad:** con solo NAND (o solo NOR) se construye cualquier circuito (Semana 3).

### 5.3. Comparación
| Compuerta | Operación | Expresión |
| :--- | :--- | :---: |
| **XOR** | `1` si las entradas son **distintas** | $Y=A\oplus B$ |
| **XNOR** | `1` si las entradas son **iguales** | $Y=(A\oplus B)'$ |

*Ejemplo XOR:* luz de escalera con dos interruptores conmutados.

### 5.4. Tabla de verdad conjunta

| $A$ | $B$ | AND | OR | NAND | NOR | XOR | XNOR |
| :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: |
| 0 | 0 | 0 | 0 | 1 | 1 | 0 | 1 |
| 0 | 1 | 0 | 1 | 1 | 0 | 1 | 0 |
| 1 | 0 | 0 | 1 | 1 | 0 | 1 | 0 |
| 1 | 1 | 1 | 1 | 0 | 0 | 0 | 1 |

### 5.5. Normas de símbolos
- **ANSI/IEEE (formas distintivas):** la usada en este curso (si el simulador muestra rectángulos, cambiarlo en opciones).
- **IEC (rectángulos):**

| Compuerta | Símbolo IEC | Salida |
| :--- | :-: | :--- |
| NOT | `1` | con círculo |
| AND | `&` | directa |
| OR | `≥1` | directa |
| NAND | `&` | con círculo |
| NOR | `≥1` | con círculo |
| XOR | `=1` | directa |
| XNOR | `=1` | con círculo |

---

## 🔲 6. Circuitos integrados y datasheets

### 6.1. 74LS08 en encapsulado DIP-14
4 compuertas AND de 2 entradas. Dos filas de 7 pines; con la **muesca arriba**, el pin 1 está arriba a la izquierda y se cuenta en **sentido antihorario**.
- **Pin 14 = VCC (+5 V)** · **Pin 7 = GND (0 V)**

| Pin | Función | Pin | Función |
| :-: | :--- | :-: | :--- |
| 1 | 1A | 14 | VCC (+5 V) |
| 2 | 1B | 13 | 4B |
| 3 | 1Y | 12 | 4A |
| 4 | 2A | 11 | 4Y |
| 5 | 2B | 10 | 3B |
| 6 | 2Y | 9 | 3A |
| 7 | GND | 8 | 3Y |

### 6.2. Lectura del código `SN74LS08N`
| Parte | Significado |
| :-: | :--- |
| `SN` | Fabricante: Texas Instruments |
| `74` | Rango comercial ($0$–$70\,^\circ\text{C}$); `54` = militar |
| `LS` | Low-power Schottky (TTL); `HC` = CMOS de alta velocidad |
| `08` | Función: 4 AND de 2 entradas |
| `N` | Encapsulado DIP plástico |

**Consejo:** el `74HC08` tiene la misma función y distribución de pines (versión CMOS); sirve para las prácticas. Buscar `74LS08 datasheet` en sitios oficiales (`ti.com`, `onsemi.com`).

---

<div align="center">

⬅️ [📅 Semana 1](Semana1.md) &nbsp;|&nbsp; 🏠 [Índice](../../README.md) &nbsp;|&nbsp; [📅 Semana 2](../SEMANA%202/Semana2.md) ➡️

</div>
