

🏠 [Inicio](README.md) › 🔌 [Unidad 1](Unidad1.md) › 📅 Semana 4

---

# 📅 Semana 4 — Circuitos aritméticos, comparadores y ALU

**Objetivo:** diseñar con el procedimiento clásico un sumador de 4 bits, adaptarlo para restar, detectar *overflow*, comparar magnitudes y estimar el retardo de propagación. Aporta a **R1**, es el corazón de la **ALU** y prepara la práctica **APE 04**.

## 📂 Contenido
| Archivo | Descripción |
| :--- | :--- |
| [01_Teoria_Circuitos_Aritmeticos_Resta_y_Overflow.md](01_Teoria_Circuitos_Aritmeticos_Resta_y_Overflow.md) | Semisumador, sumador completo, ripple-carry, resta con XOR, overflow |

## 📝 Resumen
- $S$ = bit que se escribe; $C$ = acarreo.
- Semisumador: $S=A\oplus B$, $C=AB$ (solo columna 0).
- Sumador completo: $S=A\oplus B\oplus C_{in}$; $C_{out}=AB+AC_{in}+BC_{in}$.
- Ripple-carry: retardo $\approx 2\cdot n\cdot10\text{ ns}$.
- Resta: con $M=1$, XOR invierten $B$ y $C_{in,0}=1$.
- Overflow: $V=C_3\oplus C_4$ (no $C_{out}$).

---

<div align="center">

⬅️ [🌐 Sesión 2](SEMANA%203/02_Sesion2_Condiciones_Indiferentes_y_NAND_NOR.md) &nbsp;|&nbsp; 🏠 [Índice](README.md) &nbsp;|&nbsp; [📖 Teoría](01_Teoria_Circuitos_Aritmeticos_Resta_y_Overflow.md) ➡️

</div>
