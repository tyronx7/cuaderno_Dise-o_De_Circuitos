

🏠 [Inicio](README.md) › 🔌 [Unidad 1](Unidad1.md) › 📅 [Semana 3](Semana3.md) › 🗺️ Sesión 1

---

# 📖 Semana 3 · Sesión 1 — Mapas de Karnaugh (SOP y POS, 3 y 4 variables)

**Objetivo:** obtener la expresión mínima SOP y POS de funciones de 3 y 4 variables, justificando cada agrupación.

## 🧱 1. Fundamento

**Reto inicial:** dada $F(A,B,C)=\sum m(0,2,4,5,6)$, ¿cómo saber con certeza que el resultado algebraico es mínimo? → el álgebra no lo garantiza visualmente.

**Teorema de adyacencia:** $XY+XY'=X$. Si dos minitérminos difieren en **una sola variable** (p. ej. $m_4=AB'C'$ y $m_5=AB'C$), esa variable desaparece: queda $AB'$ (`10-`).

**¿Por qué el orden `00, 01, 11, 10`?**
- Binario natural: de `01`→`10` y de `11`→`00` cambian 2 bits.
- **Gray**: entre cada celda y la siguiente (incluso de la última a la primera) cambia **1 bit** ⇒ adyacencia lógica = adyacencia visual.

## 🏗️ 2. Estructura de los mapas

### 2.1. Tres variables ($A$ filas, $BC$ columnas) — 8 celdas
| $A\backslash BC$ | `00` | `01` | `11` | `10` |
| :-: | :-: | :-: | :-: | :-: |
| **0** | $m_0$ | $m_1$ | $m_3$ | $m_2$ |
| **1** | $m_4$ | $m_5$ | $m_7$ | $m_6$ |

### 2.2. Cuatro variables ($AB$ filas, $CD$ columnas) — 16 celdas
| $AB\backslash CD$ | `00` | `01` | `11` | `10` |
| :-: | :-: | :-: | :-: | :-: |
| **00** | $m_0$ | $m_1$ | $m_3$ | $m_2$ |
| **01** | $m_4$ | $m_5$ | $m_7$ | $m_6$ |
| **11** | $m_{12}$ | $m_{13}$ | $m_{15}$ | $m_{14}$ |
| **10** | $m_8$ | $m_9$ | $m_{11}$ | $m_{10}$ |

## 📏 3. Reglas de agrupación
1. **Tamaño $2^k$** (1, 2, 4, 8, 16); un grupo de $2^k$ celdas elimina $k$ variables.
2. **Rectangular o cuadrado**: sin «L» ni diagonales.
3. **Lo más grande posible** ⇒ menos literales.
4. **Toroide**: bordes superior/inferior e izquierdo/derecho son vecinos; **las 4 esquinas** son vecinas ($m_0,m_2,m_8,m_{10}\to B'D'$).

### 3.1. Resolución del reto: $F=\sum m(0,2,4,5,6)$
| $A\backslash BC$ | `00` | `01` | `11` | `10` |
| :-: | :-: | :-: | :-: | :-: |
| **0** | **1** | 0 | 0 | **1** |
| **1** | **1** | **1** | 0 | **1** |

- Grupo de 4 ($m_0,m_2,m_4,m_6$, enrollando bordes): $C=0$ constante ⇒ $C'$.
- Grupo de 2 ($m_4,m_5$): $A=1,B=0$ ⇒ $AB'$.

$$F=C'+AB'$$

### 3.2. Implicantes
| Concepto | Definición |
| :--- | :--- |
| **Implicante** | Grupo válido de `1` (o `X`) |
| **Implicante primo** | No se puede agrandar más (no está contenido en otro mayor) |
| **Primo esencial** | Único primo que cubre algún `1`; **debe** estar en la solución |

$$\text{Solución mínima}=\text{Primos esenciales}+\text{Primos mínimos para cubrir el resto}$$

## 🧭 4. Procedimiento SOP y POS (4 variables)

### 4.1. Cinco pasos (SOP)
1. Colocar `1`, `0` y `X`.
2. Agrupar formando implicantes primos.
3. Elegir primero los primos esenciales.
4. Cubrir los `1` restantes con la menor cantidad de grupos.
5. Leer un producto por grupo: constante en 1 → sin complementar; constante en 0 → complementada; variable que cambia → desaparece.

### 4.2. Ejemplo SOP: $F=\sum m(0,1,2,5,8,9,10)$
| $AB\backslash CD$ | `00` | `01` | `11` | `10` |
| :-: | :-: | :-: | :-: | :-: |
| **00** | **1** | **1** | 0 | **1** |
| **01** | 0 | **1** | 0 | 0 |
| **11** | 0 | 0 | 0 | 0 |
| **10** | **1** | **1** | 0 | **1** |

- $\{m_0,m_1,m_8,m_9\}\to B'C'$
- $\{m_0,m_2,m_8,m_{10}\}\to B'D'$ (esquinas)
- $\{m_1,m_5\}\to A'C'D$

$$F_{SOP}=B'C'+B'D'+A'C'D\quad(\text{3 términos, 7 literales})$$

### 4.3. Misma función en POS (se agrupan los `0`, lectura invertida)
Lectura POS: constante en 0 → sin complementar; constante en 1 → complementada.
Ceros: $M_3,M_4,M_6,M_7,M_{11},M_{12},M_{13},M_{14},M_{15}$.

- Fila $AB=11$ → $(A'+B')$
- Columna $CD=11$ → $(C'+D')$
- $\{M_4,M_6,M_{12},M_{14}\}$ ($B=1,D=0$) → $(B'+D)$

$$F_{POS}=(A'+B')(B'+D)(C'+D')\quad(\text{3 factores, 6 literales})$$

### 4.4. Comparación de costo
| Forma | Expresión | Literales |
| :-: | :--- | :-: |
| SOP | $B'C'+B'D'+A'C'D$ | 7 |
| POS | $(A'+B')(B'+D)(C'+D')$ | **6** |

**Criterio:** construir ambas y elegir la de menor costo (literales, compuertas, CI).

---

<div align="center">

⬅️ [📅 Semana 3](Semana3.md) &nbsp;|&nbsp; 🏠 [Índice](README.md) &nbsp;|&nbsp; [🌐 Sesión 2](02_Sesion2_Condiciones_Indiferentes_y_NAND_NOR.md) ➡️

</div>
