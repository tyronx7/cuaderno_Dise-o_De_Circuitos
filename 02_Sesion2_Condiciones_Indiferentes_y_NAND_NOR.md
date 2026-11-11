
🏠 [Inicio](README.md) › 🔌 [Unidad 1](Unidad1.md) › 📅 [Semana 3](Semana3.md) › 🌐 Sesión 2

---

# 📖 Semana 3 · Sesión 2 — Condiciones Indiferentes y Universalidad NAND/NOR

## ❓ 1. Condiciones indiferentes (`X` / *don't care*)

### 1.1. Origen
Un dígito **BCD** usa 4 bits pero solo 10 de 16 combinaciones (`0000`–`1001`). Las 6 restantes (`1010`–`1111`, es decir 10–15) **nunca ocurren**; el circuito puede hacer «lo que más convenga». Notación:
$$F=\sum m(\dots)+d(\dots)$$

### 1.2. Reglas en el K-map
| Uso de la `X` | Cuándo |
| :--- | :--- |
| Como `1` | Si ayuda a **agrandar** un grupo |
| Como `0` | Si no ayuda: se ignora (ninguna `X` está obligada a cubrirse) |
| **Nunca sola** | Jamás un grupo formado solo por `X` (añade un término inútil) |

### 1.3. Ejemplo: detector de BCD $\ge5$
$$F(A,B,C,D)=\sum m(5,6,7,8,9)+d(10,11,12,13,14,15)$$

| $AB\backslash CD$ | `00` | `01` | `11` | `10` |
| :-: | :-: | :-: | :-: | :-: |
| **00** | 0 | 0 | 0 | 0 |
| **01** | 0 | **1** | **1** | **1** |
| **11** | X | X | X | X |
| **10** | **1** | **1** | X | X |

| Estrategia | Grupos | Expresión | Literales / CI |
| :--- | :--- | :--- | :--- |
| Sin usar `X` | $\{8,9\}\to AB'C'$; $\{5,7\}\to A'BD$; $\{6,7\}\to A'BC$ | $AB'C'+A'BD+A'BC$ | 9 lit. / **3 CI** (`7404+7411+7432`) |
| **Con `X`** | $\{8..15\}\to A$; $\{5,7,13,15\}\to BD$; $\{6,7,14,15\}\to BC$ | $\boxed{A+BD+BC}$ | 5 lit. / **2 CI** (`7408+7432`) |

---

## 🌐 2. Universalidad de NAND y NOR

### 2.1. NOT, AND y OR con una sola familia
| Función | Solo NAND | Solo NOR (dual) |
| :--- | :--- | :--- |
| **NOT** $A'$ | Unir entradas: $(A\cdot A)'=A'$ | Unir entradas: $(A+A)'=A'$ |
| **AND** $AB$ | NAND + NAND inversora: $((AB)')'=AB$ | NOR con entradas invertidas: $(A'+B')'=AB$ |
| **OR** $A+B$ | NAND con entradas invertidas: $(A'B')'=A+B$ | NOR + NOR inversora: $((A+B)')'=A+B$ |

**Arquitecturas de dos niveles:**
- SOP mínima ⇒ **NAND-NAND**
- POS mínima ⇒ **NOR-NOR**

### 2.2. SOP → NAND-NAND en 4 pasos (con $F=A+BD+BC$)
1. SOP mínima: $F=A+BD+BC$
2. Doble negación: $F=((A+BD+BC)')'$
3. De Morgan interna: $F=(A'\cdot(BD)'\cdot(BC)')'$
4. Cada paréntesis negado es una NAND: $F=\text{NAND}\big(A',\ \text{NAND}(B,D),\ \text{NAND}(B,C)\big)$

**Características:**
- El literal solo ($A$) entra **complementado** ($A'$) a la NAND de salida (se obtiene con una NAND como inversor).
- Dos niveles ⇒ **mismo retardo** que la red AND-OR original.
- Una sola familia: `7400` (2 entradas) + `7410` (3 entradas) ⇒ **2 CI**.

### 2.3. Piensa, Pareja, Comparte — respuestas
**1. ¿Por qué $A$ entra complementada y $B,C,D$ no?**
Tras De Morgan, $(BD)'$ y $(BC)'$ ya son NAND de nivel 1 (bloques negados). El literal aislado $A$ solo recibe una negación y pasa a $A'$; si entrara sin negar, la NAND de salida entregaría $A'$ en lugar de $A$.

**2. Convertir $F=A'C'+BC$ (taller T2) a NAND-NAND:**
$$F=((A'C'+BC)')'=\big((A'C')'\cdot(BC)'\big)'$$

| # | Función | Compuertas |
| :-: | :--- | :-: |
| 1 | Inversor para $A'$ | 1 |
| 2 | Inversor para $C'$ | 1 |
| 3 | Nivel 1: $(A'C')'$ | 1 |
| 4 | Nivel 1: $(BC)'$ | 1 |
| 5 | Nivel 2 (salida) | 1 |
| | **Total** | **5 NAND ⇒ 2 chips `7400`** (4 NAND por chip) |

---

<div align="center">

⬅️ [🗺️ Sesión 1](01_Sesion1_Mapas_de_Karnaugh_SOP_POS.md) &nbsp;|&nbsp; 🏠 [Índice](README.md) &nbsp;|&nbsp; [📅 Semana 4](Semana4.md) ➡️

</div>
