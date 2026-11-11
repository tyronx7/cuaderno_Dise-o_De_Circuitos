<p align="center"><img src="../../assets/logo_unl.png" alt="Universidad Nacional de Loja" width="280"></p>

🏠 [Inicio](../../README.md) › 🔌 [Unidad 1](../Unidad1.md) › 📅 [Semana 2](Semana2.md) › 📖 Teoría

---

# 📖 Semana 2 — Teoría: Álgebra de Boole y Formas Canónicas

## 🔌 1. De un circuito a una expresión booleana

### 1.1. Recuperación de la APE 01: $Y=A\cdot B+C'$
$A$ y $B$ entran a una AND; $C$ pasa por un NOT; ambas salidas se unen en una OR.
- $A=1,B=0,C=0$: $0+1=1$.
- $A=1,B=1,C=1$: $1+0=1$.

### 1.2. Equivalencia circuito ↔ expresión
| Circuito | Expresión |
| :--- | :--- |
| Cable de entrada | Variable ($A,B,C$) |
| Compuerta AND | Producto ($\cdot$) |
| Compuerta OR | Suma ($+$) |
| Compuerta NOT | Complemento ($'$) |
| Salida | Valor evaluado |

**Precedencia:** $\text{NOT}\rightarrow\text{AND}\rightarrow\text{OR}$.

### 1.3. Ambigüedad del lenguaje natural
Enunciado: *«La sirena suena cuando se activan dos sensores»* (A, B, C).

| Lectura | Filas activas | Función | Problema |
| :--- | :--- | :--- | :--- |
| **Exactamente** dos | 3, 5, 6 | $\sum m(3,5,6)$ | Con `111` la sirena **no** suena |
| **Al menos** dos ✅ | 3, 5, 6, 7 | $\sum m(3,5,6,7)$ | Es la especificación real del cliente |

---

## 📜 2. Postulados y teoremas

### 2.1. Postulados de Huntington (conjunto $\{0,1\}$)
| Postulado | Forma OR | Forma AND (dual) |
| :--- | :--- | :--- |
| Identidad | $X+0=X$ | $X\cdot1=X$ |
| Complemento | $X+X'=1$ | $X\cdot X'=0$ |
| Conmutativa | $X+Y=Y+X$ | $XY=YX$ |
| Distributiva | $X(Y+Z)=XY+XZ$ | $X+YZ=(X+Y)(X+Z)$ |

**Principio de dualidad:** si en una identidad válida se intercambian $+\leftrightarrow\cdot$ y $0\leftrightarrow1$ (sin complementar variables), la nueva identidad también es válida.

### 2.2. Teoremas (tabla de consulta del EVA)
| Teorema | Forma OR | Forma AND (dual) |
| :--- | :--- | :--- |
| Elemento nulo | $X+1=1$ | $X\cdot0=0$ |
| Idempotencia | $X+X=X$ | $X\cdot X=X$ |
| Involución | $(X')'=X$ | $(X')'=X$ |
| Absorción | $X+XY=X$ | $X(X+Y)=X$ |
| Absorción del complemento | $X+X'Y=X+Y$ | $X(X'+Y)=XY$ |
| Adyacencia | $XY+XY'=X$ | $(X+Y)(X+Y')=X$ |
| Consenso | $XY+X'Z+YZ=XY+X'Z$ | $(X+Y)(X'+Z)(Y+Z)=(X+Y)(X'+Z)$ |

### 2.3. Demostración: «la distributiva que sorprende» $A+BC=(A+B)(A+C)$
1. $(A+B)(A+C)$
2. Distributiva: $=AA+AC+AB+BC$
3. Idempotencia: $=A+AC+AB+BC$
4. Factor común: $=A(1+C+B)+BC$
5. Elemento nulo: $=A+BC\ \blacksquare$

### 2.4. Leyes de De Morgan
> *«Rompe la barra (o abre el paréntesis negado) y cambia el operador.»*

$$(AB)'=A'+B' \qquad (A+B)'=A'B'$$
$$(A+B+C)'=A'B'C' \qquad (ABC)'=A'+B'+C'$$

**Actividad en parejas resuelta:**
1. $(ABC)'=A'+B'+C'$
2. $(A+B'C)'=A'\,(B'C)'=A'(B+C')$
3. $(A'B+C)'=(A'B)'\,C'=(A+B')C'$

*(Verificar evaluando una fila de la tabla de verdad.)*

---

## 📐 3. Formas canónicas

### 3.1. Minitérminos y maxitérminos (3 variables)
- **Minitérmino $m_i$:** producto con todas las variables; vale `1` en **una** fila. Variable en 1 → sin complemento; en 0 → complementada.
- **Maxitérmino $M_i$:** suma con todas las variables; vale `0` en **una** fila. Variable en 0 → sin complemento; en 1 → complementada.
- Relación: $M_i=(m_i)'$.

| $i$ | $ABC$ | $m_i$ (vale 1) | $M_i$ (vale 0) |
| :-: | :-: | :-: | :-: |
| 0 | `000` | $A'B'C'$ | $A+B+C$ |
| 1 | `001` | $A'B'C$ | $A+B+C'$ |
| 2 | `010` | $A'BC'$ | $A+B'+C$ |
| 3 | `011` | $A'BC$ | $A+B'+C'$ |
| 4 | `100` | $AB'C'$ | $A'+B+C$ |
| 5 | `101` | $AB'C$ | $A'+B+C'$ |
| 6 | `110` | $ABC'$ | $A'+B'+C$ |
| 7 | `111` | $ABC$ | $A'+B'+C'$ |

### 3.2. SOP canónica (Suma de Productos)
Filas con $F=1$ → sus minitérminos unidos con OR.
$$F=\sum m(3,5,6,7)=A'BC+AB'C+ABC'+ABC$$
Lectura: «$F=1$ si la entrada es la 3, o la 5, o la 6, o la 7».

### 3.3. POS canónica (Producto de Sumas)
Filas con $F=0$ → sus maxitérminos unidos con AND.
$$F=\prod M(0,1,2,4)=(A+B+C)(A+B+C')(A+B'+C)(A'+B+C)$$
Lectura: «$F=1$ si la entrada NO es la 0, NI la 1, NI la 2, NI la 4».
Los índices de $\prod M$ son exactamente los que **faltan** en $\sum m$.

### 3.4. ¿SOP o POS? Criterio: costo en hardware
| Situación | Conviene | Razón |
| :--- | :-: | :--- |
| Pocos `1` en la tabla | **SOP** | Menos términos |
| Pocos `0` en la tabla | **POS** | Menos términos |
| Solo NAND disponibles | **SOP** | Se convierte a NAND-NAND (`7400`) |
| Solo NOR disponibles | **POS** | Se convierte a NOR-NOR (`7402`) |

---

<div align="center">

⬅️ [📅 Semana 2](Semana2.md) &nbsp;|&nbsp; 🏠 [Índice](../../README.md) &nbsp;|&nbsp; [📅 Semana 3](../SEMANA%203/Semana3.md) ➡️

</div>
